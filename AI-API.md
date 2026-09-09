# AI API 활용

## 1. 파이썬과 프로그래밍 기본1

```
표현식, 식별자, 키워드, 연산자 등 (기본 구성 요소)  
    ↓ 모여서
문장 (실행할 수 있는 최소 단위)
    ↓ 모여서
프로그램 (문장이 모인 것)
```

키워드 확인 코드
```
>>> import keyword
>>> print(keyword.kwlist)
```

스네이크 케이스 vs 캐멀 케이스

- 스네이크 케이스: 단어 사이에 언더 바(_) 기호를 붙임
- 캐멀 케이스: 단어의 첫 글자를 대문자로 만듦

**파이썬에서는 첫 번째 글자가 소문자면 무조건 스네이크 케이스, 대문자면 무조건 캐멀 케이스**

```
식별자
├─ 캐멀 케이스 (대문자로 시작) → 클래스
└─ 스네이크 케이스 (소문자로 시작)
    ├─ 뒤에 괄호가 있다 → 함수
    └─ 뒤에 괄호가 없다 → 변수
```

**자료(리터럴, literal)** 란 숫자든 문자든 어떠한 값 자체를 의미
```
1
10
"Hello"
```

---

프로그램의 역할: 자료를 처리
프로그래밍: 프로그램을 만드는 과정
자료: 프로그램이 처리할 수 있는 모든 것


객체지향 프로그래밍
```
현실세계
객체(Object) - 보이는 것, 보이지 않는 것
↓
일반화와 추상화
↓
컴퓨터세계
객체 - 클래스
```

- 카메라로 사진을 찍으면, 사진 → 자료(리터럴)
- 카메라에 저장 → 처리
- 카카오톡으로 친구에게 사진과 메시지를 보낸다. 사진, 메시지 → 자료
- 친구에게 전송 → 처리
- 게임의 경험치 → 자료
- 경험치를 증가하는 행위 → 처리

---

### 자료형과 문자열

**1. 자료와 자료형**

- 자료(data): 프로그램이 처리할 수 있는 모든 것 (사진, 메시지, 경험치 등)
- 자료형(data type): 자료를 기능과 역할에 따라 구분한 종류(string, number, boolean)
- 자료형 확인: type() 함수 사용

---

### 변수와 입력

**변수를 활용하는 세 가지 방법**

① **변수 선언** - 변수를 생성하는 것 (사용하겠다고 선언)

② **변수 할당** - 변수에 값을 넣는 것 (=기호 사용)

③ **변수 참조** - 변수에서 값을 꺼내 쓰는 것

**input() 함수의 입력 자료형**

input() 함수는 사용자가 무엇을 입력해도 결과는 **무조건 문자열 자료형**

**문자열을 숫자로 바꾸기 (형 변환/캐스트)**

문자열을 숫자로 변환해야 숫자 연산에 활용할 수 있음. 영어로는 **캐스트(cast)** 라고 부름

**ValueError 예외**

자료형을 변환할 때 '변환할 수 없는 것'을 변환하려 하면 **ValueError** 예외가 발생함

**① 숫자가 아닌 것을 숫자로 변환하려 할 때**


```python
int("안녕하세요")
float("안녕하세요")
```

```
[오류] ValueError: invalid literal for int() with base 10: '안녕하세요'
```

**② 소수점이 있는 숫자 형식의 문자열을 int()로 변환하려 할 때**

```python
int("52.273")
```

```
[오류] ValueError: invalid literal for int() with base 10: '52.273'
```

→ int는 정수형인데 부동 소수점이 있는 자료를 정수형으로 바꾸려 하면 오류가 발생함.

💡 문제 해결 Tip: 정수와 실수, 부동 소수점 구분이 어려울 때는 일단 float() 함수를 사용한다고 기억하세요. float()는 실수를 의미하고 실수는 정수도 포함하기 때문에 정수·실수 구분 없이 사용할 수 있습니다.

---

### 숫자와 문자열의 다양한 기능

### 1. 문자열의 format() 함수

format() 함수는 숫자를 문자열로 변환하는 함수. 중괄호{}를 포함한 문자열 뒤에 마침표(.)를 찍고 format() 함수를 사용하는데, **중괄호의 개수와 format 함수 괄호 안 매개변수의 개수는 반드시 같아야** 함.

```
format_a = "{}만 원".format(5000)
format_b = "파이썬 열공하여 첫 연봉{}만 원 만들기".format(5000)
format_c = "{}{}{}".format(3000, 4000, 5000)
format_d = "{}{}{}".format(1, "문자열", True)
```

### 2. format() 함수의 다양한 기능 (숫자)

**① 정수를 특정 칸에 출력하기**


```python
output_a = "{:d}".format(52)        # 기본
output_b = "{:5d}".format(52)       # 5칸
output_c = "{:10d}".format(52)      # 10칸
output_d = "{:05d}".format(52)      # 5칸, 빈칸 0으로 채움 (양수)
output_e = "{:05d}".format(-52)     # 5칸, 빈칸 0으로 채움 (음수)
```

**② 기호 붙여 출력하기**


```python
output_f = "{:+d}".format(52)    # +52 (양수)
output_g = "{:+d}".format(-52)   # -52 (음수)
output_h = "{: d}".format(52)    #  52 (양수: 기호 부분 공백)
output_i = "{: d}".format(-52)   # -52 (음수: 기호 부분 공백)
```

- `:+d`처럼 d 앞에 + 기호를 붙이면 양수/음수 기호를 표현할 수 있음 (양수는 + 기호로 표현)
- `:` d처럼 앞에 공백을 두면 기호 위치를 비워둔 채로 표현됨

### 3. 부동 소수점 출력의 다양한 형태

float 자료형의 숫자 출력을 강제로 지정할 때는 **:f**를 사용함.

```python
output_a = "{:f}".format(52.273)
output_b = "{:15f}".format(52.273)     # 15칸 만들기
output_c = "{:+15f}".format(52.273)    # 15칸에 부호 추가하기
output_d = "{:+015f}".format(52.273)   # 15칸에 부호 추가하고 0으로 채우기
```

**소수점 아래 자릿수 지정하기**


```python
output_a = "{:15.3f}".format(52.273)
output_b = "{:15.2f}".format(52.273)
output_c = "{:15.1f}".format(52.273)
```

### 4. 의미 없는 소수점 제거하기: `:g`

파이썬은 0과 0.0을 출력했을 때 내부적으로 자료형이 다르므로 서로 다른 값으로 출력함. 의미 없는 0을 제거한 후 출력하고 싶을 때는 **:g**를 사용함.


```python
output_a = 52.0
output_b = "{:g}".format(output_a)
print(output_a)   # 52.0print(output_b)   # 52
```

### 5. 대소문자 바꾸기: upper()와 lower()

- **upper()**: 문자열의 알파벳을 대문자로 변환
- **lower()**: 문자열의 알파벳을 소문자로 변환


```python
>>> a = "Hello Python Programming...!"
>>> a.upper()'HELLO PYTHON PROGRAMMING...!'
>>> a.lower()'hello python programming...!'
```

**— 파괴적 함수와 비파괴적 함수**

upper()나 lower() 함수를 사용하면 a의 문자열이 바뀔 것으로 생각하기 쉽지만, 절대로 원본은 변하지 않음. 이렇게 원본을 변화시키지 않는 함수를 **비파괴적 함수**라고 부름.

### 6. 문자열 양옆의 공백 제거하기: strip()

- **strip()**: 문자열 양옆의 공백을 제거
- **lstrip()**: 문자열 왼쪽의 공백을 제거
- **rstrip()**: 문자열 오른쪽의 공백을 제거

(공백이란 '띄어쓰기', '탭', '줄바꿈'을 모두 포함)

```python
>>> input_a = """      안녕하세요문자열의 함수를 알아봅니다"""
>>> print(input_a)      안녕하세요       ← 의도하지 않은 공백이 들어갑니다.문자열의 함수를 알아봅니다
>>> print(input_a.strip())안녕하세요문자열의 함수를 알아봅니다
```

📌 note: lstrip()과 rstrip() 함수는 거의 사용하지 않습니다.

### 7. 문자열의 구성 파악하기: isOO()

문자열이 소문자로만 구성되어 있는지, 알파벳으로만 구성되어 있는지, 숫자로만 구성되어 있는지 등을 확인할 때는 is로 시작하는 이름의 함수를 사용함.

- **isalnum()**: 문자열이 알파벳 또는 숫자로만 구성되어 있는지 확인
- **isalpha()**: 문자열이 알파벳으로만 구성되어 있는지 확인
- **isidentifier()**: 문자열이 식별자로 사용할 수 있는지 확인
- **isdecimal()**: 문자열이 정수 형태인지 확인
- **isdigit()**: 문자열이 숫자로 인식될 수 있는지 확인
- **isspace()**: 문자열이 공백으로만 구성되어 있는지 확인
- **islower()**: 문자열이 소문자로만 구성되어 있는지 확인
- **isupper()**: 문자열이 대문자로만 구성되어 있는지 확인

출력은 **True(맞다)** 또는 **False(아니다)** 로 나오는데, 이를 **불(boolean)** 이라고 부름.

```python
>>> print("TrainA10".isalnum())  True
>>> print("10".isdigit())  True
```

### 8. 문자열 찾기: find()와 rfind()

- **find()**: 왼쪽부터 찾아서 처음 등장하는 위치를 찾음.
- **rfind()**: 오른쪽부터 찾아서 처음 등장하는 위치를 찾음.

"안녕안녕하세요"라는 문자열에는 "안녕"이라는 문자열이 두 개.

```python
>>> output_a = "안녕안녕하세요".find("안녕")
>>> print(output_a)0
>>> output_b = "안녕안녕하세요".rfind("안녕")
>>> print(output_b)2
```

### 9. 문자열과 in 연산자

문자열 내부에 어떤 문자열이 있는지 확인하려면 **in 연산자**를 사용함. 출력은 True(맞다) 또는 False(아니다)로 나옴.

```python
>>> print("안녕" in "안녕하세요")True
>>> print("잘자" in "안녕하세요")False
```

### 10. 문자열 자르기: split()

문자열을 특정 문자로 자를 때는 **split()** 함수를 사용함.

```python
>>> a = "10 20 30 40 50".split(" ")
>>> print(a)['10', '20', '30', '40', '50']
```

실행 결과로 **리스트**가 나옴.

### 11.  f-문자열

```
name = "재연"
age = 28

print(f"이름은 {name}이고, 나이는 {age}살입니다.")
```

**format()과 비교**

```python
>>> "{}".format(10) #'10'
>>> f'{10}'  #'10'
```

💡 f-문자열이 format() 함수보다 간단하고 직관적이므로 f-문자열이 등장한 이후로는 대부분 format() 함수보다는 f-문자열을 사용하는 편입니다.

### 12. f-문자열보다 format() 함수를 사용하는 것이 더 좋은 경우

대부분의 상황에서는 f-문자열을 더 많이 사용함. 그러나 다음 두 가지 상황에서는 format() 함수를 더 많이 사용함.

**① 문자열 내용이 너무 많을 때**

f-문자열을 사용하면 어떤 데이터를 삽입하여 출력하는지 확인하기 위해 문자열을 모두 읽어야 한다는 문제가 있음.

```python
>>> name = "구름"
>>> age = 7
>>> """데이터{}을/를 출력해야 하는 경우가 있습니다.""".format(name, age)
```

→ format() 함수를 사용하면 문자열이 아무리 길어도 다음 줄만 보면 어떤 데이터를 출력하는지 쉽게 알 수 있음.

**② 데이터를 리스트에 담아서 사용할 때**


```python
data = ['별', 2, 'M', '서울특별시 강서구', 'Y']
```

f-문자열로 형식화해서 출력하려면 [ ] 기호로 일일이 리스트 요소에 접근해야 하지만,


```python
>>> f"""이름:{data[0]}나이:{data[1]}성별:{data[2]}지역:{data[3]}중성화 여부:{data[4]}"""
```

format() 함수와 전개 연산자(*)를 사용하면 다음과 같이 더 간단하게 리스트 요소를 한꺼번에 출력할 수 있음.


```python
>>> """이름:{}나이:{}성별:{}지역:{}중성화 여부:{}""".format(*data)
```

→ 전개 연산자를 사용하여 리스트 내용을 전개함.

---

### raise NotImplementedError

`pass` 키워드를 입력해 놓아도 나중에 잊어버리는 경우가 많음. `raise` 키워드와 미구현 상태를 표현하는 `NotImplementedError`를 조합해 `raise NotImplementedError`를 사용하면 "아직 구현하지 않은 부분이에요!"라는 오류를 강제로 발생시킬 수도 있음.

```python
# 입력을 받습니다.
number = input("정수 입력> ")
number = int(number)
# 조건문 사용
if number > 0:    
# 양수일 때: 아직 미구현 상태입니다.
    raise NotImplementedError
else:    
# 음수일 때: 아직 미구현 상태입니다.
    raise NotImplementedError
```

---

### 리스트

파괴적

### 리스트에 요소 추가하기: append(), insert()

리스트에 요소를 추가할 때는 두 가지 방법이 있음. 한 가지는 `append()` 함수를 활용하는 것으로, 리스트 뒤에 요소를 추가함.

```
리스트명.append(요소)
```

다른 한 가지는 `insert()` 함수를 활용하는 것으로, 리스트의 중간에 요소를 추가함.

```
리스트명.insert(위치, 요소)
```

### 여러 요소를 한 번에 추가하기: extend()

`append()`와 `insert()` 함수는 리스트에 요소를 하나만 추가함. 한 번에 여러 요소를 추가하고 싶을 때는 `extend()` 함수를 사용함. `extend()` 함수는 매개변수로 리스트를 입력하는데, 원래 리스트 뒤에 새로운 리스트의 요소를 모두 추가해 줌.

```python
list_a = [1, 2, 3]
list_a.extend([4, 5, 6])
print(list_a)
```

### 리스트에 요소 제거하기

- 인덱스로 제거하기
- 값으로 제거하기

### 인덱스로 제거하기: del 키워드, pop()

인덱스로 제거한다는 것은 '리스트의 2번째 요소를 제거해 주세요'처럼 요소의 위치를 기반으로 요소를 제거하는 것. `del` 키워드 또는 `pop()` 함수를 사용함.

```
del 리스트명[인덱스]
```

`pop()` 함수 또한 제거할 위치에 있는 요소를 제거하는데, 매개변수를 입력하지 않으면 -1이 들어가는 것으로 취급해서 마지막 요소를 제거함.

```
리스트명.pop(인덱스)
```

**리스트 슬라이싱**

```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8]
print(numbers[0:5:2])
numbers = [1, 2, 3, 4, 5, 6, 7, 8]
print(numbers[::-1])   
# 시작/끝 인덱스는 자동으로 "전부"가 지정됩니다
```

```
[1, 3, 5]
[8, 7, 6, 5, 4, 3, 2, 1]
```

### 값으로 제거하기: remove()

```
리스트.remove(값)
```

remove() 함수는 지정한 값이 리스트 내부에 여러 개 있어도 가장 먼저 발견되는 하나만 제거. 만약 리스트에 중복된 여러 개의 값을 모두 제거하려면 반복문과 조합해서 사용해야 함.

### 모두 제거하기: clear()

```
리스트.clear()
```

### 리스트 정렬하기: sort()

리스트 요소를 정렬하고 싶다면 `sort()` 함수를 사용. 기본은 오름차순 정렬.

```
리스트.sort()
```

```python
list_e = [52, 273, 103, 32, 275, 1, 7]
list_e.sort()               
# 오름차순 정렬
print(list_e)
list_e.sort(reverse=True)   
# 내림차순 정렬 (키워드 매개변수 활용)
print(list_e)
```

### 리스트 내부에 있는지 확인하기: in / not in 연산자

```
값 in 리스트
```

---

### 딕셔너리와 반복문

### get() 함수

존재하지 않는 키에 접근했을 때 오류 없이 처리하는 두 번째 방법으로 **`get()` 함수** 가 있음. `get()` 함수는 딕셔너리의 키로 값을 추출하는 기능은 `딕셔너리[키]`와 같지만, **존재하지 않는 키에 접근해도 `KeyError`를 내지 않고 `None`을 반환** 한다는 차이가 있음.

> 💡 **비유**: `딕셔너리[키]`는 "이 서랍 무조건 열어!"라고 명령하는 것(없으면 당황해서 에러 발생)이고, `get()`은 "이 서랍 있으면 열고, 없으면 그냥 빈손으로 와도 돼"라고 부드럽게 물어보는 것과 같아요.
> 

```
# 딕셔너리를 선언합니다.
dictionary = {    "name": "7D 건조 망고",    "type": "당절임",    "ingredient": ["망고", "설탕", "메타중아황산나트륨", "치자황색소"],    "origin": "필리핀"}

# 존재하지 않는 키에 접근해 봅니다.
value = dictionary.get("존재하지 않는 키")

print("값:", value)
# None 확인 방법
if value == None:
     # None과 같은지 확인만 하면 됩니다.
    print("존재하지 않는 키에 접근했습니다.")
```
```
for key, value in movie.items():
    print("()()".format(key,value))
```
```
for i, v in enumerate(array):
    if v == t_number:
        print(i)
        break
    else:
        print(-1)
```
리스트 컴프리헨션
```
# 기존 for문
result = []
for x in range(5):
    result.append(x)  # ◀ 이 x가 앞의 x

# 리스트 컴프리헨션
result = [x for x in range(5)]
#         ▲        ▲
#      앞의 x    뒤의 x
```
```
# 앞의 x를 x * 2로 변경
double = [x * 2 for x in range(5)]
# 결과: [0, 2, 4, 6, 8]
```
```
# 앞의 x를 str(x)로 변경
strings = [str(x) for x in range(5)]
# 결과: ['0', '1', '2', '3', '4']
```

---

## 2. 파이썬과 프로그래밍2

### 함수

### 문자열, 리스트, 딕셔너리와 관련된 함수

**.replace()**
- 문법: ```문자열.replace(찾을값, 바꿀값)```

```
review = "이 서비스는 별로예요"
fixed = review.replace("별로예요", "최고예요")
print(fixed)  # 출력: 이 서비스는 최고예요
```

**f.-string(포맷팅)**
- 문법: ```f"{변수명}"```

```
user_name = "클라라"
question = "파이썬 딕셔너리 사용법"
# 실무 예시: 사용자 입력값을 넣어 AI에게 보낼 프롬프트를 자동 생성
prompt = f"{user_name}님이 '{question}'에 대해 질문했습니다. 초보자 눈높이로 답변해주세요."
print(prompt)
# 출력: 클라라님이 '파이썬 딕셔너리 사용법'에 대해 질문했습니다. 초보자 눈높이로 답변해주세요.
```

**sorted() / .sort()**

- 문법: ```sorted(리스트)``` (새 리스트 반환) / ```리스트.sort()``` (원본 변경)

```
numbers = [5, 2, 8, 1]
print(numbers)
numbers1 = sorted(numbers)
print(numbers1)
# 출력: [1, 2, 5, 8]  (원본 numbers는 그대로)

numbers.sort()
print(numbers)  # 출력: [1, 2, 5, 8]  (원본 자체가 바뀜)
```

**.index() / in**

- 문법: ```값 in 리스트``` (있는지 확인), ```리스트.index(값)``` (몇 번째인지 확인)

```
skills = ["Git", "Python", "OpenAI API"]
print("Python" in skills)      # 출력: True
print(skills.index("Python"))  # 출력: 1  (0번부터 세서 두 번째)
```

**.keys()**

- 문법: ```딕셔너리.keys()```

```
student = {"name": "클라라", "course": "AI서비스개발"}
print(student.keys())  

# 출력: dict_keys(['name', 'course'])
```

**.values()**

- 문법: ```딕셔너리.values()```

```
student = {"name": "클라라", "course": "AI서비스개발"}
print(student.values())  
# 출력: dict_values(['클라라', 'AI서비스개발'])
```

**.items()**

- 문법: ```for key, value in 딕셔너리.items():```

```
profile = {"이름": "클라라", "관심분야": "AI 서비스 기획"}
for key, value in profile.items():    
print(f"{key}:{value}")   

# 출력:# 이름: 클라라# 관심분야: AI 서비스 기획
```

**.update()**

- 문법: ```딕셔너리.update({key: value})```

```
user = {"name": "클라라", "level": "초급"}
user.update({"level": "중급", "course_week": 3})
print(user)  

# 출력: {'name': '클라라', 'level': '중급', 'course_week': 3}
```

| 목적         | 코드                                 |
| ---------- | ---------------------------------- |
| 값 하나 수정    | `dic["키"] = 값`                     |
| 값 하나 추가    | `dic["새 키"] = 값`                   |
| 여러 값 추가·수정 | `dic.update({"키1": 값1, "키2": 값2})` |

```
# 다른 딕셔너리의 내용으로 업데이트
original = {
    "name": "재연",
    "age": 28
}

new_data = {
    "age": 29,
    "city": "서울"
}

original.update(new_data)

print(original)
# {'name': '재연', 'age': 29, 'city': '서울'}
```
```
# 키워드 인자 방식
person.update(age=29, city="서울")
```

**중첩 딕셔너리(Nested Dictionary)**

- 문법: ```딕셔너리["key1"]["key2"]```

```
# 실무 예시: OpenAI API 응답 형태를 흉내낸 딕셔너리
api_response = {    "id": "chatcmpl-123",   
                    "choices": [ { "message": { "role": "assistant",
                    "content": "안녕하세요! 무엇을 도와드릴까요?"  } 
                     }  
                      ]
                } 
                    
# 서랍장(딕셔너리) 속 리스트, 그 리스트 속 딕셔너리를 차례로 열어서 답변만 꺼내기

answer = api_response["choices"][0]["message"]["content"]

print(answer)  # 출력: 안녕하세요! 무엇을 도와드릴까요?
```

---

```
def get_input(prompt, value):
    try:
        return input(prompt)
    except EOFError:
        print(f"입력처리 불가 --> {value} 기본값으로 진행합니다.")
        return value
```

---

### 모듈

**라이브러리란?**
- 여러 모듈과 패키지의 모음
- 특정 작업을 수행하기 위한 함수, 클래스, 상수 등의 모음
- 라이브러리는 여러 패키지들의 묶음으로 구성되며, 패키지는 여러 모듈의 묶음으로 구성
- 표준 라이브러리(Standard Library): 파이썬 설치 시 기본으로 제공됨
- 외부 라이브러리(Third-party Library): 다양한 회사, 개발자들이 제작하여 제공

**파이썬 표준 라이브러리**
- 랜덤 숫자 생성하는 라이브러리: random
- 복잡한 수학 관련 라이브러리: math
- 시간과 날짜 라이브러리: datetime
- 파일, 디렉토리 등 운영체제 라이브러리: os

---

### 표준 모듈

- math 모듈 — 수학 관련 기능

**math 모듈의 주요 함수**
|변수/함수|설명|
|---|---|
|```sin(x)```|사인값|
|```cos(x)```|코사인값|
|```tan(x)```|탄젠트값|
|```log(x[, base])```|로그값|
|```ceil(x)```|올림|
|```floor(x)```|내림|

*은행가 반올림(Banker's Rounding): 정수 부분이 짝수일 때 소수점이 5면 내리고, 홀수일 때 5면 올립니다.*
```
>>> round(1.5)2
>>> round(2.5)2
>>> round(3.5)4
>>> round(4.5)4
```

**from 구문 (모듈 이름 생략하기)**
```
from 모듈이름 import 가져오고싶은변수또는함수
```
**모두 가져오기**
```
from math import *
```
**as 구문(별칭 지정하기)**
```
import 모듈 as 사용하고싶은식별자
```

- random 모듈 — 랜덤 값 생성

```
import random
print("# random 모듈")

# random(): 0.0 <= x < 1.0 사이의 float를 리턴합니다.

print("- random():", random.random())

# uniform(min, max): 지정한 범위 사이의 float를 리턴합니다.

print("- uniform(10, 20):", random.uniform(10, 20))
# randrange(): 지정한 범위의 int를 리턴합니다.
# - randrange(max): 0부터 max 사이의 값을 리턴합니다.
# - randrange(min, max): min부터 max 사이의 값을 리턴합니다.
print("- randrange(10):", random.randrange(10))

# choice(list): 리스트 내부에 있는 요소를 랜덤하게 선택합니다.
print("- choice([1, 2, 3, 4, 5]):", random.choice([1, 2, 3, 4, 5]))

# shuffle(list): 리스트의 요소들을 랜덤하게 섞습니다.
print("- shuffle([1, 2, 3, 4, 5]):", random.shuffle([1, 2, 3, 4, 5]))

# sample(list, k=<숫자>): 리스트의 요소 중에 k개를 뽑습니다.
print("- sample([1, 2, 3, 4, 5], k=2):", random.sample([1, 2, 3, 4, 5], k=2))
```

*```shuffle()```은 리스트를 그 자리에서(in-place) 섞기 때문에 반환값은 ```None```입니다. 섞인 결과를 보려면 원본 리스트를 다시 출력해야 합니다.*

*```random.py```처럼 사용 중인 모듈과 같은 이름으로 파일을 저장하면 안 됩니다.*

*파이썬의 import 구문은 가장 먼저 현재 폴더에서 같은 이름의 파일을 찾습니다. ```random.py```로 저장하면 진짜 random 모듈이 아니라 자기 자신을 불러와 버려서 오류가 발생합니다.*

- sys 모듈 — 시스템 관련 정보

```
# 모듈을 읽어 들입니다.
import sys # 명령 매개변수를 출력합니다.
print(sys.argv)
print("---")

# 컴퓨터 환경과 관련된 정보를 출력합니다.
print("getwindowsversion():", sys.getwindowsversion())
print("---")
print("copyright:", sys.copyright)
print("---")
print("version:", sys.version)
# 프로그램을 강제로 종료합니다.
sys.exit()
```

```
# read_file.py

import sys

file_path = sys.argv[1]

with open(file_path, "r", encoding="utf-8") as file:
    content = file.read()

print(content)
```

- os 모듈 — 운영체제 관련 기능

```
# 모듈을 읽어 들입니다.

import os# 기본 정보를 몇 개 출력해 봅시다.
print("현재 운영체제:", os.name)
print("현재 폴더:", os.getcwd())
print("현재 폴더 내부의 요소:", os.listdir())

# 폴더를 만들고 제거합니다(폴더가 비어있을 때만 제거 가능).
os.mkdir("hello")
os.rmdir("hello")

# 파일을 생성하고 + 파일 이름을 변경합니다.
with open("original.txt", "w") as file:
    file.write("hello")
os.rename("original.txt", "new.txt")

# 파일을 제거합니다.
os.remove("new.txt")

# os.unlink("new.txt")   # remove()와 완전히 동일한 함수(이름만 다름)

# 시스템 명령어 실행
os.system("dir")
```

> ⚠️ **os.system() 함수의 위험성**
> 
> 
> `os.system()`은 명령어를 그대로 실행시켜 버립니다. 실제로 국내 한 대학교 알고리즘 대회에서, 참가 학생이 코드 내부에 `os.system("rm -rf /")` 같은 명령어를 실행시켜 리눅스 서버 전체(루트 권한이 있는 경우 컴퓨터의 모든 것)를 삭제해버린 사례가 있습니다. 운영 측에서 권한 관리를 소홀히 했던 보안 사고였습니다. **`os.system()`은 굉장히 위험할 수 있는 함수임을 꼭 기억하세요.** (물론 적절한 상황에서는 매우 유용합니다.)
>

- datetime 모듈 — 날짜와 시간 다루기

```
# 모듈을 읽어 들입니다.
import datetime
# 현재 시각을 구하고 출력하기
print("# 현재 시각 출력하기")
now = datetime.datetime.now()
print(now.year, "년")
print(now.month, "월")
print(now.day, "일")
print(now.hour, "시")
print(now.minute, "분")
print(now.second, "초")
print()# 시간 출력 방법

print("# 시간을 포맷에 맞춰 출력하기")
output_a = now.strftime("%Y.%m.%d %H:%M:%S")
output_b = "{}년{}월{}일{}시{}분{}초".format(    now.year,    now.month,    now.day,    now.hour,    now.minute,    now.second)

output_c = now.strftime("%Y{} %m{}%d{} %H{} %M{} %S{}").format(*"년월일시분초")
print(output_a)
print(output_b)
print(output_c)
```

>💡 ```strftime()```은 시간을 원하는 형식으로 출력할 수 있지만, 매개변수에 한글 같은 문자는 직접 넣을 수 없습니다. 그래서 ```output_b```, ```output_c```처럼 문자열/리스트 앞에 ```*```를 붙여 각 요소를 매개변수로 풀어 넣는 방식을 활용합니다.
>

```
# 모듈을 읽어 들입니다.
import datetimenow = datetime.datetime.now()

# 특정 시간 이후의 시간 구하기
print("# datetime.timedelta로 시간 더하기")
after = now + datetime.timedelta(    weeks=1,    days=1,    hours=1,    minutes=1,    seconds=1)
print(after.strftime("%Y{} %m{}%d{} %H{} %M{} %S{}").format(*"년월일시분초"))
print()# 특정 시간 요소 교체하기
print("# now.replace()로 1년 더하기")
output = now.replace(year=(now.year + 1))
print(output.strftime("%Y{} %m{}%d{} %H{} %M{} %S{}").format(*"년월일시분초"))
```

>💡 ```timedelta()```는 주/일/시/분/초 단위 계산은 되지만, "몇 년 후"를 직접 구하는 기능은 없습니다. 그래서 연도를 바꿀 땐 ```replace()```로 날짜 값 자체를 교체하는 게 일반적입니다.
>

- time 모듈 — 시간 정지 등

```
import time
print("지금부터 5초 동안 정지합니다!")
time.sleep(5)
print("프로그램을 종료합니다")
```

- urllib 모듈 — URL 다루기

URL = Uniform Resource Locator, 네트워크 자원의 위치

```
# 모듈을 읽어 들입니다.
from urllib import request

# urlopen() 함수로 구글의 메인 페이지를 읽습니다.
target = request.urlopen("https://google.com")
output = target.read()# 출력합니다.
print(output)
```

**실행 결과 (일부):**

```
b'<!doctype html><html itemscope="" itemtype="http://schema.org/WebPage" lang="ko">...생략...
```

> 💡 결과 앞에 `b`가 붙어있는데, 이는 **바이너리 데이터(binary data)** 를 의미합니다. `urlopen()`으로 페이지를 열고 `read()`로 내용을 읽어오는 흐름을 기억해두세요.
>

- operator

#### 📌 문제 상황: 콜백 함수 vs 람다

딕셔너리 리스트에서 최솟값/최댓값을 구할 때 콜백 함수나 람다를 쓰면 가독성 문제가 생길 수 있습니다.

```python
books = [{    "제목": "파이썬 프로그래밍",    "가격": 18000}, {    "제목": "머신러닝 + 딥러닝",    "가격": 26000}, {    "제목": "자바스크립트 프로그래밍",    "가격": 24000}]

def 가격추출함수(book):
    return book["가격"]
    min(books, key=가격추출함수)
    min(books, key=lambda book: book["가격"])
```

- **콜백 함수 방식**: 코드를 읽다가 `가격추출함수`가 뭔지 확인하려면 함수 정의부까지 찾아가야 함
- **람다 방식**: 람다 문법 자체를 모르는 개발자가 많아 코드 읽기가 어려울 수 있음

#### ✅ 해결책: operator.itemgetter()

```python
from operator 
import itemgetter   
# operator 모듈의 itemgetter() 함수를 가져옵니다.
books = [{    "제목": "파이썬 프로그래밍",    "가격": 18000}, {    "제목": "머신러닝 + 딥러닝",    "가격": 26000}, {    "제목": "자바스크립트 프로그래밍",    "가격": 24000}]

print(min(books, key=itemgetter("가격")))
print()print("# 가장 비싼 책")
print(max(books, key=itemgetter("가격")))
```

> 💡 `itemgetter("가격")`은 "'가격' 키를 꺼내는 함수"를 즉석에서 만들어줍니다. 함수 이름만 봐도 무슨 역할인지 짐작할 수 있어 **람다보다 코드 가독성이 좋습니다.**
>

---

### 튜플(tuple), 람다(lambda)

- 튜플: 함수와 함께 사용되는 리스트와 비슷한 자료형. 리스트와의 차이점은 한 번 결정된 요소는 바꿀 수 없다는 점이다.

```python
(a,b,c,d) = 1,2,3,4
tuple_test = 1,2,3,4
print(tuple_test)

a,b=10,20
a,b=b,a
```

*튜플은 함수의 리턴에 많이 사용된다.*

```python
for i, value in enumerate([1,2,3,4,5,6]):
    print(f"{}번째 요소는 {}입니다.".format(i,value))
```

- 람다: 매개변수로 함수를 전달하기 위해 함수 구문을 작성하는 것이 번거롭고, 코드 낭비라 생각이 들 때 함수를 간단하고 쉽게 선언하는 방법. `()->{}` 1회용 함수를 만들 때 사용한다.

```python
def call_10_times(func):    #콜백 함수
    for i in range(10):
        func()

def print_hello():
    print("hello!")

call_10_times(print_hello)

call_10_times(lambda : print("hello!"))
```

**함수를 매개변수로 사용하는 대표적인 표준 함수**
- `filter(함수, 반복할데이터)`: 원소마다 함수를 적용해 바꾼다.
- `map(조건함수, 반복할데이터)`: 조건이 참인 원소만 남긴다.

```python
def power(item):
    return item*item
def under_3(item):
    return item<3
    
list_input_a = [1,2,3,4,5]

output_a = map(power,list_input_a)
print(output_a)
print(list(output_a))
output_b = filter(under_3,list_input_a)
print(output_b) #제너레이터
print(list(output_b))

power = lambda x: x*x
under_3 = lambda x: x<3
output_a = map(power, list_input_a)
output_b = filter(under_3, list_input_a)
print(list(output_a))
print(list(output_b))

output_a = map(lambda x: x*x, list_input_a)
output_b = filter(lambda x: x<3, list_input_a)
```

---

파일처리
- 텍스트 파일
- 바이너리 파일

파일을 처리하려면
1. 파일을 열기 (open) &rightarrow; 파일 읽기, 파일 쓰기
    - open(파일의 경로, mode)
        - mode: w,a,r
    - close()
    - with 키워드: 파일의 열고 닫지 않는 실수를 방지하기 위한 태그

```python
file = open("basic.txt","w",encoding="utf-8")

file.write("파이썬 파일 처리 예제 작성중...")
file.close()
```

---

*복잡하고 구조화된 모듈을 만들 때는 **패키지(package)** 라는 기능을 사용한다.*

---

프로그래밍 언어에서는 프로그램의 진입점을 **엔트리 포인트(entry point)** 또는 메인(main)이라고 부릅니다. 그리고 이러한 엔트리 포인트 내부에서의 `__name__`은 `"__main__"`입니다.

#### 모듈의 `__name__`

엔트리 포인트가 아니지만 엔트리 포인트 파일 내에서 import 되었기 때문에 모듈 내 코드가 실행됩니다. 모듈 내부에서 `__name__`을 출력하면 모듈의 이름을 나타냅니다.

> `module_main` 디렉터리를 생성해 파일을 저장합니다.
> 

**소스 코드: `module_main/main.py`**

```python
# main.py 파일
import test_module

print("# 메인의 __name__ 출력하기")
print(__name__)
print()
```

**소스 코드: `module_main/test_module.py`**

```python
# test_module.py 파일
print("# 모듈의 __name__ 출력하기")
print(__name__)
print()
```

**실행 결과**

```
# 모듈의 __name__ 출력하기
test_module

# 메인의 __name__ 출력하기
__main__
```

코드를 실행하면 엔트리 포인트 파일에서는 `"__main__"`을 출력하지만, 모듈 파일에서는 모듈 이름을 출력하는 것을 볼 수 있습니다.

#### `__name__` 활용하기

엔트리 포인트 파일 내부에서는 `__name__`이 `"__main__"`이라는 값을 갖습니다. 이를 활용하면 현재 파일이 모듈로 실행되는지, 엔트리 포인트로 실행되는지 확인할 수 있습니다.

예를 들어 다음 코드를 살펴보겠습니다.

 `test_module.py`라는 이름으로 프로그램을 만들었습니다. 그리고 '이러한 형태로 활용한다'는 것을 보여주기 위해 간단한 출력도 넣었습니다.

> `module_example` 디렉터리를 생성해 파일을 저장합니다.
> 

**소스 코드: `module_example/test_module.py`**

```python
PI = 3.141592

def number_input():
    output = input("숫자 입력> ")
    return float(output)

def get_circumference(radius):
    return 2 * PI * radius

def get_circle_area(radius):
    return PI * radius * radius

# 활용 예 (이런 식으로 동작해요! 를 알려주는 용도)
print("get_circumference(10):", get_circumference(10))
print("get_circle_area(10): ", get_circle_area(10))
```

**소스 코드: `module_example/main.py`**

```python
import test_module as test  # 위 모듈을 읽어 들입니다.

radius = test.number_input()
print(test.get_circumference(radius))
print(test.get_circle_area(radius))
```

**실행 결과**

```
get_circumference(10): 62.83184     # 모듈에서 활용 예로 사용했던 코드까지 출력됨
get_circle_area(10):  314.1592
숫자 입력> 10 (Enter)
62.83184
314.1592
```

그런데 현재 `test_module.py`라는 파일에는 '이런 식으로 동작해요!'라는 설명을 위해 추가한 활용 예시 부분이 있습니다. 모듈로 사용하고 있는데, 내부에서 출력이 발생하니 문제가 됩니다.

이때 현재 파일이 엔트리 포인트인지 구분하는 코드를 활용합니다. 조건문으로 `__name__`이 `"__main__"`인지 확인만 하면 됩니다.

**소스 코드: `module_example/test_module.py` (엔트리 포인트를 확인하는 버전)**

```python
PI = 3.141592

def number_input():
    output = input("숫자 입력> ")
    return float(output)

def get_circumference(radius):
    return 2 * PI * radius

def get_circle_area(radius):
    return PI * radius * radius

# 활용 예: 현재 파일이 엔트리 포인트인지 확인하고,
# 엔트리 포인트일 때만 실행됩니다.
if __name__ == "__main__":
    print("get_circumference(10):", get_circumference(10))
    print("get_circle_area(10): ", get_circle_area(10))
```

**소스 코드: `module_example/main.py`**

```python
import test_module as test

radius = test.number_input()
print(test.get_circumference(radius))
print(test.get_circle_area(radius))
```

**실행 결과**

```
숫자 입력> 10 (Enter)
62.83184
314.1592
```

---

#### 패키지

