---
title: "python if else문 정리"
excerpt_separator: "<!--more-->"
categories:
  - Category
tags:
#   - Post Formats
#   - readability
  - standard
---

# Python





# 특징

 1. 무료다 인터프리터 언어다

 2. 읽기 쉽고 사용하기 쉽다.

 3. 사물인터넷과 잘 연동된다.

 4. 다양하고 강력한 외부 라이브러리들이 풍부하다

 5. 강력한 웹 프레임워크를 사용할 수 있다

    

설치 3.9.x 버전 설치 (3.10.x버전은 크롤링 안되는게 있)

-다른버전 사용 환경변수 설정 PATH에서 원하는버전을 위로



폴더 C:\apps\python_lecture 폴더 생성 vsc 실행

cmd창에서 

​	1.cd \ 	

​	2.cd apps	 

​	3.mkdir python_lecture && cd python_lecture && code .



vsc에서 Extensions에서 python, jupyter, Material icon 설치



python

변수

스네이크케이스기법 , 카멜케이스기법



주석처리 ctrl+/ 

vsc 터미널 ctrl+`



```
sel = int(input("진수 타입을 넣으시오(16/10/8/2)"))
num = input('값 입력:')
if sel == 16:
    num10 = int(num, 16)
    print(hex(num10))
    print(bin(num10))
if sel == 2:
    num2 = int(num, 2)
    print(bin(num2))
    print(hex(num2))
```

인터프리터는 한줄만 인식 {}아니라 들여쓰기에 따라 판단

줄이 길어질 떄 다음줄과 같이 쓰려면 \ 써주면 된다.



```
print("나는"사람이다.)          (X)
print(''' '나는' 사람이다.''')  (O)
```

