# Мебель параметрического каталога

## ProtoID 311 Терминал

![_terminal](./Pictures/terminal.jpg)

```python
{
    'proto_id': 311,                                                                                     
    'band': 6793,  # param_randomize(18)                Тип кромки корпуса                               
    'colorcmmater': 22390,  # param_randomize(440)               Цвет корпуса                            
    'count': 5,  #                                    Число полок                                        
    'd': 413,  # random.choice(range(402, 900))     Глубина                                              
    'h': 2058,  # random.choice(range(2058, 2700))   Высота                                              
    'hcok': 100,  # random.choice(range(41, 250))      Высота цоколя                                     
    'prmater': 0,  # param_randomize(330)               Материал                                         
    'rshcomplect': 11165,  # param_randomize(474)               Набор для РШ (ключ инструкци    крепежи) 
    'symmetry': 1,  # random.choice(range(0,1)           Признак симметрии 0- Левый 1-правый                        |
    'w': 400,  # random.choice(range(200, 600))     Ширина                                               
}
```

## Антресоли

| Параметры                       | MONO-1   | MONO-2   | DUO-2    | DUO-3   левый | DUO-3   правый | DUO-4     | TRIO-3   | TRIO-4    | TRIO-5    | TRIO-6    |
| :------------------------------ | :------- | :------- | :------- | :------------ | :------------- | :-------- | :------- | :-------- | :-------- | :-------- |
| Количество секций по ширине, шт | 1        | 1        | 2        | 2             | 2              | 2         | 3        | 3         | 3         | 3         |
| Количество                      |          |          |          |               |                |           |          |           |           |           |
| фасадов, шт                     | 1        | 2        | 2        | 3             | 3              | 4         | 3        | 4         | 5         | 6         |
| Ширина корпуса ШК, мм           | 250-600  | 500-1000 | 500-1200 | 750-1500      | 750-1500       | 1000-2000 | 750-1800 | 1000-2000 | 1250-2500 | 1500-2790 |
| Глубина корпуса                 |          |          |          |               |                |           |          |           |           |           |
| ГК, мм                          | 150-800  | 150-800  | 150-800  | 150-800       | 150-800        | 150-800   | 150-800  | 150-800   | 150-800   | 150-800   |
| Высота корпуса                  |          |          |          |               |                |           |          |           |           |           |
| ВК, мм                          | 400-1200 | 400-1200 | 400-1200 | 400-1200      | 400-1200       | 400-1200  | 400-1200 | 400-1200  | 400-1200  | 400-1200  |

### ProtoId 400  РШ Антресоль MONO-1

![MONO-1](./Pictures/Antresol/MONO-1.jpg)

```python
{
    'proto_id': 400,
    'colorcmmater': param_randomize(dYAD.colorcmmater),  # Цвет корпуса
    'colorfsmat1': param_randomize(dYAD.colorcmmater),  # Цвет фасада Прайм
    'd': random.choice(range(150, 800)),  # Глубина
    'decofsmatc1': 0.0,  # MDF Отделка фасада
    'fasrtype1': 10841.0,  # Рисунок фасада
    'h': random.choice(range(400, 1200)),  # Высота
    'hantype': 24459.0,  # Тип ручки
    'pusher': 21534,  # param_randomize(127)               Демпфер/Толкатель для "без ручки" Hettich 16904
    'n1delh_1': 0,  # param_randomize(367)               Низ фил выс кол-во делителей
    'openside1': 1.0,  # Открывание двери
    'polkstd1': 0.0,  # Полки в нишу
    'w': random.choice(range(250, 600)),  # Ширина
}
```

### ProtoId 401  РШ Антресоль MONO-2

![MONO-2](./Pictures/Antresol/MONO-2.jpg)

```python
{
    'proto_id': 401,
    'colorcmmater': param_randomize(dYAD.colorcmmater),  # Цвет корпуса
    'colorfsmat1': param_randomize(dYAD.colorcmmater),  # Цвет фасада Прайм
    'd': random.choice(range(150, 800)),  # Глубина
    'decofsmatc1': 0.0,  # MDF Отделка фасада
    'fasrtype1': 10841.0,  # Рисунок фасада
    'h': random.choice(range(400, 1200)),  # Высота
    'hantype': 24459.0,  # Тип ручки
    'polkstd1': 0.0,  # Полки в нишу
    'w': random.choice(range(500, 1000)),  # Ширина
    'pusher': 21534,  # param_randomize(127)               Демпфер/Толкатель для "без ручки" Hettich 16904
    'n1delh_1': 0,  # param_randomize(367)               Низ фил выс кол-во делителей
}
```

### ProtoId 402 РШ Антресоль DUO-2

![DUO](./Pictures/Antresol/DUO.jpg)

```python
{
    'proto_id': 402,
    'colorcmmater': param_randomize(dYAD.colorcmmater),  # Цвет корпуса
    'colorfsmat1': param_randomize(dYAD.colorcmmater),  # Цвет фасада Прайм
    'colorfsmat2': param_randomize(dYAD.colorcmmater),  # Цвет фасада Прайм
    'd': random.choice(range(150, 800)),  # Глубина
    'decofsmatc1': 0.0,  # MDF Отделка фасада
    'decofsmatc2': 0.0,  # MDF Отделка фасада
    'fasrtype1': 10841.0,  # Рисунок фасада
    'fasrtype2': 10841.0,  # Рисунок фасада
    'h': random.choice(range(400, 1200)),  # Высота
    'hantype': 24459.0,  # Тип ручки
    'polkstd1': 0.0,  # Полки в нишу справа
    'polkstd2': 0.0,  # Полки в нишу слева
    'w': random.choice(range(500, 1200)),  # Ширина
    'pusher': 21534,  # param_randomize(127)               Демпфер/Толкатель для "без ручки" Hettich 16904
    'n1delh_1': 0,  # param_randomize(367)               Низ фил выс кол-во делителей 
    'n1delh_2': 0,  # param_randomize(367)               Низ фил выс кол-во делителей   
}
```

### ProtoId 403 РШ Антресоль DUO-3 левый

![DUO-3L](./Pictures/Antresol/DUO-3L.jpg)

```python
{
    'proto_id': 403,
    'colorcmmater': param_randomize(dYAD.colorcmmater),  # Цвет корпуса
    'colorfsmat1': param_randomize(dYAD.colorcmmater),  # Цвет фасада Прайм
    'colorfsmat2': param_randomize(dYAD.colorcmmater),  # Цвет фасада Прайм
    'd': random.choice(range(150, 800)),  # Глубина
    'decofsmatc1': 0.0,  # MDF Отделка фасада
    'decofsmatc2': 0.0,  # MDF Отделка фасада
    'fasrtype1': 10841.0,  # Рисунок фасада
    'fasrtype2': 10841.0,  # Рисунок фасада
    'h': random.choice(range(400, 1200)),  # Высота
    'hantype': 24459.0,  # Тип ручки
    'polkstd1': 0.0,  # Полки в нишу справа
    'polkstd2': 0.0,  # Полки в нишу слева
    'w': random.choice(range(500, 1500)),  # Ширина
    'pusher': 21534,  # param_randomize(127)               Демпфер/Толкатель для "без ручки" Hettich 16904
    'n1delh_1': 0,  # param_randomize(367)               Низ фил выс кол-во делителей 
    'n1delh_2': 0,  # param_randomize(367)               Низ фил выс кол-во делителей
}
```

### ProtoId 404 РШ Антресоль DUO-3 правый

![DUO-3R](./Pictures/Antresol/DUO-3R.jpg)

```python
{
    'proto_id': 404,
    'colorcmmater': param_randomize(dYAD.colorcmmater),  # Цвет корпуса
    'colorfsmat1': param_randomize(dYAD.colorcmmater),  # Цвет фасада Прайм
    'colorfsmat2': param_randomize(dYAD.colorcmmater),  # Цвет фасада Прайм
    'd': random.choice(range(150, 800)),  # Глубина
    'decofsmatc1': 0.0,  # MDF Отделка фасада
    'decofsmatc2': 0.0,  # MDF Отделка фасада
    'fasrtype1': 10841.0,  # Рисунок фасада
    'fasrtype2': 10841.0,  # Рисунок фасада
    'h': random.choice(range(400, 1200)),  # Высота
    'hantype': 24459.0,  # Тип ручки
    'polkstd1': 0.0,  # Полки в нишу справа
    'polkstd2': 0.0,  # Полки в нишу слева
    'w': random.choice(range(500, 1500)),  # Ширина
    'pusher': 21534,  # param_randomize(127)               Демпфер/Толкатель для "без ручки" Hettich 16904
    'n1delh_1': 0,  # param_randomize(367)               Низ фил выс кол-во делителей 
    'n1delh_2': 0,  # param_randomize(367)               Низ фил выс кол-во делителей
}
```

### ProtoId 405 РШ Антресоль DUO-4

![DUO-4](./Pictures/Antresol/DUO-4.jpg)

```python
{
    'proto_id': 405,
    'colorcmmater': param_randomize(dYAD.colorcmmater),  # Цвет корпуса
    'colorfsmat1': param_randomize(dYAD.colorcmmater),  # Цвет фасада Прайм
    'colorfsmat2': param_randomize(dYAD.colorcmmater),  # Цвет фасада Прайм
    'd': random.choice(range(150, 800)),  # Глубина
    'decofsmatc1': 0.0,  # MDF Отделка фасада
    'decofsmatc2': 0.0,  # MDF Отделка фасада
    'fasrtype1': 10841.0,  # Рисунок фасада
    'fasrtype2': 10841.0,  # Рисунок фасада
    'h': random.choice(range(400, 1200)),  # Высота
    'hantype': 24459.0,  # Тип ручки
    'polkstd1': 0.0,  # Полки в нишу справа
    'polkstd2': 0.0,  # Полки в нишу слева
    'w': random.choice(range(1000, 2000)),  # Ширина
    'pusher': 21534,  # param_randomize(127)               Демпфер/Толкатель для "без ручки" Hettich 16904
    'n1delh_1': 0,  # param_randomize(367)               Низ фил выс кол-во делителей 
    'n1delh_2': 0,  # param_randomize(367)               Низ фил выс кол-во делителей
}
```

### ProtoId 406 РШ Антресоль TRIO-3

![TRIO-3](./Pictures/Antresol/TRIO-3.jpg)

```python
{
    'proto_id': 406,
    'colorcmmater': param_randomize(dYAD.colorcmmater),  # Цвет корпуса
    'colorfsmat1': param_randomize(dYAD.colorcmmater),  # Цвет фасада Прайм
    'colorfsmat2': param_randomize(dYAD.colorcmmater),  # Цвет фасада Прайм
    'colorfsmat3': param_randomize(dYAD.colorcmmater),  # Цвет фасада Прайм
    'd': random.choice(range(150, 800)),  # Глубина
    'decofsmatc1': 0.0,  # MDF Отделка фасада справа
    'decofsmatc2': 0.0,  # MDF Отделка фасада середина
    'decofsmatc3': 0.0,  # MDF Отделка фасада слева
    'fasrtype1': 10841.0,  # Рисунок фасада справа
    'fasrtype2': 10841.0,  # Рисунок фасада середина
    'fasrtype3': 10841.0,  # Рисунок фасада слева
    'h': random.choice(range(400, 1200)),  # Высота
    'hantype': 24459.0,  # Тип ручки
    'polkstd1': 0.0,  # Полки в нишу справа
    'polkstd2': 0.0,  # Полки в нишу середина
    'polkstd3': 0.0,  # Полки в нишу слева
    'w': random.choice(range(750, 1800)),  # Ширина
    'pusher': 21534,  # param_randomize(127)               Демпфер/Толкатель для "без ручки" Hettich 16904
    'n1delh_1': 0,  # param_randomize(367)               Низ фил выс кол-во делителей 
    'n1delh_2': 0,  # param_randomize(367)               Низ фил выс кол-во делителей   
    'n1delh_3': 0,  # param_randomize(367)               Низ фил выс кол-во делителей
}
```

### ProtoId 407  РШ Антресоль TRIO-4

![TRIO-4](./Pictures/Antresol/TRIO-4.jpg)

```python
{
    'proto_id': 407,
    'colorcmmater': param_randomize(dYAD.colorcmmater),  # Цвет корпуса
    'colorfsmat1': param_randomize(dYAD.colorcmmater),  # Цвет фасада Прайм
    'colorfsmat2': param_randomize(dYAD.colorcmmater),  # Цвет фасада Прайм
    'colorfsmat3': param_randomize(dYAD.colorcmmater),  # Цвет фасада Прайм
    'd': random.choice(range(150, 800)),  # Глубина
    'decofsmatc1': 0.0,  # MDF Отделка фасада справа
    'decofsmatc2': 0.0,  # MDF Отделка фасада середина
    'decofsmatc3': 0.0,  # MDF Отделка фасада слева
    'fasrtype1': 10841.0,  # Рисунок фасада справа
    'fasrtype2': 10841.0,  # Рисунок фасада середина
    'fasrtype3': 10841.0,  # Рисунок фасада слева
    'h': random.choice(range(400, 1200)),  # Высота
    'hantype': 24459.0,  # Тип ручки
    'polkstd1': 0.0,  # Полки в нишу справа
    'polkstd2': 0.0,  # Полки в нишу середина
    'polkstd3': 0.0,  # Полки в нишу слева
    'w': random.choice(range(1000, 2000)),  # Ширина
    'pusher': 21534,  # param_randomize(127)               Демпфер/Толкатель для "без ручки" Hettich 16904
    'n1delh_1': 0,  # param_randomize(367)               Низ фил выс кол-во делителей 
    'n1delh_2': 0,  # param_randomize(367)               Низ фил выс кол-во делителей   
    'n1delh_3': 0,  # param_randomize(367)               Низ фил выс кол-во делителей   
}
```

### ProtoId 408  РШ Антресоль TRIO-5

![TRIO-5](./Pictures/Antresol/TRIO-5.jpg)

```python
{
    'proto_id': 408,
    'colorcmmater': param_randomize(dYAD.colorcmmater),  # Цвет корпуса
    'colorfsmat1': param_randomize(dYAD.colorcmmater),  # Цвет фасада Прайм
    'colorfsmat2': param_randomize(dYAD.colorcmmater),  # Цвет фасада Прайм
    'colorfsmat3': param_randomize(dYAD.colorcmmater),  # Цвет фасада Прайм
    'd': random.choice(range(150, 800)),  # Глубина
    'decofsmatc1': 0.0,  # MDF Отделка фасада справа
    'decofsmatc2': 0.0,  # MDF Отделка фасада середина
    'decofsmatc3': 0.0,  # MDF Отделка фасада слева
    'fasrtype1': 10841.0,  # Рисунок фасада справа
    'fasrtype2': 10841.0,  # Рисунок фасада середина
    'fasrtype3': 10841.0,  # Рисунок фасада слева
    'h': random.choice(range(400, 1200)),  # Высота
    'hantype': 24459.0,  # Тип ручки
    'polkstd1': 0.0,  # Полки в нишу справа
    'polkstd2': 0.0,  # Полки в нишу середина
    'polkstd3': 0.0,  # Полки в нишу слева
    'w': random.choice(range(1250, 2500)),  # Ширина
    'pusher': 21534,  # param_randomize(127)               Демпфер/Толкатель для "без ручки" Hettich 16904
    'n1delh_1': 0,  # param_randomize(367)               Низ фил выс кол-во делителей 
    'n1delh_2': 0,  # param_randomize(367)               Низ фил выс кол-во делителей   
    'n1delh_3': 0,  # param_randomize(367)               Низ фил выс кол-во делителей   
}
```

### ProtoId 409  РШ Антресоль TRIO-6

![TRIO-6](./Pictures/Antresol/TRIO-6.jpg)

```python
{
    'proto_id': 409,
    'colorcmmater': param_randomize(dYAD.colorcmmater),  # Цвет корпуса
    'colorfsmat1': param_randomize(dYAD.colorcmmater),  # Цвет фасада Прайм
    'colorfsmat2': param_randomize(dYAD.colorcmmater),  # Цвет фасада Прайм
    'colorfsmat3': param_randomize(dYAD.colorcmmater),  # Цвет фасада Прайм
    'd': random.choice(range(150, 800)),  # Глубина
    'decofsmatc1': 0.0,  # MDF Отделка фасада справа
    'decofsmatc2': 0.0,  # MDF Отделка фасада середина
    'decofsmatc3': 0.0,  # MDF Отделка фасада слева
    'fasrtype1': 10841.0,  # Рисунок фасада справа
    'fasrtype2': 10841.0,  # Рисунок фасада середина
    'fasrtype3': 10841.0,  # Рисунок фасада слева
    'h': random.choice(range(400, 1200)),  # Высота
    'hantype': 24459.0,  # Тип ручки
    'polkstd1': 0.0,  # Полки в нишу справа
    'polkstd2': 0.0,  # Полки в нишу середина
    'polkstd3': 0.0,  # Полки в нишу слева
    'w': random.choice(range(1500, 2790)),  # Ширина
    'pusher': 21534,  # param_randomize(127)               Демпфер/Толкатель для "без ручки" Hettich 16904
    'n1delh_1': 0,  # param_randomize(367)               Низ фил выс кол-во делителей 
    'n1delh_2': 0,  # param_randomize(367)               Низ фил выс кол-во делителей   
    'n1delh_3': 0,  # param_randomize(367)               Низ фил выс кол-во делителей   
}
```

## Комоды

  | Параметры                       | MONO-1   | DUO-2 левый | DUO-2 правый | TRIO-3    |
  | :------------------------------ | :------- | :---------- | :----------- | :-------- |
  | Количество секций по ширине, шт | 1        | 2           | 2            | 3         |
  | Количество                      |          |             |              |           |
  | фасадов по ширине, шт           | 1        | 2           | 2            | 3         |
  | Ширина корпуса           ШК, мм | 500-800  | 800-1200    | 800-1200     | 1200-1600 |
  | Глубина корпуса                 |          |             |              |           |
  | ГК, мм*                         | 300-800  | 300-800     | 300-800      | 300-800   |
  | Высота корпуса                  |          |             |              |           |
  | ВК, мм                          | 600-1100 | 600-1100    | 600-1100     | 600-1100  |

Зависимость числа ящиков от высоты корпуса

  | Высота корпуса комода, мм | Кол-во ящиков, шт |
  | :------------------------ | :---------------- |
  |                           |                   |
  | 600 - 614                 | 2                 |
  | 600 - 878                 | 3                 |
  | 662 - 1008                | 4                 |
  | 806 - 1100                | 5                 |

## ProtoId 422 РШ Комод MONO-1

![MONO-1](./Pictures/Commode/MONO-1.jpg)

```python
{
    'proto_id': 422,
    'blockbox1': 3,  # random.choice(range(2, 5))         Количество ящиков
    'bxtype': 10804,  # param_randomize(475)               Тип ящика
    'colorcmmater': 22390,  # param_randomize(440)               Цвет корпуса
    'colorfsmat1': 22390,  # param_randomize(440)               Цвет фасада Прайм
    'd': 600,  # random.choice(range(300, 800))     Глубина
    'decofsmatc1': 0,  # param_randomize(454)               MDF Отделка фасада
    'fasrtype1': 10841,  # param_randomize(446)               Рисунок фасада
    'h': 600,  # random.choice(range(600, 1100))    Высота
    'hantype': 24459,  # param_randomize(23)                Тип ручки
    'hcok': 70,  # random.choice(range(70, 250))      Высота цоколя
    'w': 500,  # random.choice(range(500, 800))     Ширина
    'pusher': 21534,  # param_randomize(127)               Демпфер/Толкатель для "без ручки" Hettich 16904
}
```

### ProtoId 423 РШ Комод DUO-2 левый

![DUO_2L](./Pictures/Commode/DUO_2L.jpg)

```python
{
    'proto_id': 423,
    'blockbox1': 0,  # random.choice(range(0, 5))         Количество ящиков
    'blockbox2': 3,  # random.choice(range(2, 5))         Количество ящиков
    'bxtype': 10804,  # param_randomize(475)               Тип ящика
    'colorcmmater': 22390,  # param_randomize(440)               Цвет корпуса
    'colorfsmat1': 22390,  # param_randomize(440)               Цвет фасада Прайм
    'colorfsmat2': 22390,  # param_randomize(440)               Цвет фасада Прайм
    'd': 600,  # random.choice(range(300, 800))     Глубина
    'decofsmatc1': 0,  # param_randomize(454)               MDF Отделка фасада
    'decofsmatc2': 0,  # param_randomize(454)               MDF Отделка фасада
    'fasrtype1': 10841,  # param_randomize(446)               Рисунок фасада
    'fasrtype2': 10841,  # param_randomize(446)               Рисунок фасада
    'h': 600,  # random.choice(range(600, 1100))    Высота
    'hantype': 24459,  # param_randomize(23)                Тип ручки
    'hcok': 70,  # random.choice(range(70, 250))      Высота цоколя
    'polkstd1': 0,  # random.choice(range(0, 5))         Полки в нишу
    'polkstd2': 0,  # random.choice(range(0, 0))         Полки в нишу
    'w': 900,  # random.choice(range(800, 1200))    Ширина
    'pusher': 21534,  # param_randomize(127)               Демпфер/Толкатель для "без ручки" Hettich 16904
    'n1delh_1': 0,  # param_randomize(367)               Низ фил выс кол-во делителей 
    'n1delh_2': 0,  # param_randomize(367)               Низ фил выс кол-во делителей   
}
```

### ProtoId 424 РШ Комод DUO-2 правый

![DUO_2R](./Pictures/Commode/DUO_2R.jpg)

```python
{
    'proto_id': 424,
    'blockbox1': 3,  # random.choice(range(2, 5))         Количество ящиков
    'blockbox2': 0,  # random.choice(range(0, 0))         Количество ящиков
    'bxtype': 10804,  # param_randomize(475)               Тип ящика
    'colorcmmater': 22390,  # param_randomize(440)               Цвет корпуса
    'colorfsmat1': 22390,  # param_randomize(440)               Цвет фасада Прайм
    'colorfsmat2': 22390,  # param_randomize(440)               Цвет фасада Прайм
    'd': 600,  # random.choice(range(300, 800))     Глубина
    'decofsmatc1': 0,  # param_randomize(454)               MDF Отделка фасада
    'decofsmatc2': 0,  # param_randomize(454)               MDF Отделка фасада
    'fasrtype1': 10841,  # param_randomize(446)               Рисунок фасада
    'fasrtype2': 10841,  # param_randomize(446)               Рисунок фасада
    'h': 600,  # random.choice(range(600, 1100))    Высота
    'hantype': 24459,  # param_randomize(23)                Тип ручки
    'hcok': 70,  # random.choice(range(70, 250))      Высота цоколя
    'polkstd1': 0,  # random.choice(range(0, 5))         Полки в нишу
    'polkstd2': 0,  # random.choice(range(0, 0))         Полки в нишу
    'w': 900,  # random.choice(range(800, 1200))    Ширина
    'pusher': 21534,  # param_randomize(127)               Демпфер/Толкатель для "без ручки" Hettich 16904
    'n1delh_1': 0,  # param_randomize(367)               Низ фил выс кол-во делителей 
    'n1delh_2': 0,  # param_randomize(367)               Низ фил выс кол-во делителей   
}
```

### ProtoId 425 РШ Комод TRIO-3

![TRIO-3](./Pictures/Commode/TRIO-3.jpg)

```python
{
    'proto_id': 425,
    'blockbox1': 0,  # random.choice(range(0, 0))         Количество ящиков
    'blockbox2': 0,  # random.choice(range(2, 5))         Количество ящиков
    'blockbox3': 0,  # random.choice(range(0, 0))         Количество ящиков
    'bxtype': 10804,  # param_randomize(475)               Тип ящика
    'colorcmmater': 22390,  # param_randomize(440)               Цвет корпуса
    'colorfsmat1': 22390,  # param_randomize(440)               Цвет фасада Прайм
    'colorfsmat2': 22390,  # param_randomize(440)               Цвет фасада Прайм
    'colorfsmat3': 22390,  # param_randomize(440)               Цвет фасада Прайм
    'd': 600,  # random.choice(range(300, 800))     Глубина
    'decofsmatc1': 0,  # param_randomize(454)               MDF Отделка фасада
    'decofsmatc2': 0,  # param_randomize(454)               MDF Отделка фасада
    'decofsmatc3': 0,  # param_randomize(454)               MDF Отделка фасада
    'fasrtype1': 10841,  # param_randomize(446)               Рисунок фасада
    'fasrtype2': 10841,  # param_randomize(446)               Рисунок фасада
    'fasrtype3': 10841,  # param_randomize(446)               Рисунок фасада
    'h': 600,  # random.choice(range(600, 1100))    Высота
    'hantype': 24459,  # param_randomize(23)                Тип ручки
    'hcok': 70,  # random.choice(range(70, 250))      Высота цоколя
    'polkstd1': 0,  # random.choice(range(0, 5))         Полки в нишу
    'polkstd2': 0,  # random.choice(range(0, 0))         Полки в нишу
    'polkstd3': 0,  # random.choice(range(0, 5))         Полки в нишу
    'w': 1300,  # random.choice(range(1200, 1600))   Ширина
    'pusher': 21534,  # param_randomize(127)               Демпфер/Толкатель для "без ручки" Hettich 16904
    'n1delh_1': 0,  # param_randomize(367)               Низ фил выс кол-во делителей 
    'n1delh_2': 0,  # param_randomize(367)               Низ фил выс кол-во делителей   
    'n1delh_3': 0,  # param_randomize(367)               Низ фил выс кол-во делителей
}
```

## РШ Обувницы

  | Параметры                        | обувница MONO-1 с одним механизмом | обувница MONO-1 с двумя механизмами | обувница   MONO-1 с одним механизмом и ящиком | обувница MONO-1 с двумя механизмами и ящиком |
  | :------------------------------- | :--------------------------------- | :---------------------------------- | :-------------------------------------------- | :------------------------------------------- |
  | Количество секций по ширине, шт  | 1                                  | 1                                   | 1                                             | 1                                            |
  | Количество фасадов по ширине, шт | 1                                  | 1                                   | 1                                             | 1                                            |
  | Количество цвета/дизайна фасадов | 1                                  | 1                                   | 2                                             | 2                                            |
  | Ширина корпуса ШК, мм            | 500-800                            | 500-800                             | 500-800                                       | 500-800                                      |
  | Глубина корпуса  ГК, мм          | 320                                | 320                                 | 320                                           | 320                                          |
  | Высота корпуса  ВК, мм           | 447-507                            | 805-925                             | 590-650                                       | 948-1068                                     |

### ProtoId 427 РШ Обувница MONO-1-1

![Mono-1-1](./Pictures/Shoemaker/Mono-1-1.jpg)

```python
{
    'proto_id': 427,
    'colorcmmater': 22393,  # param_randomize(440)               Цвет корпуса 
    'colorfsmat1': 22378,  # param_randomize(440)               Цвет фасада Прайм 
    'd': 320,  # random.choice(range(300, 800))     Глубина 
    'decofsmatc1': 0,  # param_randomize(454)               MDF Отделка фасада 
    'fasrtype1': 10841,  # param_randomize(446)               Рисунок фасада 
    'h': 500,  # random.choice(range(400, 600))     Высота 
    'hantype': 24459,  # param_randomize(23)                Тип ручки 
    'hcok': 70,  # random.choice(range(70, 250))      Высота цоколя 
    'w': 600,  # random.choice(range(500, 800))     Ширина 
}
```

### ProtoId 428 РШ Обувница MONO-1-2

![Mono-1-2](./Pictures/Shoemaker/Mono-1-2.jpg)

```python
{
    'proto_id': 428,
    'colorcmmater': 22378,  # param_randomize(440)               Цвет корпуса 
    'colorfsmat1': 22393,  # param_randomize(440)               Цвет фасада Прайм 
    'd': 320,  # random.choice(range(300, 800))     Глубина 
    'decofsmatc1': 0,  # param_randomize(454)               MDF Отделка фасада 
    'fasrtype1': 10841,  # param_randomize(446)               Рисунок фасада 
    'h': 808,  # random.choice(range(800, 950))     Высота 
    'h_carga': 138,  # random.choice(range(0, 300))       Размер царги 
    'hantype': 24459,  # param_randomize(23)                Тип ручки 
    'hcok': 70,  # random.choice(range(70, 250))      Высота цоколя 
    'w': 600,  # random.choice(range(500, 800))     Ширина 
}
```

### ProtoId 429 РШ Обувница MONO-1-1-ящик

![Mono-1-1-box](./Pictures/Shoemaker/Mono-1-1-box.jpg)

```python
{
    'proto_id': 429,
    'colorcmmater': 22378,  # param_randomize(440)               Цвет корпуса 
    'colorfsmat1': 22393,  # param_randomize(440)               Цвет фасада Прайм 
    'colorfsmat2': 22393,  # param_randomize(440)               Цвет фасада Прайм 
    'd': 320,  # random.choice(range(300, 800))     Глубина 
    'decofsmatc1': 0,  # param_randomize(454)               MDF Отделка фасада 
    'decofsmatc2': 0,  # param_randomize(454)               MDF Отделка фасада 
    'fasrtype1': 10841,  # param_randomize(446)               Рисунок фасада 
    'fasrtype2': 10841,  # param_randomize(446)               Рисунок фасада 
    'h': 808,  # random.choice(range(580, 700))     Высота 
    'h_carga': 138,  # random.choice(range(0, 300))       Размер царги 
    'hantype': 24459,  # param_randomize(23)                Тип ручки 
    'hcok': 70,  # random.choice(range(70, 250))      Высота цоколя 
    'w': 600,  # random.choice(range(500, 800))     Ширина 
}
```

### ProtoId 430  РШ Обувница MONO-1-2-ящик

![Mono-1-2-box](./Pictures/Shoemaker/Mono-1-2-box.jpg)

```python
{
    'proto_id': 430,
    'colorcmmater': 22378,  # param_randomize(440)               Цвет корпуса 
    'colorfsmat1': 22393,  # param_randomize(440)               Цвет фасада Прайм 
    'colorfsmat2': 22393,  # param_randomize(440)               Цвет фасада Прайм 
    'd': 320,  # random.choice(range(300, 800))     Глубина 
    'decofsmatc1': 0,  # param_randomize(454)               MDF Отделка фасада 
    'decofsmatc2': 0,  # param_randomize(454)               MDF Отделка фасада 
    'fasrtype1': 10841,  # param_randomize(446)               Рисунок фасада 
    'fasrtype2': 10841,  # param_randomize(446)               Рисунок фасада 
    'h': 808,  # random.choice(range(940, 1200))     Высота 
    'h_carga': 138,  # random.choice(range(0, 300))       Размер царги 
    'hantype': 24459,  # param_randomize(23)                Тип ручки 
    'hcok': 70,  # random.choice(range(70, 250))      Высота цоколя 
    'w': 600,  # random.choice(range(500, 800))     Ширина 
}
```

## Тумбы

| Параметры                       | DUO-2    | DUO-3 левый | DUO-3 правый |
| :------------------------------ | :------- | :---------- | :----------- |
| Количество секций по ширине, шт | 2        | 2           | 2            |
| Количество                      |          |             |              |
| фасадов, шт                     | 2        | 3           | 3            |
| Ширина корпуса ШК, мм           | 500-1200 | 750-1500    | 750-1500     |
| Глубина корпуса                 |          |             |              |
| ГК, мм*                         | 150-800  | 150-800     | 150-800      |
| Высота корпуса                  |          |             |              |
| ВК, мм                          | 370-1200 | 370-1200    | 370-1200     |

:::{tip}
17/09/2025

1.Добавлены параметры **СЕТОК** вместо полок. Если число сеток больше 0, то ставим сетки.

- 'sotshelf1':  Полка сетчатая в нижнюю нишу
- 'sotshelf2':  Полка сетчатая в нижнюю нишу

2.Добавлены параметры мягких сидений

- 'seat_bk_gap': 0      Отступ сзади мягкого  - сиденья
- 'seatfabrics ': 21746    Ткань для мягкого  - сидения
- 'softseats': 0          Тип мягкого сидения

Типы мягких сидений:

| TValue                   | IValue |
| :----------------------- | :----- |
| Нет                      | 0      |
| Сидушка Corner мягкая    | 10731  |
| Сидушка Portofino мягкая | 10733  |

Ткани для сидений:

| TValue           | IValue |
| :--------------- | :----- |
| Велюр Velutto 03 | 21746  |
| Велюр Velutto 08 | 21751  |
| Велюр Velutto 11 | 21752  |
| Велюр Velutto 17 | 21756  |
| Велюр Velutto 23 | 21760  |
| Велюр Velutto 53 | 21773  |
| Нет              | 0      |

3.Добавлен тип фасада **НЕТ**. Если Фасада во всех секциях **НЕТ**, то свес вместо 20 всего 1 мм.

4.Минимальный размер коруса стал 370 мм.
:::

### ProtoId 431  РШ Тумба DUO-2

![DUO-2](./Pictures/Pedestal/DUO-2.jpg)

```python
{
    'proto_id': 431,
    'colorcmmater': 22390,  # param_randomize(440)               Цвет корпуса 
    'colorfsmat1': 22390,  # param_randomize(440)               Цвет фасада Прайм 
    'colorfsmat2': 22390,  # param_randomize(440)               Цвет фасада Прайм 
    'd': 600,  # random.choice(range(150, 800))     Глубина 
    'fasrtype1': 10841,  # param_randomize(446)               Рисунок фасада 
    'fasrtype2': 10841,  # param_randomize(446)               Рисунок фасада 
    'h': 440,  # random.choice(range(400, 1200))    Высота 
    'hantype': 24459,  # param_randomize(23)                Тип ручки 
    'hcok': 70,  # random.choice(range(70, 250))      Высота цоколя 
    'polkstd1': 0,  # random.choice(range(0, 5))         Полки в нишу 
    'polkstd2': 0,  # random.choice(range(0, 5))         Полки в нишу 
    'seat_bk_gap': 0,  # random.choice(range(0, 100))       Отступ сзади мягкого сиденья 
    'seatfabrics ': 21746,  # param_randomize(499)               Ткань для мягкого сидения 
    'softseats': 0,  # param_randomize(498)               Тип мягкого сидения 
    'sotshelf1': 0,  # random.choice(range(0, 5))         Полка сетчатая в нижнюю нишу 
    'sotshelf2': 0,  # random.choice(range(0, 5))         Полка сетчатая в нижнюю нишу 
    'w': 800,  # random.choice(range(500, 1200))    Ширина 
    'pusher': 21534,  # param_randomize(127)               Демпфер/Толкатель для "без ручки" Hettich 16904
    'n1delh_1': 0,  # param_randomize(367)               Низ фил выс кол-во делителей 
    'n1delh_2': 0,  # param_randomize(367)               Низ фил выс кол-во делителей   
}
```

### ProtoId 432 РШ Тумба DUO-3 левый

![DUO-3L](./Pictures/Pedestal/DUO-3L.jpg)

```python
{
    'proto_id': 432,
    'colorcmmater': 22390,  # param_randomize(440)               Цвет корпуса 
    'colorfsmat1': 22390,  # param_randomize(440)               Цвет фасада Прайм 
    'colorfsmat2': 22390,  # param_randomize(440)               Цвет фасада Прайм 
    'd': 600,  # random.choice(range(150, 800))     Глубина 
    'fasrtype1': 10841,  # param_randomize(446)               Рисунок фасада 
    'fasrtype2': 10841,  # param_randomize(446)               Рисунок фасада 
    'h': 440,  # random.choice(range(400, 1200))    Высота 
    'hantype': 24459,  # param_randomize(23)                Тип ручки 
    'hcok': 70,  # random.choice(range(70, 250))      Высота цоколя 
    'polkstd1': 0,  # random.choice(range(0, 5))         Полки в нишу 
    'polkstd2': 0,  # random.choice(range(0, 5))         Полки в нишу 
    'seat_bk_gap': 0,  # random.choice(range(0, 100))       Отступ сзади мягкого сиденья 
    'seatfabrics ': 21746,  # param_randomize(499)               Ткань для мягкого сидения 
    'softseats': 0,  # param_randomize(498)               Тип мягкого сидения 
    'sotshelf1': 0,  # random.choice(range(0, 5))         Полка сетчатая в нижнюю нишу 
    'sotshelf2': 0,  # random.choice(range(0, 5))         Полка сетчатая в нижнюю нишу 
    'w': 800,  # random.choice(range(750, 1500))    Ширина 
    'pusher': 21534,  # param_randomize(127)               Демпфер/Толкатель для "без ручки" Hettich 16904
    'n1delh_1': 0,  # param_randomize(367)               Низ фил выс кол-во делителей 
    'n1delh_2': 0,  # param_randomize(367)               Низ фил выс кол-во делителей   
}
```

### ProtoId 433  РШ Тумба DUO-3 правый

![DUO-3P](./Pictures/Pedestal/DUO-3P.jpg)

```python
{
    'proto_id': 433,
    'colorcmmater': 22390,  # param_randomize(440)               Цвет корпуса 
    'colorfsmat1': 22390,  # param_randomize(440)               Цвет фасада Прайм 
    'colorfsmat2': 22390,  # param_randomize(440)               Цвет фасада Прайм 
    'd': 600,  # random.choice(range(150, 800))     Глубина 
    'fasrtype1': 10841,  # param_randomize(446)               Рисунок фасада 
    'fasrtype2': 10841,  # param_randomize(446)               Рисунок фасада 
    'h': 440,  # random.choice(range(400, 1200))    Высота 
    'hantype': 24459,  # param_randomize(23)                Тип ручки 
    'hcok': 70,  # random.choice(range(70, 250))      Высота цоколя 
    'polkstd1': 0,  # random.choice(range(0, 5))         Полки в нишу 
    'polkstd2': 0,  # random.choice(range(0, 5))         Полки в нишу 
    'seat_bk_gap': 0,  # random.choice(range(0, 100))       Отступ сзади мягкого сиденья 
    'seatfabrics ': 21746,  # param_randomize(499)               Ткань для мягкого сидения 
    'softseats': 0,  # param_randomize(498)               Тип мягкого сидения 
    'sotshelf1': 0,  # random.choice(range(0, 5))         Полка сетчатая в нижнюю нишу 
    'sotshelf2': 0,  # random.choice(range(0, 5))         Полка сетчатая в нижнюю нишу 
    'w': 800,  # random.choice(range(750, 1500))    Ширина 
    'pusher': 21534,  # param_randomize(127)               Демпфер/Толкатель для "без ручки" Hettich 16904
    'n1delh_1': 0,  # param_randomize(367)               Низ фил выс кол-во делителей 
    'n1delh_2': 0,  # param_randomize(367)               Низ фил выс кол-во делителей   
}
```

## Панели

Стеновые панели характеризуют три основных параметра:

- **w** - ширина
- **h** - высота
- **colorcmmater** - цвет материала

для панели с крючками добавляются три параметра:

- **id_mensola** - id менсолы в зависимости от выбраного цвета
- **n_hoock** - количество крючков
- **id_fixnaves** - id крепежа для навеса

### ProtoId 434  РШ Панель с зеркалом

![mirrpan](./Pictures/wallpanels/mirrpan.jpg)

```python
{
    'proto_id': 434,
    'colorcmmater': 22390,  # param_randomize(440)               Цвет корпуса 
    'cutsize_mirr': 1,  #                                    Подрез зеркала по стороне 
    'd': 16,  #                                    Глубина 
    'h': 1500,  # random.choice(range(150, 2000))    Высота 
    'id_fixkruk': 11236,  # param_randomize(19)                Крепеж в коробку крюк с дюбелем 
    'id_fixnaves': 11234,  # param_randomize(19)                Id Типа крепежа для навеса 
    'id_mirband': 16473,  # param_randomize(18)                Кромка зеркала 
    'id_mirror': 5251,  # param_randomize(17)                Материал зеркала 
    'w': 800,  # random.choice(range(150, 1000))    Ширина 
}
```

### ProtoId 435  РШ Панель ЛДСП с крючками

![hookspan](./Pictures/wallpanels/hookspan.jpg)

```python
{
    'proto_id': 435,
    'colorcmmater': 22390,  # param_randomize(440)               Цвет корпуса 
    'd': 266,  # random.choice(range(16, 500))      Глубина 
    'h': 1500,  # random.choice(range(1000, 2000))   Высота 
    'id_fixkruk': 11236,  # param_randomize(19)                Крепеж в коробку крюк с дюбелем 
    'id_fixnaves': 11234,  # param_randomize(19)                Id Типа крепежа для навеса 
    'id_hoock': 24567,  # param_randomize(481)               ID крючка для одежды 
    'id_mensola': 24572,  # param_randomize(482)               Менсола для полки 
    'n_hoock': 3,  # random.choice(range(2, 5))         Количество крючков 
    'w': 800,  # random.choice(range(400, 1500))    Ширина 
}
```
