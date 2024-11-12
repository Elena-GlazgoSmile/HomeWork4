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

![Снимок экрана 2024-11-12 183143](https://github.com/user-attachments/assets/f71cfff0-e6ee-466b-949d-ca45ee1537aa)

Одно тестирование - одна ошибка.

![Снимок экрана 2024-11-12 183246](https://github.com/user-attachments/assets/263d7a60-9f55-471a-86d0-8409d7cce2f2)

Дальнейшие действия я буду писать менее подробно, но будут данные вывода, количество тестов и их скриншоты, пока тотальных ошибок не станет 0.

Два тестирования: 
- первое - две ошибки
- второе - две ошибки.

![Снимок экрана 2024-11-12 201420](https://github.com/user-attachments/assets/3bcd8b42-4fcd-40a8-a0be-4cea3e124437)

Три тестирования:
- первое - одна ошибка,
- второе - одна ошибка,
- третье - 0 ошибок.

![Снимок экрана 2024-11-12 202042](https://github.com/user-attachments/assets/95cc3c0b-dd85-4a2e-8a4e-c52810d7b4c2)

![Снимок экрана 2024-11-12 202037](https://github.com/user-attachments/assets/1c67f244-368b-4899-8ad5-2d25bf6eeba3)

Вывод: три тестирования понадобилось, чтобы обучить перцептрон логическому OR.

Дальнейшие действия я не буду рассматривать настолько детально, как первое, но буду прикреплять скриншоты своей работы и немного объяснять, что хотела этим скриншотом сказать.

Следующее логическое выражение - AND.
Создала 3D-объект цилиндр, чтобы навесить на него код перцептрон.

![Снимок экрана 2024-11-12 213410](https://github.com/user-attachments/assets/6833957c-7efb-4215-9083-ff15d1f57628)

Вводимые данные:

- 0 0
- 0 1
- 1 0
- 1 1

Ожидаемый вывод:

- 0
- 0
- 0
- 1
  
Одно тестирование - 2 ошибки.

![Снимок экрана 2024-11-12 213709](https://github.com/user-attachments/assets/aa2060f0-5de7-4cd0-bef1-acb1a2c157b6)

Два тестирования: 
- первое - две ошибки
- второе - две ошибки.
  
![Снимок экрана 2024-11-12 213841](https://github.com/user-attachments/assets/246c7ce8-51a4-4173-9d0b-aaefd2ef1c1d)

Три тестирования:
- первое - 3 ошибки,
- второе - 2 ошибки,
- третье - 3 ошибки.
  
  ![Снимок экрана 2024-11-12 213957](https://github.com/user-attachments/assets/90b72eb3-a042-485d-a031-66ad122bd1bf)
  
Четыре тестирования:
- первое - 3 ошибки,
- второе - 2 ошибки,
- третье - 3 ошибки.
- четвертое - 2 ошибки.

![Снимок экрана 2024-11-12 214309](https://github.com/user-attachments/assets/24638169-8273-483c-a38f-f40601a7eed3)


Пять тестирований:
- первое - 1 ошибка,
- второе - 3 ошибки,
- третье - 1 ошибка,
- четвертое - 0 ошибок,
- пятое - 0 ошибок.
  
![Снимок экрана 2024-11-12 214635](https://github.com/user-attachments/assets/f892be37-ee99-48b3-8c1e-dd9a5f02270e)

Вывод: пять тестирований понадобилось, чтобы обучить перцептрон логическому AND.

Следующее логическое выражение - NAND.
Создала 3D-объект сферу, чтобы навесить на неё код перцептрон.

![Снимок экрана 2024-11-12 220426](https://github.com/user-attachments/assets/4694c27d-2118-440d-b194-f15dccb882cd)

Вводимые данные:

- 0 0
- 0 1
- 1 0
- 1 1

Ожидаемый вывод:

- 1
- 1
- 1
- 0

Одно тестирование - 3 ошибки.

![Снимок экрана 2024-11-12 220544](https://github.com/user-attachments/assets/b8bcbeb8-a40d-4dc2-ac3d-e496c9c80f59)

Два тестирования: 
- первое - 1 ошибка,
- второе - 2 ошибки.
  
![Снимок экрана 2024-11-12 220719](https://github.com/user-attachments/assets/77022fce-4afd-4784-9cd9-03f1edc8b5aa)

Три тестирования:
- первое - 3 ошибки,
- второе - 2 ошибки,
- третье - 3 ошибки.
  
  ![Снимок экрана 2024-11-12 220908](https://github.com/user-attachments/assets/f14a3ff8-a2f0-4001-bba3-a6ab246460f6)

Четыре тестирования:
- первое - 3 ошибки,
- второе - 2 ошибки,
- третье - 3 ошибки.
- четвертое - 2 ошибки.

![Снимок экрана 2024-11-12 221136](https://github.com/user-attachments/assets/9872c30a-2a1b-4c26-a085-bc23de51da8f)

Пять тестирований:
- первое - 2 ошибки,
- второе - 3 ошибки,
- третье - 3 ошибки,
- четвертое - 2 ошибки,
- пятое - 1 ошибка.

  ![Снимок экрана 2024-11-12 221337](https://github.com/user-attachments/assets/14c330f6-2e45-4257-848d-ab0e59e400a8)

Шесть тестирований:
- первое - 1 ошибка,
- второе - 3 ошибки,
- третье - 3 ошибки,
- четвертое - 2 ошибки,
- пятое - 1 ошибка,
- шестое - 0 ошибок.
  
![Снимок экрана 2024-11-12 221811](https://github.com/user-attachments/assets/2b8f2fef-db32-4342-af9d-925eb840cd72)

Следующее логическое выражение - XOR.

Вводимые данные:

- 0 0
- 0 1
- 1 0
- 1 1

Ожидаемый вывод:

- 0
- 1
- 1
- 0

Одно тестирование - одна ошибка.

Два тестирования: 
- первое - две ошибки
- второе - две ошибки.

Три тестирования:
- первое - одна ошибка,
- второе - одна ошибка,
- третье - 0 ошибок.
  
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
