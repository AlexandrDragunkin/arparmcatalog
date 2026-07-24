# Аудит DUO-2: `pedestal_pivot_duo/boad.py` ↔ SParametrica

> Разбор файла `Proto/LeroyProtoLIB/pedestal_pivot_duo/boad.py` (класс
> `UnitDUO`, «Тумба DUO-2») с UML-диаграммами и полной картой зависимостей, плюс
> сравнение с параллельным треком SParametrica на ветках `feature/CA-46516` →
> `feature/duo2-doors-fillings`. Оракул — read-only; правки не предлагаются,
> только наблюдения. Составлено 2026-07-24.

## 0. Резюме для занятых (TL;DR)

1. **`boad.py` — это ТУМБА (педестал), а не шкаф.** K3-точка входа —
   `P_PedestalDUO_2.py` (`boad.main(default_name_proto="РШ Тумба DUO-2")`). У него
   есть близнец-**шкаф** `cab_duo_two.py`, который зовёт `P_CabDUO_2.py` оракула.
   Оба — `class UnitDUO(uFurnObject.FurnObject)` с двумя нишами; различаются
   корпусом, цоколем и мягким сиденьем (см. §5).
2. **Параллельный трек SParametrica DUO-2 целится в ШКАФ**
   (`cab_duo_two.py` / `shell.ShellPivotDuo`), **а не в тумбу `boad.py`**. Значит
   прямого «двойника» у `boad.py` в новой архитектуре пока нет — есть двойник его
   родственника-шкафа. Общий у них паттерн `UnitDUO`/две ниши/`spacing_calculate`.
3. **Архитектурный контраст — главное содержание аудита.**
   `boad.py` — **монолит**: Model (что построить), View (как нарисовать в K3) и
   Controller (чтение прототипа, номенклатура) слиты в одном `FurnObject`-классе,
   `import k3` и `priceinfo` — прямо в теле. SParametrica DUO-2 — **MVC**:
   `arline_geometry` (Model, ноль K3) → `renderer_k3` (View) → `controller`
   (proto → `DuoParams` + номенклатура). Правило трека: «повторяем оракул 1:1,
   меняется только раскладка по слоям».
4. **Статус кода DUO-2 в SParametrica:** ключевые файлы **НЕ закоммичены**
   (`?? P_CabDUO_2.py`, `?? controller/duo_controller.py`,
   `?? tests/test_duo_controller.py`; `renderer_k3/__init__.py` — `M`). Чистая
   модель (`build_duo_parts`, `DuoParams`, тесты пакета) — **закоммичена** на
   ветке. То есть Model готова и в git, а K3-обвязка DUO живёт только в рабочем
   дереве (см. §7).
5. **Находки аудита в оракуле** (read-only, не трогаем): вероятный баг
   `count_box2` (читает `_blockbox1`), мутабельный дефолт `nums_fas=[1,]` +
   `.append`, гомоглиф-идентификатор `calculatе_shell_wpos1` (кириллическая
   `е`). Подробно — §8.

## 1. Место `boad.py` в семействе DUO

`boad.py` — один из ветвей «распашного» (pivot) семейства DUO. Каждый
K3-макрос `P_*.py` — тонкая точка входа, которая перезагружает модуль-строитель
и зовёт его `main()`.

| K3-макрос (точка входа) | Модуль-строитель | Класс | `_elemname` | Корпус |
|---|---|---|---|---|
| `P_PedestalDUO_2.py` | **`pedestal_pivot_duo/boad.py`** | `UnitDUO` | «Тумба DUO-2» | `based_shell.ShellBased` (плоский «Базовый корпус») |
| `P_CabDUO_2.py` | `cab_duo_two.py` | `UnitDUO` | «Шкаф DUO-2» | `shell.ShellPivotDuo` |
| `P_AntresolDUO_2.py` | `shell_pivot_duo_commode`/`cab_*` | — | Антресоль | — |
| `CommodeDUO_2L/R.py` | комодные строители | — | Комод | — |

**Родословная.** `boad.py` и функция `build_aboad` — прямые наследники старых
K3-макросов на языке K3-Script: `Proto/boad_coplaner.mac`,
`Proto/AKitchen/aboad5V.mac` (семейство `aboad`/`boad`). Те же понятия
(`h_dsp_left/right/mid`, «средняя стойка», `MakePan`, `DbVar("СторОткр")`)
переехали в Python-класс — это объясняет «телеграфный» стиль имён и обилие
прямых `k3.*`/`dbsetvar`.

## 2. Как работает `boad.py` — детальный разбор

### 2.1. Входные данные: `RSH_DUO` + `params_adapter`

`RSH_DUO` (`@dataclass`, `boad.py:375`) — контейнер параметров тумбы: габариты
`width/height/depth`, `w_small` (ширина секции с полками), материалы
(`prmater/bandtypereal/fsmater1/…`), крепёж (`fix_eck/fix_mf/fix_konf/…`,
целый комплект `GoodsID`), число ящиков `blockbox1/2`, `module`, `zsk`, а также
блок **мягкого сиденья** (`softseats/seatfabrics/basematseat/seat_bk_gap/
basetrimgab`) — уникальный для тумбы.

`RSH_DUO.params_adapter(protopars)` (`boad.py:427`) — мост «сырой прототип →
типизированные поля». Читает `_w/_h/_d`, `_openside1/2`, `_doubledoor1/2`,
цоколь `_hcok` (→ `ShellSocle.validate_cokle_height`: высота 1–39 запрещена,
снапится к 70; 0 = без цоколя), материалы через `functions.get_*`
(`get_prmater→MatId`, `get_bandtypereal→mBandId`, `get_bpmater→mHdfId`, …) и
`RSH_Complect.propbuilder` (комплект крепежа). В конце — **побочные эффекты в
K3**: `k3.dbsetvar("prmater"/"band"/"dvpMaterial"/"HDFConnector", …)`. Это и есть
«Controller внутри Model» — резолв номенклатуры и запись в K3 смешаны со сборкой.

### 2.2. Жизненный цикл `FurnObject`

`UnitDUO` наследует `engine.uFurnObject.FurnObject` (`:568`) — «богатую» базу с
деревом композиции, которая, в свою очередь, стоит на тонкой исходной базе
`engine.FurnObject.FurnObject` (`:87`) и подмешивает `Parent` (дерево
родитель/потомок) и `PrototypeObject` (чтение параметров прототипа).

Ключ: **`build_aboad` НЕ вызывает `Build()`** — он зовёт `Make()` напрямую, затем
`Draw()`. То есть дерево композиции материализуется сразу в `self.objects`, а
`Draw` его отрисовывает. (Общий путь `DrawUnit()` = `Build()`+`Draw()` тут не
используется.)

| Метод | Где | Роль |
|---|---|---|
| `set_proto_pars(proto_id)` | `PrototypeObject`, зовётся в `__init__` (`boad.py:524`) | читает записи прототипа, переименовывает (`шир`→`w`, добавляет `_`), `setattr` на объект |
| `Make()` | `boad.py:1051` | строит корпус + ниши, кладёт в `self.objects`, возвращает список |
| `Draw(ispostdraw=True)` | `boad.py:1219` → `super().Draw()` | рисует каждого потомка, группирует в один K3-объект, вешает атрибуты, зовёт `PostDraw` |
| `_AssignAttributes()` | `boad.py:1257` (цепочка `super()`) | `XUnit/YUnit/ZUnit`, `_SetProtoParams`, скрейч, потом базовые атрибуты |
| `_AssignScratchAttributes()` | `boad.py:1240` | копирует `FasadPar` и `PickleData` на объект |
| `PostDraw()` | `boad.py:1232` | `PostHingeMoverPV1.execute()` — сдвигает петли, налезшие на полки |

### 2.3. `Make()` — что и в каком порядке строится

`Make()` (`boad.py:1051`) — «сценарий сборки»:

1. Устанавливает пост-процессор петель: `SetHingeMoverProc(PostHingeMoverPV1(self))`.
2. `self.shell = make_shell()` → `ShellBased.factory(...)` (плоский «Базовый
   корпус»), настраивает `d_top`, `wpos1 = calculatе_shell_wpos1(self)`,
   `is_carga` (высота ≥ 600).
3. Считает свесы: `self.spacing1/2 = spacing_calculate(self, countfas=2, top=-2)`.
4. **Секция 1 (правая):** опционально `Niche_Box_Pivot` (блок ящиков, если
   `height≥1000 и count_box1`), затем `Niche_SotShelf` (сотовые полки),
   `Niche_shelves` (съёмные полки, `count = _polkstd1`, Z-позиции из
   `_polkstd1h{i}`), вешалки `cls_veshalo`.
5. Позиционирование через `put_position_and_forma(niche)` — вызывает адаптеры
   габарита/позиции (см. 2.4) и `SetGabs`/`SetPosition`.
6. **Секция 2 (левая):** симметрично (`Niche_Box_Pivot`, `Niche_shelves`,
   `Niche_SotShelf`), плюс продольные/поперечные вешалки.
7. При `height≥1000` включает навесы (`is_naves`).
8. **Мягкое сиденье:** `SoftSeats.factory(unit_inst=self.inst_data)`.
9. **Двери-фасады:** `pivot_niche1 = createNiche1(...)`, `pivot_niche2 =
   createNiche2(...)` — это ниши, которые несут двери (через `setNicheFasFromIndex`).
10. Возвращает `self.objects`.

### 2.4. Ниши: фабрика + два адаптера + `spacing_calculate`

Все ниши-наполнители (`Niche_shelves`, `Niche_Box_Pivot`, `Niche_SotShelf`,
`NicheVesh`, `NicheVeshProd`) строятся **фабрикой** и позиционируются **не сами**,
а внешними адаптерами хозяина-изделия:

- **`Niche_*.factory(shell_inst, positionniche, count, prev_niche=…)`** — создаёт
  нишу, резолвит её прототип, и **внедряет в неё адаптеры родителя**:
  `niche.set_gab_niche_adapter(shell_inst.gab_niche_adapter)` и
  `set_symmetry_position_adapter(shell_inst.symmetry_niche_position_adapter)`.
- **`gab_niche_adapter(niche)`** (`boad.py:765`) — по типу ниши (`isinstance`) и
  `positionniche` (`STANDART_LEFT/RIGHT`, `LEFT/RIGHT/MIDLE`) считает `Gabs`
  (ширину из `width_niche_calculate`, глубину `depth - y_niche - cut_inside`,
  высоту от габарита корпуса минус цоколь/панели).
- **`symmetry_niche_position_adapter(niche)`** (`boad.py:923`) — считает `Point`
  (позицию xyz), с учётом `wpos1` (X средней стойки) и симметрии.
- **`put_position_and_forma(niche)`** (`boad.py:759`) — связывает оба:
  `niche.SetGabs(*gab())`, `niche.SetPosition(*point())`.

**`spacing_calculate`** (`piv_param_nichenumber_utilites.py:437`) — сердце расчёта
свесов фасада; `DOUBLESPACING = 4`:

```python
spacing.doublespacing = (width - (round(width/countfas) - 4) * countfas) / countfas
spacing.fullside      = h_dsp - doublespacing/2      # накладная (14 при h_dsp=16)
spacing.halpside      = (h_dsp - doublespacing)/2    # полунакладная (6)
# видимый зазор по контуру = doublespacing/2         # (2 мм)
```

Смысл: подгонка бокового зазора так, чтобы **ширина фасада была целой**;
`fullside` — свес для накладной петли, `halpside` — для полунакладной (створка
на общей стойке).

### 2.5. `build_aboad` — финализация

`build_aboad(data_proto)` (`boad.py:1274`) — оркестратор одного построения:

1. `proto_id = core_k.proto_func._get_proto_id(data_proto)`;
2. `unit = UnitDUO(id_prototype=proto_id, inst_data=RSH_DUO())`;
3. подменяет процедуры ниш на модульные (`SetNiche1/createNiche1/…`) и
   `set_calculatе_shell_wpos1(calculatе_shell_wpos1)` — **изделие настраивается
   инъекцией функций**, не только наследованием;
4. пишет скрейч типов фасадов `ТипФас1..6` (`Scratch.add/write_scratch`);
5. `unit.Make()` → `unit.Draw()`;
6. вешает GUID (`mAttribute.guid_attr.attach`), `k3.fixing`/`k3.holes` (крепёж и
   отверстия), и **переселектит** готовый объект по GUID-фильтру
   (`core_k.select.selbyattr`).

### 2.6. Модуль-уровневые функции (инъектируемые)

`SetNiche1/2`, `createNiche1/2`, `setDoorNiche/createDoorNiche` — задают/создают
двери-ниши (тип заполнения `SHELVES`, зазоры из `spacing_calculate`,
`setNicheFasFromIndex` вешает фасад/петлю/ручку). `width_niche_calculate`,
`w_small_inst_data_adapter`, `real_width_calc` (`@lru_cache`) — ширины секций по
числу фасадов. `calculatе_shell_wpos1` — X средней стойки-делителя
(`= xp + shell.h_dsp`).

## 3. UML-диаграммы

### 3.1. Диаграмма классов — `UnitDUO` и окружение

```{mermaid}
classDiagram
    direction LR

    class FurnObject_thin {
      +Make() list
      +Draw()
      +SetFurnType()
      +SetElemName()
      +_AssignAttributes()
    }
    class uFurnObject {
      +Build()
      +Draw(ispostdraw)
      +get_objects()
      +_DrawAllObjWithCinema()
    }
    class Parent
    class PrototypeObject {
      +set_proto_pars(id)
    }

    class UnitDUO {
      +shell_cls = ShellBased
      +niches_count = 2
      +inst_data: RSH_DUO
      +objects: list
      +Make() list
      +Draw()
      +PostDraw()
      +make_shell()
      +gab_niche_adapter(niche) Gabs
      +symmetry_niche_position_adapter(niche) Point
      +put_position_and_forma(niche)
      +getDoorsNiche()
      +getShelfesNiche()
    }
    class RSH_DUO {
      +width_height_depth
      +prmater_bandtypereal
      +blockbox1_2_module_zsk
      +softseats_seatfabrics
      +params_adapter(protopars)
    }

    FurnObject_thin <|-- uFurnObject
    Parent <|-- uFurnObject
    PrototypeObject <|-- uFurnObject
    uFurnObject <|-- UnitDUO
    UnitDUO *-- RSH_DUO : inst_data

    class ShellBased { +factory() }
    class Niche { +factory() +SetGabs() +SetPosition() }
    class Niche_shelves
    class Niche_Box_Pivot
    class Niche_SotShelf
    class NicheVesh
    class NicheVeshProd
    class SoftSeats { +factory() }
    class PostHingeMoverPV1 { +execute() }

    Niche <|-- Niche_shelves
    Niche <|-- Niche_Box_Pivot
    Niche <|-- Niche_SotShelf
    Niche <|-- NicheVesh
    Niche <|-- NicheVeshProd

    UnitDUO --> ShellBased : shell
    UnitDUO --> Niche_shelves : niche_shelves1_2
    UnitDUO --> Niche_Box_Pivot : niche_box_1_2
    UnitDUO --> Niche_SotShelf : niche_sotshelves
    UnitDUO --> NicheVeshProd : niche_prod_vesh
    UnitDUO --> SoftSeats : softseats
    UnitDUO --> PostHingeMoverPV1 : hinge_mover
```

### 3.2. Диаграмма последовательности — построение тумбы

```{mermaid}
sequenceDiagram
    autonumber
    participant P as P_PedestalDUO_2.py
    participant BA as build_aboad()
    participant U as UnitDUO
    participant RSH as RSH_DUO
    participant SH as ShellBased
    participant N as Niche_*
    participant HM as PostHingeMoverPV1
    participant K3 as k3 API

    P->>BA: boad.main() build_aboad(DATA_PROTO)
    BA->>K3: _get_proto_id(data_proto)
    BA->>U: __init__(proto_id, RSH_DUO())
    U->>U: set_proto_pars(proto_id)
    U->>RSH: params_adapter(self)
    RSH->>K3: priceinfo + dbsetvar(prmater/band)
    BA->>U: Make()
    U->>SH: make_shell() = ShellBased.factory()
    U->>N: Niche_*.factory(shell, positionniche, count)
    U->>U: put_position_and_forma(niche)
    Note over U: gab_niche_adapter + symmetry_position_adapter
    U->>U: createNiche1/2 setNicheFasFromIndex (двери/петли)
    U-->>BA: self.objects
    BA->>U: Draw()
    U->>U: _DrawAllObjWithCinema(objects)
    loop каждый потомок
        U->>N: child.Draw() own K3 group
    end
    U->>K3: group + _AssignAttributes + PickleData
    U->>HM: PostDraw() execute()
    HM->>K3: сдвиг петель с полок
    BA->>K3: fixing/holes(k_create, k_all)
    BA->>K3: selbyattr(GUID) skf
    BA-->>P: skf (готовый объект)
```

### 3.3. Диаграмма зависимостей (импорты `boad.py`)

```{mermaid}
graph TD
    boad["pedestal_pivot_duo/boad.py<br/>UnitDUO / RSH_DUO / build_aboad"]

    subgraph framework["Каркас (engine / базы)"]
        uFO["engine.uFurnObject.FurnObject"]
        bb["based_build<br/>Panel Door Constants Accessory"]
    end
    subgraph corek["core_k (K3-обвязка)"]
        pf["proto_func.DataProto / _get_proto_id"]
        rd["rdnomenclature.priceinfo"]
        sel["select.selbyattr"]
    end
    subgraph oracle["LeroyProtoLIB (родные модули)"]
        shellmod["based_shell.shell<br/>ShellBased RSH_ShellBased ClosedInterval"]
        shellpy["shell.py<br/>Niche RSH_Complect ShellSocle limit_checkers"]
        details["details.SoftSeats"]
        piv["piv_param_nichenumber_utilites<br/>spacing_calculate get_param_from_nichenumber"]
        funcs["functions.get_prmater/bandtypereal"]
        hinge["utilites_hinge_mover.PostHingeMoverPV1"]
        consts["constants.NichePosition"]
        types["objecttypes.NomenclatureID/GoodsID"]
        ent["entityes.common.Gabs/Point"]
        exc["exceptions_shell.NicheShelvesException"]
        limits["pedestal_pivot_duo.limits<br/>WIDTH_DIAPASON LimitSize"]
    end
    subgraph k3side["K3-инфраструктура"]
        k3["k3 (движок)"]
        scratch["Scratch.scrfas / GlobalBoxCount"]
        mattr["mAttribute.guid_attr / pickle_data_attr"]
        userb["user_build.uConstantsDeclare / Niche_user"]
    end

    boad --> uFO
    boad --> bb
    boad --> pf & rd & sel
    boad --> shellmod & shellpy & details & piv & funcs & hinge & consts & types & ent & exc & limits
    boad --> k3 & scratch & mattr & userb
```

### 3.4. Паттерн «фабрика ниши + инъекция адаптеров хозяина»

```{mermaid}
flowchart LR
    A["UnitDUO.Make()"] -->|"factory(shell, position, count)"| B["Niche_shelves"]
    B -->|"set_gab_niche_adapter"| C["UnitDUO.gab_niche_adapter"]
    B -->|"set_symmetry_position_adapter"| D["UnitDUO.symmetry_niche_position_adapter"]
    A -->|"put_position_and_forma(niche)"| E{"адаптеры"}
    E -->|"Gabs"| C
    E -->|"Point"| D
    E --> F["niche.SetGabs / SetPosition"]
```

## 4. Импорты `boad.py` — полная карта зависимостей

| Импорт | Файл | Что берётся | Роль |
|---|---|---|---|
| `k3` | движок | всё K3 API | построение, `dbsetvar`, `fixing`, `holes`, атрибуты |
| `based_build` | `based_build/` | `Panel.Panel`, `Door.Door`, `Constants`, `Accessory` | листовые детали (панель/дверь/фурнитура), константы `FILLTYPE/OPENSIDE/TEXTURE` |
| `core_k` | `core_k/` | `proto_func.DataProto`/`_get_proto_id`, `rdnomenclature.priceinfo`, `select.selbyattr`, `structobject.GetCntObjS` | резолв прототипа, номенклатура, селекция сцены |
| `engine.uFurnObject` | `engine/uFurnObject.py:568` | `FurnObject`, `Sostav`, `guid_generator` | базовый класс изделия, реестр состава |
| `based_shell.shell` (`_shell`) | `based_shell/shell.py` | `ShellBased`, `RSH_ShellBased`, `ClosedInterval` | плоский «Базовый корпус» тумбы + интервалы полок |
| `shell` | `shell.py` (415 КБ) | `Niche`, `Niche_Box_Pivot`, `Niche_SotShelf`, `Niche_shelves`, `NicheVesh`, `NicheVeshProd`, `RSH_Complect`, `ShellSocle`, `limit_size_box_checker_*` | иерархия ниш-наполнителей, комплект крепежа, цоколь, проверки размеров |
| `details` | `details.py` | `SoftSeats` | мягкое сиденье тумбы |
| `piv_param_nichenumber_utilites` | одноимённый | `spacing_calculate`, `get_param_from_nichenumber`, `setNicheFasFromIndex`, `get_fasrtype_from_box` | свесы фасада, чтение `_openside{i}`, навеска фасада |
| `functions` | `functions.py` | `get_prmater/bandtypereal/bpmater/bp_connector/id_leg/id_naves/id_fetr_leg/fix_podp/rshcomplect` | цвет-запись → id материалов/крепежа |
| `utilites_hinge_mover` | одноимённый | `PostHingeMoverPV1` | пост-сдвиг петель с полок |
| `constants` | `constants.py` | `NichePosition`, `NicheVeshPosition` | перечисления позиций ниш |
| `objecttypes` | `objecttypes.py` | `NomenclatureID`, `GoodsID` | `NewType(int)` для читаемых аннотаций |
| `entityes.common` | `entityes/common.py` | `Gabs`, `Point` | value-объекты габарита/точки (`__call__ → tuple`) |
| `exceptions_shell` | `exceptions_shell.py` | `NicheShelvesException` | ошибка недопустимой позиции ниши |
| `.limits` | `pedestal_pivot_duo/limits.py` | `WIDTH_DIAPASON`, `LimitSize` | диапазоны ширины и лимиты ниш/ящиков |
| `Scratch` | `Scratch/` | `scrfas.ScrFasadGet`, `add_scratch`, `write_scratch`, `GlobalBoxCount` | скрейч-наборы (типы фасадов, счётчик ящиков) |
| `mAttribute` | `mAttribute/` | `guid_attr`, `pickle_data_attr`, `fromdelete` | типизированные атрибуты K3-объекта |
| `user_build` | `user_build/` | `uConstantsDeclare`, `Niche_user.StraightNiche` | пользовательские константы `FILLTYPE/OPENSIDE`, прямая ниша |
| `k3_widgets.alternative` | одноимённый | `ErrMsgBox` | диалоги предупреждений |
| `loguru`, `pimp` | внешние | `logger`, `reload` | лог + горячая перезагрузка в DEBUG |

## 5. `boad.py` (тумба) vs `cab_duo_two.py` (шкаф)

Оба — `class UnitDUO(uFurnObject.FurnObject)`, `niches_count = 2`, две
распашные ниши через `createNiche1/2`+`SetNiche1/2`, тот же слой
`spacing_calculate`/`setNicheFasFromIndex`/`get_param_from_nichenumber`, те же
полки/сотовые/вешалки, тот же `PostHingeMoverPV1`, тот же снап цоколя. Различия:

| Аспект | `boad.py` (тумба) | `cab_duo_two.py` (шкаф) |
|---|---|---|
| `_elemname` | «Тумба DUO-2» | «Шкаф DUO-2» |
| Dataclass | `RSH_DUO` (`h_cok=70`) | `RSH_Duo` |
| Корпус | `based_shell.ShellBased` + `RSH_ShellBased` (плоский «Базовый корпус», логика `d_top/w_top/cut_inside/is_carga/is_naves`) | `shell.ShellPivotDuo` + `RSH_ShellPivot` (родной корпус DUO) |
| Фабрика корпуса | явные `default_name_*` («__Базовый корпус Парамкаталога») | дефолты `ShellPivotDuo` (`P_shell_duo.py`) |
| Мягкое сиденье | **Есть** (`SoftSeats.factory`, поля `seat_*`) | Нет |
| Ящики | `Niche_Box_Pivot` по `_blockbox1/2`, `module`, `zsk` | пантографы `pants_*`, без блока ящиков |
| Крепёж | через `functions.get_*` + `RSH_Complect` | инлайн `priceinfo(...)`, без `get_*` |

**Вывод:** `boad.py` — форк DUO-2-шкафа, специализированный под **тумбу**:
подменён корпус на плоский «базовый», добавлено сиденье сверху, крепёж
маршрутизирован через хелперы `functions`. Каркас «две ниши + распашные двери +
полки + вешалки» — идентичен.

## 6. Оракул ↔ SParametrica DUO-2 (CA-46516 / feature/duo2-doors-fillings)

### 6.1. Архитектурный сдвиг: монолит → MVC

```{mermaid}
flowchart TB
    subgraph ORACLE["ОРАКУЛ boad.py / cab_duo_two.py (монолит)"]
        direction TB
        O1["proto params"] --> O2["RSH_DUO.params_adapter<br/>(+ priceinfo + dbsetvar)"]
        O2 --> O3["UnitDUO.Make()<br/>геометрия + K3 вперемешку"]
        O3 --> O4["UnitDUO.Draw()<br/>Panel/Door в сцене K3"]
    end

    subgraph SPARAM["SPARAMETRICA DUO-2 (MVC)"]
        direction TB
        S1["proto dict"] --> S2["duo_controller.build_duo_params<br/>+ resolve_render_materials<br/>(единственная точка priceinfo)"]
        S2 --> S3["DuoParams (чистые числа)"]
        S3 --> S4["arline_geometry.build_duo_parts<br/>ноль k3 - List of Part"]
        S4 --> S5["renderer_k3.render<br/>Part.tag - based_build.Panel/Hinge"]
        S4 -.-> S6["renderer_web (calcmebel)<br/>тот же Part - GLB"]
    end

    ORACLE -->|"портируется 1:1 по поведению"| SPARAM
```

Оракул смешивает три роли в одном классе; SParametrica режет по слоям (правила
блупринта 031 / требования 024):

| | Оракул `boad.py` | SParametrica DUO-2 |
|---|---|---|
| Model (что построить) | `Make()` + адаптеры, `import k3` внутри | `arline_geometry.families.duo.build_duo_parts` — **ноль k3** |
| Controller (proto→числа, номенклатура) | `params_adapter` + `priceinfo` + `dbsetvar` в теле | `controller/duo_controller.build_duo_params` + `resolve_render_materials` (**единственная** точка `priceinfo`, R3) |
| View (рисование) | `Draw()`/`Panel`/`Door` там же | `renderer_k3.render` (`Part.tag` → `Panel`/`Hinge`) |
| Второй потребитель | нет | `renderer_web` (GLB для веб-калькулятора) — **тот же Model** |
| Тесты без K3 | практически нет | 32 k3-less (controller) + 140 (пакет геометрии) |

### 6.2. Соответствие полей: `params_adapter` → `build_duo_params` → `DuoParams`

`duo_controller` явно объявляет себя портом `RSH_Duo.params_adapter`
(`cab_duo_two.py:229-291`):

| proto | `DuoParams` | Заметка |
|---|---|---|
| `w/h/d` | `w/h/d` | — |
| `colorcmmater→MatId→Thickness` | `h_dsp` | толщина ЛДСП из материала (истина в K3), запас — `hdsp_k` |
| `hcok` | `h_cok` | валидируется в Model |
| `openside1/openside2` | `openside1/openside2` | `openside2` дефолт **2** (внешние боковины, спека) |
| `doubledoor1/2` | `doubledoor1/2` | — |
| `hinge_shift/p_type` | `hng_shift` | `resolve_hng_shift` |
| `colorfsmat1→MatId→Thickness` | `h_fas` | 0 → Model падает на `h_dsp` |
| `polkstd1/2` | `polkstd1/2` | число полок |
| `polkstd{n}h1..5` | `polkstd{n}_z` | Z-позиции, обрезаются по числу |
| `hpolktp1/2`, `hpolkdp1/2` | `h_small_int_t1/2`, `h_small_int_d1/2` | `d` кладётся **сырым** (флаг применяется в Model) |
| `ishpolkdp1/2` | `ishpolkdp1/2` | — |
| `symmetry` | — | у `DuoParams` нет; зеркалит через `openside1/2` |

### 6.3. Осознанные расхождения с оракулом (записаны в коде)

1. `h_small_int_d1/d2` — **сырые**; оракул домножает на `ishpolkdp`, но Model
   учитывает флаг на месте использования → домножать здесь = применить дважды.
2. `h_dsp` — из `Thickness` материала, а вне K3 — из `hdsp_k` (функция остаётся
   k3-less, R4).
3. `openside2` дефолт `2` (RIGHT) вместо `1` — единственная смена **поведения по
   умолчанию** (D2 в `plans/035`), важна только для превью без K3.
4. `h_fas` — один на оба фасада (обе створки одного материала); TODO при разных.

### 6.4. `renderer_k3` — family-agnostic (ветвление по тегу)

`renderer_k3.render` **не знает про DUO/MONO** — диспетчеризует по `part.tag`:

- `PANEL_SPECS`: `SIDE_LEFT/RIGHT`, `TOP`, `BOTTOM`, `SHELF`, `BACK`, `PLINTH`,
  `WALL_STRONG`, **`MIDDLE_WALL`**, `DOOR` → параметрический `based_build.Panel`
  (ось length/width/thick по `MAJORPLACE`, материал `panel|back|facade`).
- `HINGE` → реальная сборная петля (`_make_hinge`, сторона навеса из
  `part.rotation_y_deg`), иначе placeholder-бокс.
- `ASSET_TAGS` (`ROD`, `ROD_HOLDER`, `BOX_FRONT`, `ACCESSORY`) → ассет по goods-id.
- иначе — тег молча пропускается.

Единственная «DUO-специфика» — `MIDDLE_WALL`, и она рисуется как обычная стенка
(`WALL`). Никаких `if family == "DUO"`.

### 6.5. Контракт детали `Part` (версионируемый)

`arline_geometry.base_types.Part`: `name, tag: PartTag, position: Point, size:
Gabs, rotation_y_deg: float, cuts: Optional[Cuts]`. Без `material_id`/`thickness`
(толщина внутри `size`, материал вешает рендерер стороны). `effective_box(part)`
даёт реальное тело после подрезов. Ключи контракта закреплены тестом
`test_renderer_web.py`.

### 6.6. Чего в SParametrica DUO-2 ещё НЕТ (по сравнению с `boad.py`)

| Функция оракула | Статус в SParametrica |
|---|---|
| Тумба/педестал (`boad.py`) как отдельное семейство | **Нет** — портирован только шкаф DUO-2 |
| Мягкое сиденье `SoftSeats` | **Нет** |
| Блок ящиков в DUO-family | Провайдер `niches/boxes.py` **есть** (module=185, стойка 41, MIN_DEPTH 263), но `build_duo_parts` его **не подключает** (только `Fill.SHELVES`) |
| Вешалки/штанга в DUO-family | Провайдер `niches/hangers.py` **есть**, в DUO **не подключён** |
| Сотовые полки (`Niche_SotShelf`) | Нет |
| `halpside` (6 мм, полунакладная) со стойкой | Посчитан, но **не связан** со стойкой блока ящиков (открытый TODO) |
| `dbsetvar` (кэш `prmater/band/…`) в рендерере | **Задекларирован в docstring, но не реализован** — см. §8 |

## 7. Статус веток и git

```{mermaid}
gitGraph
    commit id: "develop"
    branch feature/CA-46516
    checkout feature/CA-46516
    commit id: "MVC MONO-1"
    commit id: "exp3d convention"
    branch feature/duo2-doors-fillings
    checkout feature/duo2-doors-fillings
    commit id: "build_duo_parts + тесты"
    commit id: "блок ящиков FacadeLayout"
    commit id: "планы 035/036/037"
```

**Закоммичено** на `feature/duo2-doors-fillings` (не на origin!): модель
`packages/arline-geometry/.../families/duo.py`, `params.py`, `doors.py`,
`niches/*`, тесты пакета, планы 035/036/037, exp3d-конвенция.

**НЕ закоммичено (рабочее дерево):**

```text
 M Proto/SParamProtoLib/renderer_k3/__init__.py
?? Proto/SParamProtoLib/P_CabDUO_2.py
?? Proto/SParamProtoLib/controller/duo_controller.py
?? Proto/SParamProtoLib/tests/test_duo_controller.py
```

То есть **K3-обвязка DUO-2 (макрос + контроллер + тест) существует только в
рабочем дереве** — риск потери (как и вся ветка: её нет на origin). Диф
`CA-46516..duo2` по закоммиченному — 24 файла (модель, планы, exp3d, инструменты
бота), DUO-макрос в него не входит, потому что untracked.

**Задача текущей сессии (kickoff 037):** нет K3-точки входа для DUO-2 → написать
`P_CabDUO_2.py` (есть в рабочем дереве, не закоммичен), при необходимости
расширить `renderer_k3` под DUO и построить в живом K3.

## 8. Находки аудита, долги и риски

### 8.1. Наблюдения в оракуле `boad.py` (read-only — только фиксируем)

1. **Вероятный баг `count_box2`** (`boad.py:581-589`): условие проверяет
   `if self._blockbox1:` вместо `self._blockbox2`, хотя возвращает `_blockbox2`.
   Симметричный `count_box1` (`:558`) корректен. При `blockbox1=0, blockbox2>0`
   второй блок ящиков не построится.
2. **Мутабельный дефолт + мутация** (`SetNiche1`, `:137` и `SetNiche2`, `:192`):
   сигнатура `nums_fas=[1,]`, внутри `nums_fas.append(...)`. Классический
   Python-капкан (общий список между вызовами). В основном пути `build_aboad`
   передаёт свежий список, поэтому «не стреляет», но при прямом вызове с дефолтом
   список растёт от вызова к вызову.
3. **`createNiche1` игнорирует свой `num_niche`** (`:169`): вызывает
   `inst.SetNiche1(inst, num_niche=1, …)` — жёстко `1`, хотя параметр принят.
   Аналогично `createNiche2` (`:224`, жёстко `2`).
4. **Гомоглиф в имени** `calculatе_shell_wpos1`: буква `е` — **кириллическая**
   (U+0435), не латинская `e`. Все использования консистентны (`:291`, `:512`,
   `:616`, `:1306`), поэтому работает, но поиск по латинскому `calculate` его не
   найдёт, а IDE-переименование может разорвать.
5. **`Build()` не используется** в `build_aboad` — дерево `Sostav` не строится
   пред-проходом; `getDoorsNiche()` полагается на `Sostav.sostav`, наполняемый во
   время `Draw`. Порядок «Make → Draw» тут корректен, но отличается от общего
   `DrawUnit()`.

### 8.2. Долги трека SParametrica DUO-2

1. **`dbsetvar` в `renderer_k3` — задекларирован, не реализован.** Docstring/
   комментарии говорят, что кэш `prmater/band/dvpMaterial` должен обновлять
   рендерер (R3), но код делает только `SetFurnType("100011")`. Если кэш
   «цвет-как-комплекс» должен освежаться при рендере — шаг отсутствует.
2. **Незакоммиченные файлы + ветка не на origin** (§7) — работа держится на одном
   диске. Первый безопасный шаг любой сессии — коммит и пуш.
3. **DUO-family не подключает `boxes`/`hangers`** — провайдеры готовы, но
   `build_duo_parts` эмитит только полки. Наполнения средней ниши (ящики, штанга,
   вешало) из спеки — открыты.
4. **`halpside` (6 мм) не связан со стойкой** блока ящиков — створка у стойки
   должна получать полунакладную петлю; посчитано, но не применено.
5. **Многозначность ProtoID DUO-2** — в разных реестрах разные числа:
   `P_CabDUO_2.py` (SParametrica): DUO-2 = **465**, MONO-1 = 463, меню владельца =
   7156; `feature-CA-46516-workflow.md`: MONO-1 = 455, DUO-2 = **329**. Перед
   живой сборкой ID нужно уточнить (открытый вопрос kickoff 037).
6. **Педестал/тумба (`boad.py`) вообще не портирован** — если тумба DUO-2 нужна
   на вебе, это отдельное семейство (свой `build_pedestal_duo_parts`, `SoftSeats`,
   плоский корпус), которого сейчас нет.

## 9. Как проверить (Verify)

```powershell
# чистая модель DUO (пакет геометрии) — 140 тестов
cd c:\REPO\ARLINE\packages\arline-geometry ; C:/VENV37/Scripts/python.exe -m pytest -q

# K3-сторона DUO (controller + renderer_k3, без K3) — 32 теста
cd c:\REPO\ARLINE\Proto ; C:/VENV310/Scripts/python.exe -m pytest SParamProtoLib/tests/ -m "not k3" -q

# живой K3 (агент TCP 9000): построить DUO-2 макросом
#   k3.protoobj(k3.k_create, "SParametrica", <ProtoID DUO-2>, k3.k_done, 0, 0, 0)
# оракул-тумба для сверки:
#   P_PedestalDUO_2.py -> boad.main(default_name_proto="РШ Тумба DUO-2")
```

**Файлы-ориентиры:** `Proto/LeroyProtoLIB/pedestal_pivot_duo/boad.py`,
`Proto/LeroyProtoLIB/cab_duo_two.py`, `Proto/SParamProtoLib/P_CabDUO_2.py`,
`Proto/SParamProtoLib/controller/duo_controller.py`,
`packages/arline-geometry/src/arline_geometry/families/duo.py`,
`plans/031-sparametrica-protolib-blueprint.md` (ветка CA-46516),
`plans/035-duo2-decisions.md`, `plans/037-kickoff-duo2-k3-build.md`.
