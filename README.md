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

Вывод: 6 тестирований понадобилось, чтобы обучить перцептрон логическому NAND.

Создала 3D-объект капсулу, чтобы навесить на неё код перцептрон.

![Снимок экрана 2024-11-12 222433](https://github.com/user-attachments/assets/7478eb83-9466-4c4e-9e68-2ee8291cfaa7)

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

Одно тестирование - 3 ошибки.

![Снимок экрана 2024-11-12 222726](https://github.com/user-attachments/assets/b63a3de4-d0de-4d10-b8d1-f0116b813e52)

Два тестирования: 
- первое - 3 ошибки
- второе - две ошибки.
  
![Снимок экрана 2024-11-12 222824](https://github.com/user-attachments/assets/cb3fbac6-1b40-47da-ab69-97c3e3f82be8)

Три тестирования:
- первое - 3 ошибки,
- второе - 3 ошибки,
- третье - 4 ошибки.
- 
![Снимок экрана 2024-11-12 222928](https://github.com/user-attachments/assets/9a7bfc3a-0725-4d6c-af6b-9d670967242f)

Четыре тестирования:
- первое - 2 ошибки,
- второе - 2 ошибки,
- третье - 3 ошибки.
- четвертое - 4 ошибки.

![Снимок экрана 2024-11-12 223147](https://github.com/user-attachments/assets/f43998d5-9cf7-45be-9531-f5f7200824a2)

Семь тестирований:
- первое - 4 ошибки,
- второе - 4 ошибки,
- третье - 4 ошибки,
- четвертое - 4 ошибки,
- пятое - 4 ошибки,
- шестое - 4 ошибки,
- седьмое - 4 ошибки.

Вывод: перцептрон невозможно обучить логической операции XOR, при увеличении количества тестов увеличивается количество ошибок и тем самым нет смысла проводить дальнейшие исследования. + даже при двух правильных ответах перцептрон показывает, что тотальных ошибок всё равно 4.

## Задание 2
### Построить графики зависимости количества эпох от ошибки  обучения. Указать от чего зависит необходимое количество эпох обучения.

Ход работы:

График обучения перцептрона логическому выражению OR

![Снимок экрана 2024-11-14 215112](https://github.com/user-attachments/assets/0bea3de5-086b-419d-a090-f9daba4ae579)

График обучения перцептрона логическому выражению AND

![Снимок экрана 2024-11-14 215125](https://github.com/user-attachments/assets/ffebc759-d303-4b1f-a8bc-a42c873156f4)

График обучения перцептрона логическому выражению NAND

![Снимок экрана 2024-11-14 215139](https://github.com/user-attachments/assets/9af9198c-47e1-4a75-8d98-05cce8afa303)

График обучения перцептрона логическому выражению XOR

![Снимок экрана 2024-11-14 215150](https://github.com/user-attachments/assets/a2f81825-8c99-4945-8e65-935b3c5188e9)

Я построила 4 графика зависимостей количества эпох от ошибки обучения по своим полученным данным, и теперь могу назвать, от чего зависит необходимое количество эпох обучения. Во-первых, зависит от того, является ли логическое выражение линейно зависимым. У меня на последнем графике с XOR показано, что логическому выражению XOR перцептрон не смог обучиться, хотя и прошло много эпох обучения, это связано с тем, что XOR - не имеет линейную зависимость. Так что первое - является ли логическое выражение линейно зависимым.

Во-вторых, в своих тестах я обучаю перцептрон на 4 примерах, хотя наверное их может быть больше или меньше.

В-третьих, если использовать не одно логическое выражение, а несколько, которые составляют сложную систему между переменными, то всё же эпох понадобится больше, чтобы обучить для выражения (A AND B) OR (A NAND C), чем для одинарного AND.

В-четвертых, можно использовать не 2 переменные, а несколько (хотя это можно больше отнести к пункту 3), я всё же хочу сказать, что для обучения A AND B AND C понадобится куда больше эпох, чем для моего A AND B

В-пятых, значения весов, которые устанавливаются рандомайзером, а также пороговое значение. Значение порога помогает перцептрону принять решение - либо 0, либо 1, в зависимости от того, будет ли он завышен, или занижен. + работа с смещением упрощает работу с математическим выражением, нежели пытаться изменить само пороговое значение.

## Задание 3
### Построить визуальную модель работы перцептрона на сцене Unity. //можно использовать GameObject Сube и метод OnCollisionEnter.

Ход работы:

Для начала я создала копию для каждой из 3D-фигур (у каждой копии в имени есть приставка One, у оригинала приставка Zero), а также сделала поле, которое не позволило бы упасть объектам, имеющим силу притяжения и остальные физические свойства.

![Снимок экрана 2024-11-15 115357](https://github.com/user-attachments/assets/2ea32b85-b0d8-4829-9141-f3f5b73767f1)

Также я думаю для лучшей визуализации создам несколько материалов, чтобы окрасить разные объекты. Например, белый и чёрный цвета у меня по дефолту будут использоваться для объектов, которые пока не взаимодействовали. Голубой цвет для плоскости, для лучшей наглядности и чтобы она не сливалась с белыми объектами. Зеленый и красный цвет будут использоваться, когда объекты будут взаимодействовать друг с другом. Например, если тотальных ошибок было больше нуля, то объекты при столкновении будут окрашиваться в красный, иначе в зелёный.

![Снимок экрана 2024-11-15 120156](https://github.com/user-attachments/assets/a26e33bb-c9aa-4c95-a745-005c6b4b8026)

Далее я на каждый из белых объектов накинула Rigiвbody и установила гравитацию 0.05, чтобы был эффект падения. А также, чтобы они не выпали за края видимой области, на поверхность тоже пришлось накинуть Rigidbody, но установить его как кинематический объект без физики.

![Снимок экрана 2024-11-15 122220](https://github.com/user-attachments/assets/cf67dc11-0ac1-4f65-97a8-9df4a644b8d7)

И хотя пока при столкновении объектов ничего не происходит, главное - я реализовала механизм работы их физического взаимодействия, а потом напишу код, в котором будет указано, что должно меняться для визуализации работы перцептрона.

![Снимок экрана 2024-11-15 122211](https://github.com/user-attachments/assets/996c7176-27c2-4624-94a6-b8e73b4abdbf)

Итак, я немного модифицировала изначальный код перцептрона, и теперь хочу немного пояснить модифицирование. 

Создала три публичных поля с типом Material: def - цвет по дефолту(он чёрный), wrong - цвет красный, если есть тотальные ошибки, right - зеленый цвет, если тотальных ошибок нет. Сделала их публичными, чтобы в инспекторе могла передать нужные Material. Например, оказалось, чтобы не окрасились из-за одного тестирования сразу все фигуры, не относящиеся к этому тестированию никакого отношения, дефолтных цветов пришлось делать на каждую фигуру свой.

![Снимок экрана 2024-11-15 223853](https://github.com/user-attachments/assets/9b8341bd-049e-40cb-ad0d-659f5fe2aee3)

![Снимок экрана 2024-11-15 224101](https://github.com/user-attachments/assets/c956148b-6f83-4913-955d-bbe9c2211b79)

Далее, чтобы происходило какое-то чудо, прежде чем узнать, прошли ли все тесты успешно, я решила добавить метод на считывание столкновения предметов OnCollisionEnter. И это же столкновение фигур является главным условием, чтобы у черных фигур поменялся цвет на цвет, интерпретирующий пройденные тесты. 

![Снимок экрана 2024-11-15 223846](https://github.com/user-attachments/assets/442d298d-6378-4db2-bd42-2a0fa2688d45)

Чтобы визуально было понятно, какая фигура отвечает за какое логическое выражение, я добавила текст над каждой тестовой фигурой на канве.

![Снимок экрана 2024-11-15 223827](https://github.com/user-attachments/assets/7dbf1057-0429-41eb-8222-6eb6d92459c8)

Вот как выглядит код перцептрона теперь.

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
	public double totalError = -1;
    	public Material def;
    	public Material wrong;
    	public Material right;

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

	public void Train(int epochs)
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

    void OnCollisionEnter(Collision collision)
    {
        if (totalError > 0)
            def.color = wrong.color;
        if (totalError == 0)
            def.color = right.color;

    }

    void Start () {
		Train(1);
        
        Debug.Log("Test 0 0: " + CalcOutput(0,0));
		Debug.Log("Test 0 1: " + CalcOutput(0,1));
		Debug.Log("Test 1 0: " + CalcOutput(1,0));
		Debug.Log("Test 1 1: " + CalcOutput(1,1));
		
	}
	
	void Update () {
        
    }
}
```

Сначала я поставила количество эпох обучения - 1. Для всех четырёх логических выражений тесты были провалены, поэтому все черные фигуры стали красными.

![Снимок экрана 2024-11-15 224349](https://github.com/user-attachments/assets/7991807e-3173-4b56-b1c9-ccaa3b549921)

Увеличу количество эпох до трёх. И теперь первое логическое выражение прошло все проверки, а это значит, что перцептрон обучился логическому выражению OR (опечаталась в игре, AND и OR должны быть поменяны местами)

![Снимок экрана 2024-11-15 224730](https://github.com/user-attachments/assets/67f596b7-977e-400b-9409-4c276838340f)

Количество эпох обучения 6 - и теперь перцептрон обучился почти всем логическим выражениям, кроме XOR.

![Снимок экрана 2024-11-15 230521](https://github.com/user-attachments/assets/2b28b348-7a3b-4b47-a231-b8102c31fad0)

Для достоверности того, что XOR с его нелинейной зависимостью невозможно обучить перцептрон, я повысила количество эпох до 100, и всё равно картина та же.

![Снимок экрана 2024-11-15 230716](https://github.com/user-attachments/assets/f73c2bde-81f0-42e6-bddc-9058626d751d)

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
