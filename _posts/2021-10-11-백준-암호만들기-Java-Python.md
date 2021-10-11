---
date: 2021-10-11 21:00:00
layout: post
title: JAVA-Python-백준-암호만들기-BOJ1759
subtitle: 알고리즘 좀 꾸준히 해라 대훈아 제발
description: 백준 암호만들기 1759 BOJ
image: /assets/img/work/알고리즘배경.jpg
optimized_image: /assets/img/work/알고리즘배경2.jpg
category: code
tags:
  - Eclipse
  - Java
  - BOJ
  - Algorithm
  - Python
  - Pycharm
author: daehun
---

## 알고리즘 좀 매일 하자 매일 대훈아

Java로 조합 문제를 풀다가

최근 공부하고 있는 Python으로 해결 해보려고 했다.

Java와 Python 코드량의 차이가 어마어마하다.

solved Gold5로 Arraysort를 사용하면 별로 어렵지 않은 문제.

## BOJ 1759 암호만들기
문제출처 - <https://https://www.acmicpc.net/problem/1759>

### 문제

문제 설명
바로 어제 최백준 조교가 방 열쇠를 주머니에 넣은 채 깜빡하고 서울로 가 버리는 황당한 상황에 직면한 조교들은, 702호에 새로운 보안 시스템을 설치하기로 하였다. 이 보안 시스템은 열쇠가 아닌 암호로 동작하게 되어 있는 시스템이다.

암호는 서로 다른 L개의 알파벳 소문자들로 구성되며 최소 한 개의 모음(a, e, i, o, u)과 최소 두 개의 자음으로 구성되어 있다고 알려져 있다. 또한 정렬된 문자열을 선호하는 조교들의 성향으로 미루어 보아 암호를 이루는 알파벳이 암호에서 증가하는 순서로 배열되었을 것이라고 추측된다. 즉, abc는 가능성이 있는 암호이지만 bac는 그렇지 않다.

새 보안 시스템에서 조교들이 암호로 사용했을 법한 문자의 종류는 C가지가 있다고 한다. 이 알파벳을 입수한 민식, 영식 형제는 조교들의 방에 침투하기 위해 암호를 추측해 보려고 한다. C개의 문자들이 모두 주어졌을 때, 가능성 있는 암호들을 모두 구하는 프로그램을 작성하시오.


[입력]
첫째 줄에 두 정수 L, C가 주어진다. (3 ≤ L ≤ C ≤ 15) 다음 줄에는 C개의 문자들이 공백으로 구분되어 주어진다. 주어지는 문자들은 알파벳 소문자이며, 중복되는 것은 없다.

4 6
a t c i s w


[출력]
각 줄에 하나씩, 사전식으로 가능성 있는 암호를 모두 출력한다.
acis
acit
aciw
acst
acsw
actw
aist
aisw
aitw
astw
cist
cisw
citw
istw

### 라이브러리 쓰면 정말 쉬운데 파이썬은 충격이다.

그냥 입력받은 arr을 Array.sort해주고

visit[]를 사용해서 조합구문을 만들었다.

처음에 메모리 초과가 났는데 또 조합구문에 start

시작 start와 i 를 헷갈려서 Set을 써버리니 메모리초과가 났다.

그리고 해결한 코드.. 밑에는 Python으로 푼 코드

```java

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

public class 암호만들기 {
	static char mo[] = {'a','e','i','o','u'};//1개이상
	static char ja[] = {'b','c','d','f','g','h','j','k','l','m','n','p','q','r','s','t','v','w','x','y','z'};//2개이상
	static int L,C,check[];
	static char[] arr;
	static boolean visit[];
	public static void main(String[] args) throws IOException{
		
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine());
		L = Integer.parseInt(st.nextToken());
		C = Integer.parseInt(st.nextToken());
		arr = new char[C];
		st = new StringTokenizer(br.readLine());
		for(int i=0; i < arr.length; i++) {
			arr[i] = st.nextToken().charAt(0);
		}
		Arrays.sort(arr);
		check = new int[C];
		for(int i = 0 ; i< arr.length; i++) {
			char temp = arr[i];
			boolean flag = false;
			for(int j=0;j<mo.length;j++) {
				if(temp==mo[j]) {
					flag = true;
				}
			}
			if(flag)check[i]=1;
			else check[i]=2;
		}
		visit = new boolean[C];
		combi(0,0);
		
	}
//L==4 C==6  a c i s t w 
	private static void combi(int start , int m) {
		if(m==L) {
			String result = "";
			int count1 = 0;
			int count2 = 0;
			for(int i = 0 ; i < C; i++) {
				if(visit[i]) {
					result += arr[i];
					if(check[i]==1)count1++;
					else count2++;
				}
			}
			if(count1>=1&&count2>=2) {
				System.out.println(result);
			}
		}
		for(int i = start ; i< C ; i++) {
				visit[i]=true;
				combi(i+1,m+1);
				visit[i]=false;
		}
	}

}

```

## 파이썬이 충격적인 이유

이번 문제를 풀면서 combination 조합을 그냥

라이브러리로 제공한다

내가 계속해서 했던 조합 실수를 이렇게 해결할 수 있다니..


```python

from itertools import combinations

L,C = input().split()
L = int(L)
C = int(C)
arr = input().split()
result = sorted(arr)

combi = list(combinations(result,L))

for i in combi:
    count1=0
    count2=0
    for temp in i:
        if temp in "aeiou":
            count1+=1
        else:
            count2+=1

    if count1>=1 and count2>=2:
        print(''.join(i))

```

### 파이썬으로도 풀어보면서 헷갈린것들


```python

combi = list(combinations(result,L))

```

L 같은경우 입력받고나서 int(L)로 변수를 정수형으로

선언을 해줘야했다.

```python

for i in combi:
    count1=0
    count2=0
    for temp in i:
        if temp in "aeiou":
            count1+=1
        else:
            count2+=1

    if count1>=1 and count2>=2:
        print(''.join(i))

```

combi에 모든 조합들이 들어오고 i로 하나씩 받은다음

i에있는 리스트에서 temp로 하나씩 꺼내서 그냥 계산만하면 끝..

join의 경우 

''.join(리스트)를 이용하면 매개변수로 들어온 [A,B,C] 

를 'ABC'문자열로 합쳐주는 함수다.

'' 안에 구분자를 넣게되면 '구분자'.join

값과 값 사이에 구분자를 넣어준다.

진짜 알고리즘 문제는 파이썬으로 해결해야 하는것인가..

아무튼 꾸준히 알고를 하자.



