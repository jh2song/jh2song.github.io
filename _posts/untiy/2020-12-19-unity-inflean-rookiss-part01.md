---
title: "[C#과 유니티로 만드는 MMORPG 게임 개발 시리즈] Part1: C# 기초 프로그래밍 입문 강의 노트"
excerpt: "20/12/19 - 강의 노트"
tags: ["unity", "c#"]
categories: ["unity"]
last_modified_at: "2020-12-19"
toc: true
toc_sticky: true
---
# 개론
## 오리엔테이션
* 유니티와 언리얼 에셋 스토어에서 제공하는 무료 에셋들을 최대한 활용한다.
* 노베이스 상태에서 강의 진행
* 최종적인 목표: 500-1000명 사이의 MMORPG 인디 게임을 출시
&nbsp;

## 환경 설정
* 그냥 VS와 C# 환경 설치
* installer에서 _.NET 데스크톱 개발_ 체크.
* Visual Assist라는 어썸한 플러그인이 있으나 돈을 내야함 ㅡㅡ. 코드가 예쁘게 보인다.
&nbsp;

&nbsp;
# 데이터 갖고 놀기
## 정수 형식
* 주석, 정수 설명

|<center>bytes</center>|<center>C++</center>|<center>C#</center>|
|---|---|---|
|<center>4 bytes</center>|<center>long</center>|<center>int</center>|
|<center>8 bytes</center>|<center>long long</center>|<center>long</center>|

## 2진수, 10진수, 16진수
* 진법 설명
&nbsp;

## 정수 범위의 비밀
* 2의 보수 설명
&nbsp;

## 불리언, 소수, 문자, 문자열 형식
* float 변수는 rValue에 f를 붙여줘야됨(double이 c# 기본이라)

```c#
float f = 3.14f;
```

* char 자료형은 1 바이트가 아니라 2 바이트다.
&nbsp;

## 형식 변환
* 캐스팅 설명
&nbsp;

## 데이터 연산
* 연산자 설명
&nbsp;

## 비트 연산
* 최상위 비트에 1이 있는데 오른쪽 시프트 연산을 하면 최상위 비트 1은 유지된채로 시프트한다.
* 고로 이런 케이스에선 헷갈리지 않게 쓰기 위해 uint를 쓰는게 좋을 듯.
&nbsp;

## 데이터 마무리
* C#에는 타입을 정하지 않고 편하게(?) 사용하는 var 자료형이 있다. 그러나 C#, C++의 장점은 타입을 명시해서 코드를 보는 사람이 명확히 이해할 수 있는 것이다. 그러므로 비추!
&nbsp;

&nbsp;
# 코드의 흐름 제어
## if와 else
* if/else 설명
&nbsp;

## switch
* switch, 삼항 연산자 설명
&nbsp;

## 가위-바위-보 게임
```c#
Random rand = new Random();
int aiChoice = rand.Next(0,3);
int choice = Convert.ToInt32(Console.ReadLine());
```

## 상수와 열거형
```c#
int ROCK = 1;
const int PAPER = 2;
switch (choice)
{
    case ROCK: // ERROR!!!
        ~~~~~~ 
    case PAPER: // OK!!! Because "const"
        ~~~~~~
}
```
```c#
enum Choice // 열거형
{
    Rock = 1, // 기본은 0부터 시퀀스
    Paper = 2,
    Scissors = 0
}

static void Main(...)
{
    switch (choice)
    {
        // 타입이 enum이므로 int로 형변환
        case (int)Choice.Scissors:
            Console.WriteLine("가위");
            break;
        ....
    }
}
```

## while

```c#
do
{
    bool a = true; 
} while (a) // ERROR!!! 변수 a는 do 안에 {} 사이에서만 유효하므로!!
```

## for
* for문 설명
&nbsp;

## break, continue
* break, continue 설명
&nbsp;

## 함수

```c#
class Program
{
    // 복사 덧셈 함수
    static void CopyAddOne(int number)
    {
        number = number + 1;
    }

    // 참조 덧셈 함수
    static void RefAddOne(ref int number)
    {
        number = number + 1;
    }

    static void Main(String[] args)
    {
        // 복사(짭퉁), 참조(진퉁)
        int a = 0;
        
        Program.CopyAddOne(a);
        Console.WriteLine(a); // 출력: 0 (덧셈 안됨)

        Program.RefAddOne(ref a);
        Console.WriteLine(a); // 출력: 1 (덧셈 완료)
    }
}
```

## ref, out

```c#
// ref 예제
static void Swap(ref int a, ref int b)
{
    int temp = a;
    a = b;
    b = temp;
}
```

```c#
// out 예제
static void Divide(int a, int b, out int result1, out int result2)
{
    result1 = a / b;
    result2 = a % b;
}

static void Main(string[] args)
{
    int num1 = 10;
    int num2 = 3;

    int result1;
    int result2;
    Divide(10, 3, out result1, out result2);

    Console.WriteLine(result1); // 3
    Console.WriteLine(result2); // 1
}
```

## 오버로딩

```c#
static int Add(int a, int b, int c = 0, float d = 1.0f, double e = 3.0)
{
    Console.WriteLine("Add int 호출");
    return a + b + c;
}

static void Main(string[] args)
{
    Program.Add(1, 2, d:2.0f) // C++는 매개변수 순서를 지켜줘야 되지만 C#은 그렇지 않아도 된다!
}
```

## 연습 문제
* 구구단 출력, 피라미드 별찍기, 팩토리얼
&nbsp;

&nbsp;
# TextRPG
## 디버깅 기초
* breakpoint 조건식 검사 ('브레이크 포인트 우클릭' - '조건')
![image](https://user-images.githubusercontent.com/43688074/102947362-821d2300-4506-11eb-93df-5aafbdef6512.png)
* 다음에 실행될 문 변경 (화살표를 위 아래로 드래그해서 사용)
![image](https://user-images.githubusercontent.com/43688074/102947845-a62d3400-4507-11eb-8773-1e461cfc3e2d.png)
&nbsp;

## TextRPG 직업 고르기
