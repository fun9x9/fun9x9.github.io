---
title: "파이썬 문법 팁 (Python Syntax Tips)"
date: 2019-07-16T13:36:00+09:00
categories:
  - IT
tags:
  - tips
  - python
  - syntax
---

- Tested Environment : python3

- 참고:
  - [https://mingrammer.com/underscore-in-python/](https://mingrammer.com/underscore-in-python/)
  - [https://www.geeksforgeeks.org/10-essential-python-tips-tricks-programmers/](https://www.geeksforgeeks.org/10-essential-python-tips-tricks-programmers/)
  - [https://www.techbeamers.com/essential-python-tips-tricks-programmers/](https://www.techbeamers.com/essential-python-tips-tricks-programmers/)
-  [https://realpython.com/python-tricks/](https://realpython.com/python-tricks/)
 
## 언더스코어(underscore, _) 
- 다음과 같은 5가지 상황에서 사용된다.
  - 인터프리터(Interpreter)에서 마지막 실행 결과값을 저장 
  - 값을 무시할 때
  - 변수나 함수명에 특별한 의미, 기능을 부여
  - 국제화/지역화 함수로 사용
  - 숫자 리터럴값의 자릿수 구분을 위한 구분자

## 조건문
```python
if condition:
    x = 1
else:
    x = 0
```
위 아래 동일
```python
x = 1 if condition else 0
```
 
## 파일 오픈: with문
```python
fd = open('text.txt', 'r')
str = fd.read()
fd.close()
```
위 아래 동일
```python
with open('text.txt', 'r') as fd:
    str = fd.read()
``` 

## enumerate
```python
names = ['Corey', 'Chiris', 'Dave', 'Travis']
index = 0
for name in names:
    print(index, name)
    index += 1
```
위 아래 동일
```python
names = ['Corey', 'Chiris', 'Dave', 'Travis']
for index, name in enumerate(names):
    print(index, name)
```
```python
names = ['Corey', 'Chiris', 'Dave', 'Travis']
# 인덱스 시작을 1부터
for index, name in enumerate(names, start=1):
    print(index, name)
``` 

## zip
```python
names = ['Peter Parker', 'Clark Kent', 'Wade Wilson', 'Bruce Wayne']
heroes = ['Spiderman', 'Superman', 'Deadpool', 'Batman']
for index, name in enumerate(names):
    hero = heroes[index]
    print(name + ' is actually ' + hero)
```
위 아래 동일
```python
names = ['Peter Parker', 'Clark Kent', 'Wade Wilson', 'Bruce Wayne']
heroes = ['Spiderman', 'Superman', 'Deadpool', 'Batman']
for name, hero in zip(names, heroes):
    print(name + ' is actually ' + hero)
```
---
```python
names = ['Peter Parker', 'Clark Kent', 'Wade Wilson', 'Bruce Wayne']
heroes = ['Spiderman', 'Superman', 'Deadpool', 'Batman']
universes = ['Marvel', 'DC', 'Marvel', 'DC']
for name, hero, universe in zip(names, heroes, universes):
    print(name + ' is actually ' + hero + ' from ' + universe)
```
위 아래 동일
```python
names = ['Peter Parker', 'Clark Kent', 'Wade Wilson', 'Bruce Wayne']
heroes = ['Spiderman', 'Superman', 'Deadpool', 'Batman']
universes = ['Marvel', 'DC', 'Marvel', 'DC']
for values in zip(names, heroes, universes):
    print(values[0] + ' is actually ' + values[1] + ' from ' + values[2])
```

## 튜플/리스트 언패킹(tuple unpacking)
- 리스트도 동일하게 동작

```python
a, b = (1, 2) # a = 1, b = 2

a, _ = (1, 2) # a = 1

a, b, *c = (1, 2, 3, 4, 5) # a = 1, b = 2, c = [3, 4, 5]

a, b, *_ = (1, 2, 3, 4, 5) # a = 1, b = 2

a, b, *c, d = (1, 2, 3, 4, 5, 6) # a = 1, b = 2, c = [3, 4, 5], d = 6

a, b, *_, d = (1, 2, 3, 4, 5, 6, 7) # a = 1, b = 2, d = 6
```
 
## 변수 바꾸기 (In-Place Swapping)
```python
x = 10
y = 20

tmp = y
y = x
x = tmp
```
위 아래 동일
```python
x = 10
y = 20

x, y = y, x
```
위 아래 동일
```python
x, y = 10, 20

x, y = y, x
```
 
## 딕셔너리
```python
ages = {
    'Mary'      : 31,
    'Jonathan'  : 28
}

age = ages['Dick']
```

```python
ages = {
    'Mary'      : 31,
    'Jonathan'  : 28
}

if 'Dick' in ages:
    age = ages['Dick']
else:
    age = 'Unknown'
```

```python
ages = {
    'Mary'      : 31,
    'Jonathan'  : 28
}
# Good way
age = ages.get('Dick', 'Unknown')
```
 

## 문자열 뒤집기(Reversing a string)
```python
a ="GeeksForGeeks"
print("Reverse is", a[::-1]) # Reverse is skeeGroFskeeG
```
 
## 리스트에서 스트링 만들기(Create a single string from all the elements in list)
```python
a = ["Geeks", "For", "Geeks"] 
print(" ".join(a)) # Geeks For Geeks
```
 
## 비교연산자 연결(Chaining Of Comparison Operators.)
```python
n = 10
result = 1 < n < 20 # True
result = 1 > n <= 9 # False
```

## 모듈 경로 출력 (Print The File Path Of Imported Modules.)
```python
import os; 
import socket; 

print(os)      #<module 'os' from '/usr/lib/python3.5/os.py'>
print(socket)  #<module 'socket' from '/usr/lib/python3.5/socket.py'>
```

## 리스트에서 최빈도값 찾기(Find The Most Frequent Value In A List.)
```python
test = [1, 2, 3, 4, 2, 2, 3, 1, 4, 4, 4] 
print(max(set(test), key = test.count)) # 4
```
 
## IF 단순화
``` python
if m in [1,3,5,7]:
```
위 아래 동일
``` python
if m==1 or m==3 or m==5 or m==7:
```
 
## 딕셔너리 스위치처럼 사용하기(Use A Dictionary To Store A Switch.)
```python
stdcalc = {
  'sum': lambda x, y: x + y,
  'subtract': lambda x, y: x - y
}

print(stdcalc['sum'](9,3))      # 12
print(stdcalc['subtract'](9,3)) # 6
```
 
## 재귀호출 제한값 바꾸기 (Reset Recursion Limit.)
- 기본값 : 1000

```python
import sys

x=1001
print(sys.getrecursionlimit())  # 1000

sys.setrecursionlimit(x)
print(sys.getrecursionlimit())  # 1001
```

## 여러 문자열 포함여부 확인(In Line Search For Multiple Prefixes In A String.)
```python
print("http://www.google.com".startswith(("http://", "https://"))) # True
print("http://www.google.co.uk".endswith((".com", ".co.uk"))) # True
```

## 딕셔너리 합치기 (merge dictionaries)
```python
x = {'a': 1, 'b': 2}
y = {'b': 3, 'c': 4}
z = {**x, **y}
print(z) # {'a': 1, 'b': 3, 'c': 4}
```

## 튜플, 딕셔너리 함수 아규먼트 전달 (function argument unpacking)
```python
def myfunc(x, y, z):
    print(x, y, z)

tuple_vec = (1, 0, 1)
dict_vec = {'x': 1, 'y': 0, 'z': 1}

myfunc(*tuple_vec)  # 1, 0, 1
myfunc(**dict_vec)  # 1, 0, 1
```

## 람다(lambda)
```python
add = lambda x, y: x + y
add(1, 2)
```
```python
def add(x, y):
    return x + y
add(1, 2)
```
```python
(lambda x, y: x + y)(1, 2)
```