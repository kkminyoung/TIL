# [SeparateCompile 예제]

#### 1) Home directory에 SeparateCompileExample 디렉토리를 생성하고 해당 디렉토리로 이동한다.
```
mkdir SpeparateCompileExample
cd SeparateCompileExample
```


#### 2) vi editor로 main.c 를 작성한다.
```
vi main.c
```

```
#include <stdio.h>
#include "add.h"

void main(int argc, char* argv[])
{
        int result = 0;

        if (argc < 2) {
                printf("Usage : ./mytest <1st num> <2nd num>\n");
                return;
        }

        result = add(atoi(argv[1]), atoi(argv[2]));

        printf("Reuslt : %s + %s = %d\n", argv[1], argv[2], result);

        return;
}
```


#### 3) vi editor로 add.h 를 작성한다.
```
vi add.h
```
```
#include <stdio.h>
#include <stdlib.h>

extern int add(int , int);
```

#### 4) vi editor로 add.c 를 작성한다.
```
vi add.c
```

```
#include "add.h"

int add(int a, int b)
{
        int result;

        result = a + b;

        return result;
}
```
#### 5) main.c add.c를 Compile 한다.
```
gcc -c main.c add.c
ls -al
```

![image](https://user-images.githubusercontent.com/61506233/98520328-3b0c0300-22b5-11eb-8d76-d42edda3bd85.png)

#### 6) 명령어 프람프트에서 아래 Command를 실행한다.
```
gcc –o mytest main.o add.o // mytest 실행파일을 만든다. (main.o add.o를 링크)
ls -al
```

![image](https://user-images.githubusercontent.com/61506233/98520688-bff71c80-22b5-11eb-8dd0-7debdd262cc4.png)



#### 7) mytest를 파라미터를 주어 실행해본다.
```
./mytest 1 2
```

-> Result : 1 + 2 -3
