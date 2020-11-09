# [SharedLib 예제]

#### 1) 이전에 만든 SeperatedCompileExample 폴더를 복사하여 SharedLibExample를 만들고 해당 디렉토리로 이동한다.
```
cp -r SeperatedCompileExample SharedLibExample
cd SharedLibExample
```

#### 2) 필요없는 파일들을 삭제한다.
```
rm add.o main.o
rm mytest
ls -al
```

![image](https://user-images.githubusercontent.com/61506233/98521430-c5089b80-22b6-11eb-8d3d-8cf6e9e6e409.png)



#### 3) vi editor로 .bashrc(Home directory)의 제일 마지막에 경로를 추가한다.
```
vi .bashrc
이후 마지막으로 이동
```

```
// 두줄 추가  (Shared Libaray는 경로를 정확히 지정을 해주어야함)

LD_LIBRARY_PATH=/home/[본인ID]/SharedLibExample:$LD_LIBRARY_PATH // LD_LIBRARY_PATH : 경로 로드
export LD_LIBRARY_PATH // 시스템에 등록
:wq
```

#### 4) 명령어 프람프트에서 아래 Command를 실행한다.
```
source .bashrc 
```

#### 5) 명령어 프람프트에서 아래 Command를 실행한다. (컴파일)
```
gcc –fPIC –c add.c // fPIC : 라이브러리의 위치를 자유롭게 -c : 컴파일만 하겠다.
gcc –shared –o libadd.so add.o // shared object만들기 (add.o로 libadd.so 만들기!)
gcc –o mytest main.c libadd.so 
// 차이 
// Seperated : mytest에 c가 들어가있음
// Shared : 정보만 남아있게 됨 실행할때 링크,  mytest에 c가 들어가있지 않음
ls -al
```

![image](https://user-images.githubusercontent.com/61506233/98522425-02b9f400-22b8-11eb-8f37-153420e82005.png)


#### 7) app.out을 아래와 같이 실행해본다.
```
./mytest 1 2
```

-> 마찬가지로 Result : 1 + 2 - 3 이 출력됨
