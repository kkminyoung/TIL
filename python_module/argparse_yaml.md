# arg_parse 와 yaml

- 파이썬에서 제공되는 표준 라이브러리로 command line을 구분해주는 모듈이다.
- 이 모듈을 사용하면 command line의 문자열들을 일일히 파싱해서 구분하지 않아도 되고, 다양한 함수들을 이용해 편하게 command line 옵션을 입력할 수 있다.

### 설치
pip install argparse


### 예제 코드

```
from __future__ import print_function
import argparse

def main():
	parser = argparse.ArgumentParser(description='This code is written for practice about argparse')
	parser.add_argument('X', type=float,
			metavar='First_number',
			help='What is the first number?')
	parser.add_argument('Y', type=float,
			metavar='Second_number',
			help='What is the second number?')
	parser.add_argument('--op', type=str, default='add',
			choices=['add','sub','mul','div'],
			help='What operation?')
	args = parser.parse_args()
	
	X = args.X
	Y = args.Y
	op = args.op
	print(calc(X,Y,op))

def calc(x, y, op):
	if op == 'add':
		return x + y
	elif op == 'sub':
		return x - y
	elif op == 'mul':
		return x * y
	elif op == 'div':
		return x / y

if __name__=="__main__":
	main()

```

### 1. 기초 코드
- argparse.ArgumentParser함수를 통해 parser를 생성한다.
- parser.add_argument를 이용하여 입력받고자 하는 인자의 조건을 설정한다
- parser.parse_args 함수를 통해 인자들을 파싱하여 args에 저장한다. 각 인자는 add_argument의 type에 지정된 형식으로 저장된다

### 2. parser.add_argument에 들어갈 수 있는 옵션
- type/ default/ choices/ help 등등
- 여기서 choices를 사용함으로써 help에서 어떤 옵션이 가능한지 보여준다.
- metavar는 필수 인자를 입력하지 않았을 때, 그 자리 변수가 무엇인지에 대한 이름이다.
- nargs=’+’를 사용함으로써, 여러개의 (제한 없음) 변수를 리스트로 받을 수 있다.

### 3. parser.parse_args()로 정의된 객체 args 사용방법
- X = args.X // Y = args.Y
- op = args.op 에서 처럼 –[]라고 add_argument를 했다면, args.[]라고 사용가능하다.



# yaml 파일

#### 기본 사용방법은 매우 간단하다.

1. 이와 같은 파일이 있다고 치자

```
# /home/info/yaml
language: python
test: pytes
```

2. 이렇게 py파일에서 쓰면 된다.

```
import yaml
   
with open('info.yaml') as f:
    cfg = yaml.load(f)
  
   
language = conf['language']
test = cfg['pytest']
```

즉, yaml 모듈의 load함수를 통해서 conf를 dictionary 객체로 만들어 준다.

3. 추가 사용법

모르겠으면 우선 yaml파일을 가져오고, type과 같은 함수를 이용해서 내가 가지고 있는 yaml.load로 선언한 객체에 대한 분석을 해보자.

### 코드 예제

```
# yaml 파일
yaml_str = """
Date: 2017-08-08
ChampionList:
- champion_id: 1000
 name: Teemo
 position: top
 skill: ap
- champion_id: 1001
 name: Vayne
 position: bottom
 skill: ad
- champion_id: 1002
 name: Ahri
 position: mid
 skill: ap
"""

# .py 파일 내부 코드
import yaml
     
cfg = yaml.load(yaml_str)
for champion in cfg['ChampionList']:
	print(champion["name"], champion["skill"])

```



# 변수 참고용
![image](https://user-images.githubusercontent.com/61506233/95857264-50653e80-0d96-11eb-96f9-182795d97176.png)

