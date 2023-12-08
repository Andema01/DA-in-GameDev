# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]

Отчет по лабораторной работе #3 выполнил(а):

- Ярочкина Валентина Эдуардовна
- РИ220941
Отметка о выполнении заданий:

|Задание|Выполнение|Баллы|
|---|---|---|
|Задание 1|*|60|
|Задание 2|*|20|
|Задание 3|*|20|

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:

- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://camo.githubusercontent.com/f7460eaa21ddbc6986b8ea884fcbbfd8378db05a44eb75f69ec1f1d362aa1e8e/68747470733a2f2f636c6475702e636f6d2f645478705069396c44662e7468756d622e706e67)](https://nodesource.com/products/nsolid)

[![Build Status](https://camo.githubusercontent.com/c29bc856325cd819f5a3bb6536b7982f04a161e656de066c4c970e0079c14ff5/68747470733a2f2f7472617669732d63692e6f72672f6a6f656d6363616e6e2f64696c6c696e6765722e7376673f6272616e63683d6d6173746572)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы

**Разработать оптимальный баланс для десяти уровней игры Dragon Picker**

## Задание 1

### **Предложите вариант изменения найденных переменных для 10 уровней в игре. Визуализируйте изменение уровня сложности в таблице.**

Переменные, влияющие на движение дракона на сцене:
1. **speed** (`float`): Определяет скорость движения дракона.    
2. **leftRightDistance** (`float`): Указывает на дистанцию по оси X, в пределах которой дракон будет двигаться влево и вправо, прежде чем изменить направление.
3. **chanceDirection** (`float`): Вероятность изменения направления движения дракона в каждом кадре.

Переменные влияют на сбрасывание драконьих яиц на сцене:
1. **timeBetweenEggDrops** (`float`): Определяет интервал времени между созданием яиц дракона.

## Задание 2

### **Создайте 10 сцен на Unity с изменяющимся уровнем сложности.**

Чтобы посчитать сложность необходимо создать формулу сложности, применяя определенные коэффициенты к параметрам каждого уровня.

Например, для вычисления общей сложности уровня можно взять сумму всех параметров, умноженных на соответствующие им весовые коэффициенты, и получить единую метрику сложности.

Предположим, что у нас есть следующие весовые коэффициенты для каждого параметра:

- Скорость (`speed_values`) - 0.4
- Интервал между яйцами (`time_between_egg_drops`) - 0.3
- Дистанция влево/вправо (`left_right_distance`) - 0.2
- Вероятность изменения направления (`chance_direction`) - 0.1

Формула для вычисления общей сложности уровня выглядит так: Сложность = speed × 0.4 + timeBetweenEggDrops × 0.3 + leftRightDistance × 0.2 + chanceDirection × 0.1

|Уровень|speed|timeBetweenEggDrops|leftRightDistance|chanceDirection| 
|---|---|---|---|---|
|Уровень 1| 4 | 2 | 10 | 0,01 | 
|Уровень 2| 4,8 | 1,9 | 10,5 | 0,012 |
|Уровень 3| 6 | 1,8 | 11 | 0,015 |
|Уровень 4| 6,8 | 1,7 | 11,5 | 0,018 |
|Уровень 5| 7,2 | 1,6 | 12 | 0,02 |
|Уровень 6| 8,8 | 1,5 | 12,5 | 0,022 |
|Уровень 7| 10 | 1,4 | 13 | 0,025 |
|Уровень 8| 11,2 | 1,3 | 13,5 | 0,028 |
|Уровень 9| 12 | 1,2 | 14 | 0,03 |
|Уровень 10| 14 | 1,1 | 14,5 | 0,035 |

![[ComplexityOfLevels.png]]

## Задание 3

### Заполнение Google-таблицы с помощью скрипта на Python

Скрипт для заполнения гугл-таблицы
```py
import gspread
import pandas as pd


gc = gspread.service_account(filename='unitydatascience-400812-c4e9504cacaa.json')
spreadsheet = gc.open("UnityWorkshop3")

# Получение активного листа
sheet = spreadsheet.sheet1
# Создаем данные для таблицы
levels = list(range(1, 11))
speed_values = [4.0, 4.8, 6.0, 6.8, 7.2, 8.8, 10.0, 11.2, 12.0, 14.0]
time_between_egg_drops = [2.0, 1.9, 1.8, 1.7, 1.6, 1.5, 1.4, 1.3, 1.2, 1.1]
left_right_distance = [10.0, 10.5, 11.0, 11.5, 12.0, 12.5, 13.0, 13.5, 14.0, 14.5]
chance_direction = [0.01, 0.012, 0.015, 0.018, 0.02, 0.022, 0.025, 0.028, 0.03, 0.035]

# Создаем DataFrame с данными
data = {
    'Уровень': levels,
    'speed': speed_values,
    'timeBetweenEggDrops': time_between_egg_drops,
    'leftRightDistance': left_right_distance,
    'chanceDirection': chance_direction
}

df = pd.DataFrame(data)

# Загружаем данные в Google Sheets
sheet.update([df.columns.values.tolist()] + df.values.tolist())

print("Данные успешно загружены в Google Sheets.")
```

![[table.png]]

Скрипт для создания графика изменения сложности уровня 
```py
import gspread
import pandas as pd
import matplotlib.pyplot as plt


gc = gspread.service_account(filename='unitydatascience-400812-c4e9504cacaa.json')

sh = gc.open("UnityWorkshop3")
worksheet = sh.sheet1

# Получаем данные из Google Sheets
data = worksheet.get_all_values()

# Создаем DataFrame из данных
df = pd.DataFrame(data[1:], columns=data[0])

# Заменяем запятые на точки и преобразуем в числа с плавающей точкой
df[['speed', 'timeBetweenEggDrops', 'leftRightDistance', 'chanceDirection']] = df[['speed', 'timeBetweenEggDrops', 'leftRightDistance', 'chanceDirection']].apply(lambda x: x.str.replace(',', '.')).astype(float)

# Строим диаграмму изменения сложности уровня
plt.figure(figsize=(10, 6))
plt.plot(df['Уровень'], df['speed'], marker='o', label='Скорость')
plt.plot(df['Уровень'], df['timeBetweenEggDrops'], marker='o', label='Интервал между яйцами')
plt.plot(df['Уровень'], df['leftRightDistance'], marker='o', label='Дистанция влево/вправо')
plt.plot(df['Уровень'], df['chanceDirection'], marker='o', label='Вероятность изменения направления')
plt.xlabel('Уровень')
plt.ylabel('Значение')
plt.title('Изменение сложности уровня')
plt.legend()
plt.grid(True)
plt.show()
```

![[ComplexityChange.png]]

Скрипт для создания графика сложности уровней, который был представлен в Задании 2.
```py
import gspread
import pandas as pd
import matplotlib.pyplot as plt


gc = gspread.service_account(filename='unitydatascience-400812-c4e9504cacaa.json')

sh = gc.open("UnityWorkshop3")
worksheet = sh.sheet1

# Получаем данные из Google Sheets
data = worksheet.get_all_values()

# Создаем DataFrame из данных
df = pd.DataFrame(data[1:], columns=data[0])

# Заменяем запятые на точки и преобразуем в числа с плавающей точкой
df[['speed', 'timeBetweenEggDrops', 'leftRightDistance', 'chanceDirection']] = df[['speed', 'timeBetweenEggDrops', 'leftRightDistance', 'chanceDirection']].apply(lambda x: x.str.replace(',', '.')).astype(float)

# Весовые коэффициенты для параметров
coefficients = [0.4, 0.3, 0.2, 0.1]

# Вычисляем сложность для каждого уровня
complexities = []
for i in range(len(levels)):
    complexity = speed_values[i] * coefficients[0] + time_between_egg_drops[i] * coefficients[1] + left_right_distance[i] * coefficients[2] + chance_direction[i] * coefficients[3]
    complexities.append(complexity)

# Построение графика сложности уровней
plt.figure(figsize=(8, 6))
plt.plot(levels, complexities, marker='o', linestyle='-', color='b')
plt.title('Сложность уровней игры')
plt.xlabel('Уровень')
plt.ylabel('Сложность')
plt.grid(True)
plt.xticks(levels)
plt.show()
```
## Выводы

В процессе выполнения лабораторной работы было изучено поведение объекта Enemy в игре Dragon Picker, и были выявлены переменные, влияющие на уровень сложности игры. 

На основе этого анализа была создана и заполнена таблица в Google Sheets с помощью Python, содержащая данные из первоначального исследования, которые также были визуализированы. 

Были предложены варианты изменения этих переменных для создания 10 уровней игры с постепенно возрастающей сложностью. Затем эти 10 сцен были реализованы с использованием Unity.

|Plugin|README|
|---|---|
|Dropbox|[plugins/dropbox/README.md][PlDb]|
|GitHub|[plugins/github/README.md][PlGh]|
|Google Drive|[plugins/googledrive/README.md][PlGd]|
|OneDrive|[plugins/onedrive/README.md][PlOd]|
|Medium|[plugins/medium/README.md][PlMe]|
|Google Analytics|[plugins/googleanalytics/README.md][PlGa]|

## [](https://github.com/Den1sovDm1triy/DA-in-GameDev-lab1/blob/main/README.md#powered-by)Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
