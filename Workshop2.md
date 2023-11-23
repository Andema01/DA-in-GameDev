# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #2 выполнила:
- Ярочкина Валентина Эдуардовна
- РИ220941
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, , группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.

## Цель работы
Научиться передавать в Unity данные из Google Sheets с помощью Python.

## Задание 1
### Выбрать одну из компьютерных игр, приведите скриншот её геймплея и краткое описание концепта игры.

Dead Cells - это 2D платформенная игра с элементами экшена и рогалика. Игрок берет на себя роль бессмертного героя, путешествующего по различным уровням и сражающегося с врагами. Основная механика игры заключается в исследовании лабиринтов, сражении с врагами, собирании оружия, получении новых навыков и улучшении персонажа. Уровни в Dead Cells процедурно генерируются, что делает каждое прохождение уникальным. Игра также известна своей сложностью, требующей от игрока навыков и стратегического мышления.

![[DeadCells]](DeadCells.png)

В игре Dead Cells здоровье игрока является одним из ключевых показателей, которые определяют его выживаемость в битвах. 

Роль здоровья:
- Здоровье (HP) определяет общее количество повреждений, которое игрок может выдержать, прежде чем погибнет.
- Если здоровье игрока достигает нуля, он теряет жизнь и повторно начинает уровень с самого начала (если не выбран режим "Бессмертия").
- Здоровье влияет на игровую стратегию и подразумевает необходимость балансировки атак и защиты, чтобы избегать смертельных повреждений.

Изменение/появление здоровья:
- Здоровье может изменяться в ходе игры такими способами, как сбор аптечек, улучшение оружия или экипировка предметов, которые увеличивают максимальное здоровье игрока.
- При прохождении некоторых моментов игры или победе над боссами игрок может получить улучшения, которые повышают его максимальное здоровье.

Диапазон допустимых значений:
- Некоторые аптечки могут восстанавливать от 20 до 60% здоровья.
- Значение текущего здоровья может быть отображено в виде полоски здоровья на экране.


![[HP]](HP.png)

## Задание 2
### Генерация данных с помощью Python и их передача в google таблицу

Ход работы:
- Запустить Jupyter Notebook, создать новый .ipynb-файл.
- Добавить в директорию с новым .ipynb-файлом файл с ключом в формате json от сервисного аккаунта.
- В .ipynb файле на языке Python реализовать передачу данных в созданную ранее google-таблицу. Реализация на Python рассчитывает темп инфляции для экономической модели:
```python
import gspread
import numpy as np
gc = gspread.service_account(filename='unitydatascience-400812-c4e9504cacaa.json')
sh = gc.open("UnityWorkshop2")
health_values = np.random.randint(100, 2000, 11)
percent = [0.2, 0.5, 0.7]
val_percent = 0
mon = list(range(1,11))
i = 0

while i <= len(mon):
    i += 1
    if i == 0:
        continue
    else:
        if health_values[i-1] < 1000:
            tempInf = round(health_values[i-1] + (health_values[i-1] * percent[2]), 2)
            val_percent = percent[2] * 100
        elif 1000 <= health_values[i-1] <= 1800:
            tempInf = round(health_values[i-1] + (health_values[i-1] * percent[1]), 2)
            val_percent = percent[1] * 100
        else:
            tempInf = round(health_values[i-1] + (health_values[i-1] * percent[0]), 2)
            val_percent = percent[0] * 100
            
        tempInf = str(tempInf)
        tempInf = tempInf.replace('.',',')
        sh.sheet1.update(('A' + str(i)), str(i))
        sh.sheet1.update(('B' + str(i)), str(health_values[i-1]))
        sh.sheet1.update(('C' + str(i)), str(tempInf))
        sh.sheet1.update(('D' + str(i)), str(val_percent))
print("Finish")
```

Результатом запись данных в google-таблицу:

![[Data]](Data.png)

>"B" - Значение здоровья
>"C" - Здоровье + процент
>"D" - Процент 

![[Diagram]](Diagram.png)

## Задание 3
### Настройка воспроизведения звуковых файлов, описывающих динамику изменения выбранной переменной на сцене Unity.


```c#
using System.Collections;  
using System.Collections.Generic;  
using UnityEngine;  
using UnityEngine.Networking;  
using SimpleJSON;  
  
public class NewBehaviourScript : MonoBehaviour  
{  
    public AudioClip goodSpeak;  
    public AudioClip normalSpeak;  
    public AudioClip badSpeak;  
    private AudioSource selectAudio;  
    private Dictionary<string, float> dataSet = new Dictionary<string, float>();  
    private bool statusStart = false;  
    private int i = 1;  
  
    // Start is called before the first frame update  
    void Start()  
    {        StartCoroutine(GoogleSheets());  
    }  
    // Update is called once per frame  
    void Update()  
    {        if (dataSet["Mon_" + i.ToString()] >= 1000 & statusStart == false & i != dataSet.Count)  
        {            StartCoroutine(PlaySelectAudioBad());  
            Debug.Log(dataSet["Mon_" + i.ToString()]);  
        }  
        if (dataSet["Mon_" + i.ToString()] > 1000 & dataSet["Mon_" + i.ToString()] < 1800 & statusStart == false & i != dataSet.Count)  
        {            StartCoroutine(PlaySelectAudioNormal());  
            Debug.Log(dataSet["Mon_" + i.ToString()]);  
        }  
        if (dataSet["Mon_" + i.ToString()] <= 1800  & statusStart == false & i != dataSet.Count)  
        {            StartCoroutine(PlaySelectAudioGood());  
            Debug.Log(dataSet["Mon_" + i.ToString()]);  
        }    }  
    IEnumerator GoogleSheets()  
    {        Debug.Log("GoogleSheets");  
        UnityWebRequest curentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/19pxv2mfHpZnoGYDIbAzTMhcxy-hMyKXGKhmHiq99uLg/values/Лист1?key=AIzaSyApLNjAREKj2UMMr7vbz8JwK_AimHbHyfU");  
        yield return curentResp.SendWebRequest();  
        string rawResp = curentResp.downloadHandler.text;  
        var rawJson = JSON.Parse(rawResp);  
        foreach (var itemRawJson in rawJson["values"])  
        {            var parseJson = JSON.Parse(itemRawJson.ToString());  
            var selectRow = parseJson[0].AsStringList;  
            dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[2]));  
        }    }  
    IEnumerator PlaySelectAudioGood()  
    {        statusStart = true;  
        selectAudio = GetComponent<AudioSource>();  
        selectAudio.clip = goodSpeak;  
        selectAudio.Play();  
        yield return new WaitForSeconds(3);  
        statusStart = false;  
        i++;  
    }    IEnumerator PlaySelectAudioNormal()  
    {        statusStart = true;  
        selectAudio = GetComponent<AudioSource>();  
        selectAudio.clip = normalSpeak;  
        selectAudio.Play();  
        yield return new WaitForSeconds(3);  
        statusStart = false;  
        i++;  
    }    IEnumerator PlaySelectAudioBad()  
    {        statusStart = true;  
        selectAudio = GetComponent<AudioSource>();  
        selectAudio.clip = badSpeak;  
        selectAudio.Play();  
        yield return new WaitForSeconds(4);  
        statusStart = false;  
        i++;  
    }
    }
```

## Выводы

В процессе работы мы освоили метод передачи данных между Google Sheets и Unity, а также визуализацию этих данных. Нам удалось заполнить данные в Google Sheets с использованием языка программирования Python.

|Plugin|README|
|---|---|
|Dropbox|[plugins/dropbox/README.md][PlDb]|
|GitHub|[plugins/github/README.md][PlGh]|
|Google Drive|[plugins/googledrive/README.md][PlGd]|
|OneDrive|[plugins/onedrive/README.md][PlOd]|
|Medium|[plugins/medium/README.md][PlMe]|
|Google Analytics|[plugins/googleanalytics/README.md][PlGa]| 
