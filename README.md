# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #4 выполнил(а):
- Амосова Варвара Ивановна
- РИ - 210940
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

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
Познакомиться с работой перцептрона при помощи движка Unity. Научиться применять его на практике, реализовать перцептрон который умеет решать логические операции.

## Задание 1
### В проекте реализовать перцептрон, который умеет производить вычисления: OR, AND, NAND, XOR, и дать комментарии о кореектности работы.

Ход работы:

- В начале выполнения лабораторной работы создадим пустой проект на Unity. Для этого создаем пустой объект и прикрепляем код из методических указаний:

```py
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public class TrainingSet
{
public double[] input;
public double output;
}

public class Perceptron : MonoBehaviour {

public TrainingSet[] ts;
double[] weights = {0,0};
double bias = 0;
double totalError = 0;

double DotProductBias(double[] v1, double[] v2)
{
if (v1 == null || v2 == null)
return -1;

if (v1.Length != v2.Length)
return -1;

double d = 0;
for (int x = 0; x < v1.Length; x++)
{
d += v1[x] * v2[x];
}

d += bias;

return d;
}

double CalcOutput(int i)
{
double dp = DotProductBias(weights,ts[i].input);
if(dp > 0) return(1);
return (0);
}

void InitialiseWeights()
{
for(int i = 0; i < weights.Length; i++)
{
weights[i] = Random.Range(-1.0f,1.0f);
}
bias = Random.Range(-1.0f,1.0f);
}

void UpdateWeights(int j)
{
double error = ts[j].output - CalcOutput(j);
totalError += Mathf.Abs((float)error);
for(int i = 0; i < weights.Length; i++)
{
weights[i] = weights[i] + error*ts[j].input[i];
}
bias += error;
}

double CalcOutput(double i1, double i2)
{
double[] inp = new double[] {i1, i2};
double dp = DotProductBias(weights,inp);
if(dp > 0) return(1);
return (0);
}

void Train(int epochs)
{
InitialiseWeights();

for(int e = 0; e < epochs; e++)
{
totalError = 0;
for(int t = 0; t < ts.Length; t++)
{
UpdateWeights(t);
Debug.Log("W1: " + (weights[0]) + " W2: " + (weights[1]) + " B: " + bias);
}
Debug.Log("TOTAL ERROR: " + totalError);
}
}

void Start () {
Train(8);
Debug.Log("Test 0 0: " + CalcOutput(0,0));
Debug.Log("Test 0 1: " + CalcOutput(0,1));
Debug.Log("Test 1 0: " + CalcOutput(1,0));
Debug.Log("Test 1 1: " + CalcOutput(1,1));
}

void Update () {

}
}
```

![0](https://user-images.githubusercontent.com/114309754/204655183-07412c62-71e0-4435-b506-4b5cde6dd569.png)


Таблицы истинности логических операций:

- Операция OR:

| A | B | A OR B |
| ------ | ------ | ------|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

- Операция AND:

| A | B | A AND B |
| ------ | ------ | ------|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

- Операция NAND :

| A | B | A NAND B |
| ------ | ------ | ------|
| 0 | 0 | 1 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

- Операция XOR:

| A | B | A XOR B |
| ------ | ------ | ------|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

### Реализация операции OR.

Установим значения для скрипта согласно таблице:

![1](https://user-images.githubusercontent.com/114309754/204655227-0e178848-f9cc-4f30-9a14-dd65e925092e.png)

Для начала зададим 1 обучающую эпоху и посмотрим на результаты наших тестов:

![2022-11-29_23-24-07](https://user-images.githubusercontent.com/114309754/204657388-6eed6991-4376-48d3-8cba-49df4ee2a276.png)


Значение Total Error отвечает за обучение перцептрона: если оно отлично от нуля, то модель не обучилась, если же ноль - модель успешно обучилась. По результатам первой эпохи заметим, что значение Total Error отлично от нуля, соответственно, обучение неуспешно.

Зададим 4 эпохи обучения:

![2022-11-29_23-25-03](https://user-images.githubusercontent.com/114309754/204657878-2d2e7cb2-11db-4922-a3b3-60f166054615.png)
![2022-11-29_23-25-08](https://user-images.githubusercontent.com/114309754/204657884-b0db4f67-6513-48e4-b523-5ab76fd99a63.png)

В данном случае даже при четырех эпохах обучение неудачно. Повторим попытку:

![2022-11-29_23-25-53](https://user-images.githubusercontent.com/114309754/204658047-ff946876-9a94-4de1-b147-4c1a363c6c70.png)
![2022-11-29_23-26-09](https://user-images.githubusercontent.com/114309754/204658053-555ff8dc-e53a-4b9f-9eda-774e9e4dce10.png)

На данном примере видно, что при четвертой эпохе обучения Total Error равняется нулю, а значит, обучение завершилось успешно, о чем также свидетельствуют корректные результаты теста ниже.
В таком случае можем сделать вывод, что не всегда 4 эпохи обучения достаточно. На примерах было продемонстрировано, что и при 4 итерациях значение Total Error отлично от нуля и программа работает некорректно.


### Реализация операции AND.

Установим значения для скрипта согласно таблице:

![1](https://user-images.githubusercontent.com/114309754/204658875-a4b33666-20b2-414b-b72f-1162bb5bf736.png)

Для начала зададим 1 обучающую эпоху и посмотрим на результаты наших тестов:

![2022-11-29_23-28-55](https://user-images.githubusercontent.com/114309754/204659005-dbe85d32-5280-426f-bfe2-8e484a7b49f2.png)

Значение Total Error отлично от нуля, соответственно, обучение также неуспешно.

Зададим 4 эпохи обучения:

![2022-11-29_23-31-09](https://user-images.githubusercontent.com/114309754/204659163-21c132f9-3783-466f-980e-37c54fbe64a1.png)
![2022-11-29_23-31-16](https://user-images.githubusercontent.com/114309754/204659171-902da913-8456-4868-ba16-9ad8afb0dcf7.png)

На данном примере видно, что при четвертой эпохе обучения Total Error равняется нулю, а значит, обучение завершилось успешно, о чем также свидетельствуют корректные результаты теста ниже.

При реализации данной функции получилось сразу обучить перцептрон с помощью четырех эпох.

### Реализация операции NAND.

Установим значения для скрипта согласно таблице:

![1](https://user-images.githubusercontent.com/114309754/204659701-e852777f-85a7-4b73-8473-5fc12345e4cc.png)

Для начала зададим 1 обучающую эпоху и посмотрим на результаты наших тестов:

![2022-11-29_23-33-14](https://user-images.githubusercontent.com/114309754/204659735-d67db9a7-3fac-489b-9ae3-de5ec6296d96.png)

Значение Total Error отлично от нуля, соответственно, обучение также неуспешно.

Зададим 4 эпохи обучения:

![2022-11-29_23-34-01](https://user-images.githubusercontent.com/114309754/204659773-23b3b1b1-c76c-45c1-b53b-5fc9177de7f3.png)
![2022-11-29_23-34-07](https://user-images.githubusercontent.com/114309754/204659777-e7e67d43-efd2-4272-8dc3-87d99b7e95fe.png)

На данном примере видно, что при четвертой эпохе обучения Total Error равняется нулю, а значит, обучение завершилось успешно, о чем также свидетельствуют корректные результаты теста ниже.

При реализации данной функции получилось сразу обучить перцептрон с помощью четырех эпох.

### Реализация операции XOR.

Установим значения для скрипта согласно таблице:

![2022-11-29_23-51-51](https://user-images.githubusercontent.com/114309754/204660008-71161a54-3234-4bbf-a274-9392ef01edbf.png)

Для начала зададим 1 обучающую эпоху и посмотрим на результаты наших тестов:

![2022-11-29_23-04-46](https://user-images.githubusercontent.com/114309754/204660243-fd0858f1-86c8-469b-a7b8-a61b87bb0baa.png)

Значение Total Error отлично от нуля, соответственно, обучение также неуспешно.

Зададим 4 эпохи обучения:

![2022-11-29_23-52-27](https://user-images.githubusercontent.com/114309754/204660302-5e678390-515b-4593-8ebc-70e101cd3261.png)
![2022-11-29_23-52-33](https://user-images.githubusercontent.com/114309754/204660310-f3958500-77c1-4781-943f-61f2fdbef51d.png)


На данном примере видно, что значение Total Error все равно отлично от нуля, соответственно, обучение также неуспешно.

Зададим 8 эпох обучения:

![2022-11-29_23-53-15](https://user-images.githubusercontent.com/114309754/204660359-85bdc3c0-9c4d-418d-af4f-1018396d510a.png)
![2022-11-29_23-53-22](https://user-images.githubusercontent.com/114309754/204660363-d0348143-8822-41de-a4a1-f2c64925c73c.png)
![2022-11-29_23-53-36](https://user-images.githubusercontent.com/114309754/204660366-eb70ba69-8413-4b57-a852-428647014dc8.png)

Total Error отлично от нуля. В ходе работы было выяснено, что начиная с третьей-четвертой эпохи значение Total Error всегда было равно 4. Соответственно перцептрон не может обучиться этой операции.

Таким образом, можем сделать вывод, что перцептрон может решать только линейные задачи, такие как AND, OR, NAND.


## Задание 2
### Построить графики зависимости количества эпох от ошибки обучения. Указать от чего зависит необходимое количество эпох обучения.

Для каждой операции было запущено 6 попыток с 8-ю эпохами обучения. На графиках указано среднее значение Total Error за 6 попыток.

- Операция OR:

![2022-11-30_00-48-31](https://user-images.githubusercontent.com/114309754/204661212-ff8bd269-cde6-47f0-90ea-b733848459cf.png)

- Операция AND:

![2022-11-30_00-48-58](https://user-images.githubusercontent.com/114309754/204661265-50d43d55-5276-4ae0-a826-6772efa801e0.png)

- Операция NAND:

![2022-11-30_00-47-57](https://user-images.githubusercontent.com/114309754/204661314-5be09c1d-26b0-4087-bacc-c50061815e3d.png)

- Операция XOR:

![2022-11-30_00-47-43](https://user-images.githubusercontent.com/114309754/204661290-ce03946b-086a-4865-abc8-dbd8c29bbe31.png)

## Задание 3
### Построить визуальную модель работы перцептрона на сцене Unity.

- Рассмотрим функцию AND. В начале выполнения данного задания была создана сцена с четырьмя сочетаниям, выражающимися сочетаниями кубов, где желтый куб - 0, синий куб - 1. Сочетания: 0-0, 0-1, 1-0, 1-1. 
- 
![1](https://user-images.githubusercontent.com/114309754/204662096-33c8c3e2-d9f7-4b47-a9f2-b783516d168d.png)

При их столкновении они оба окрасятся в цвет результата той или иной операции (желтый - 0, синий - 1).

в таком случае заведем новые переменные, которые будух хранить информацию об обозначении куба (0 или 1). И данные значения подставляем в функцию CalcOutput для определения результата операции после обучения модели.

Соответвенно:

```py
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public class TrainingSet
{
public double[] input;
public double output;
}


public class Perceptron : MonoBehaviour {
// public TrainingSet[] tsOR;
// public TrainingSet[] tsNAND;
public TrainingSet[] ts;

public double firstCube;
public double secondCube;
double[] weights = {0,0};
double bias = 0;
double totalError = 0;

double DotProductBias(double[] v1, double[] v2)
{
if (v1 == null || v2 == null)
return -1;

if (v1.Length != v2.Length)
return -1;

double d = 0;
for (int x = 0; x < v1.Length; x++)
{
d += v1[x] * v2[x];
}

d += bias;

return d;
}

double CalcOutput(int i, TrainingSet[] set)
{
double dp = DotProductBias(weights,set[i].input);
if(dp > 0) return(1);
return (0);
}

void InitialiseWeights()
{
for(int i = 0; i < weights.Length; i++)
{
weights[i] = Random.Range(-1.0f,1.0f);
}
bias = Random.Range(-1.0f,1.0f);
}

void UpdateWeights(int j, TrainingSet[] set)
{
double error = set[j].output - CalcOutput(j, set);
totalError += Mathf.Abs((float)error);
for(int i = 0; i < weights.Length; i++)
{
weights[i] = weights[i] + error * set[j].input[i];
}
bias += error;
}

double CalcOutput(double i1, double i2)
{
double[] inp = new double[] {i1, i2};
double dp = DotProductBias(weights,inp);
if(dp > 0) return(1);
return (0);
}

void Train(int epochs, TrainingSet[] set)
{
InitialiseWeights();

for(int e = 0; e < epochs; e++)
{
totalError = 0;
for(int t = 0; t < set.Length; t++)
{
UpdateWeights(t, set);
Debug.Log("W1: " + (weights[0]) + " W2: " + (weights[1]) + " B: " + bias);
}
Debug.Log("TOTAL ERROR: " + totalError);
}
}

void Start () {
// Train(8, tsOR);
// double tsOr0 = CalcOutput(0,0); //0
// double tsOr1 = CalcOutput(0,1); //1
// double tsOr2 = CalcOutput(1,0); //1
// double tsOr3 = CalcOutput(1,1); //1

// Train(8, tsNAND);
// double tsNAND0 = CalcOutput(0,0); //1
// double tsNAND1 = CalcOutput(0,1); //1
// double tsNAND2 = CalcOutput(1,0); //1
// double tsNAND3 = CalcOutput(1,1); //0
Train(8, ts);
}

void Update () {

}
private void OnTriggerEnter(Collider other) {
if (CalcOutput(firstCube, secondCube) == 0) {
other.gameObject.GetComponent<Renderer>().material.color = Color.black;
this.gameObject.GetComponent<Renderer>().material.color = Color.black;
}
else {
other.gameObject.GetComponent<Renderer>().material.color = Color.white;
this.gameObject.GetComponent<Renderer>().material.color = Color.white;
}
}
}
```

- Скриншоты работы кода:

![1](https://user-images.githubusercontent.com/114309754/204662620-1ca4181d-122b-4658-bbf0-4c19bd7ad831.png)
![2](https://user-images.githubusercontent.com/114309754/204662609-25493379-c8ea-4665-87c9-255498583c20.png)
![3](https://user-images.githubusercontent.com/114309754/204662617-b8ea230e-64a4-435a-a4ef-2030b6e4b0d5.png)

## Выводы
Лабораторная работа познакомила меня с такой моделью, как перцептрон, также я смогла реализовать такие логические операции как OR, AND, NAND, XOR. Понаблюдала за принципами перцептрона и его работой, изучила код на python. Также я поработала с различными логическими операциями и визуализировала их работу в проекте Unity, построила графики для оценки обучаемости модели той или иной операции.

| Plugin | README |
| ------ | ------ |
| GitHub | [plugins/github/README.md][PlGh] |
| Unity | [plugins/unity/README.md][PlU] |
| Visual Studio Code| [plugins/visualstudiocode/README.md][PlVSC] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
