# АНАЛИЗ ДАННЫХ В РАЗРАБОТКЕ ИГР [in GameDev]
Отчет по лабораторной работе #4 выполнил(а):
- Власова Елена Васильевна
- РИ230931
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
- в проекте Unity реализовать перцептрон, который умеет производить вычисления:
 1. OR | дать комментарии о корректности работы
 2. AND | дать комментарии о корректности работы
 3. NAND | дать комментарии о корректности работы
 4. XOR | дать комментарии о корректности работы

- Задание 2.
- Построить графики зависимости количества эпох от ошибки  обучения. Указать от чего зависит необходимое количество эпох обучения.
- Задание 3.
- Построить визуальную модель работы перцептрона на сцене Unity. //можно использовать GameObject Сube и метод OnCollisionEnter.
- Выводы.
- ✨Magic ✨

## Цель работы : «ПЕРЦЕПТРОН»

## Задание 1
### в проекте Unity реализовать перцептрон, который умеет производить вычисления:
 1. OR | дать комментарии о корректности работы
 2. AND | дать комментарии о корректности работы
 3. NAND | дать комментарии о корректности работы
 4. XOR | дать комментарии о корректности работы
    
Ход работы:
Для начала работы я создала новый 3D-проект под названием My Project (1), а также переименовала существующую сцену на WorkWithPerceptron

![Снимок экрана 2024-11-12 151251](https://github.com/user-attachments/assets/51e3fbe0-b61f-49c0-87af-00ef051a5bb8)

Далее я создала новый объект на сцене - 3D куб под названием Zero, чтобы в дальнейшем на него навесить код perceptron

![Снимок экрана 2024-11-12 161406](https://github.com/user-attachments/assets/d39bba93-4539-47b4-9ea1-a6526a235cb9)

Затем я скачала файл Perceptron.cs из репозитория и добавила его к себе в проект, а затем в свойства куба

```
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

![Снимок экрана 2024-11-12 162632](https://github.com/user-attachments/assets/1d0f7a07-94fb-413d-8a10-90c048a7278d)

Первым делом я решила реализовать логическое OR.
Сначала я в инспекторе добавила 4 тренировочных теста, в каждом из которых также добавила два элемента на вход, о тренировочных элементах каждого теста напишу ещё раз здесь:

- 0 0
- 0 1
- 1 0
- 1 1
  
  А также указала значения, которые ожидаемы для каждого из тестов, проведённых для этих элементов, ожидаемые значения тоже напишу здесь:
  
- 0
- 1
- 1
- 1
  
В самом коде изменила значение тренировочных тестов с 8 на 1, в последующих тестах буду увеличивать количество тренировочных тестов на 1 и фиксировать, сколько тотальных ошибок произошло по мере увеличения тестов.

## Задание 2
### Построить графики зависимости количества эпох от ошибки  обучения. Указать от чего зависит необходимое количество эпох обучения.

Ход работы:




## Задание 3
### Построить визуальную модель работы перцептрона на сцене Unity. //можно использовать GameObject Сube и метод OnCollisionEnter.

Ход работы:




## Выводы

Абзац умных слов о том, что было сделано и что было узнано.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
