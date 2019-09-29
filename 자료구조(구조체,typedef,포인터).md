# 자료구조 스터디

유튜브 박동규 교수님의 널널한 박교수의 C프로그래밍 고급편으로 스터디 진행함

![널널한 박교수의 C언어 고급편](https://youtu.be/yV1gplHtXiM)

(인프런에도 강의가 있다 ㅎㅎ)



## 구조체



구조체를 공부하기 위해 C에서 쓰는 자료형들을 먼저 한번 구조를 살펴본다.

### C의 자료형

![C의자료형](https://image.slidesharecdn.com/1-160719064554/95/1-6-638.jpg?cb=1517373510)

### 구조체란?

구조체의 정의 : 기본 자료형만으로 표현하지 못하는 복잡한 형

- 예를 들면, 사람의 이름, 나이 키, 몸무게 정보를 하나의 자료형으로 묶어서 표현하고 싶은 경우?

**struct**라는 키워드를 이용하여 선언한다.

![구조체의 메모리구조](https://image.slidesharecdn.com/1-160719064554/95/1-8-638.jpg?cb=1517373510)

```c
#include <stdio.h>

// 구조체 정의
// Person은 struct 태그(식별하기 위한 이름)
struct Person {         // Person 구조체 선언
    char name[100];     // 구조체 멤버-이름
    int age;            // 구조체 멤버-나이
    int height;         // 구조체 멤버-키
    float weight;       // 구조체 멤버-몸무게
};

struct Person personA, personB; // 선언
```

이런식으로 struct 안에 필요한 것들을 정의해준다.

### 구조체 초기화

변수 초기화와 마찬가지로, 구조체도 초기화가 가능하다.

- {..., ..., ...} 형식으로 멤버값을 초기화 시키고
- 정의된 순서대로 초기값을 입력하여 초기화시킨다.

```c
struct Person personC = {"Park", 40, 174, 66};
printf("personC의 이름 %s, 나이 = %d\n", personC.name, personC.age);
```

### 구조체의 활용

1. 구조체를 멤버로 가지는 구조체를 만들 수 있다.
2. 구조체의 대입연산과 비교연상
- 대입연산은 가능하다.
- 비교연산, 사칙연산은 불가하다.
3. 구조체의 배열 선언또한 가능.


예제코드


```c
#include <stdio.h>

struct point {
    int x;
    int y;
};

struct rect {
    struct point p1;
    struct point p2;
};

int main(void)
{
    struct rect r;
    int w, h, area, peri;
    
    printf("왼쪽 상단의 좌표를 입력하시오: ");
    scanf("%d %d", &r.p1.x, &r.p1.y);
    
    printf("오른쪽 하단의 좌표를 입력하시오: ");
    scanf("%d %d", &r.p2.x, &r.p2.y);
    
    w = r.p2.x - r.p1.x;
    h = r.p2.y - r.p1.y;
    
    area = w * h;
    peri = 2 * w + 2 * h;
    printf("사각형의 면적은 %d이고 둘레는 %d입니다.\n", area, peri);
    
    struct rect new_r;
    new_r = r;   // OK
//    struct rect rx = r * new_r;
//    if( r < new_r )   // 비교연산은 지원하지 않는다
//    {
//        printf("new_r이 큽니다 \n");
//    }
//
    struct rect array_r[10];   // 구조체 배열은 가능함
    
    return 0;
}
```



## typedef 자료형


typedef 자료형은 C에서 자료형을 **새롭게 정의** 하는데에 굉장히 유용하다.

### typedef의 이용

![typedef의 이용](https://image.slidesharecdn.com/2typedef-160719064714/95/2-typedef-2-638.jpg?cb=1517373603)


```c
typedef unsigned char BYTE;
BYTE idx;	// unsigned int index; 랑 똑같음

typedef int INT32;
typedef unsigned int UNIT32;

INT32 i;	// int i;와 같다.
UNIT32 k;	// unsigned int k;와 같다.
```

위에서 배운 구조체랑 섞으면

```c
typedef struct Person { // Person 구조체 선언
    char name[100]; // 구조체 멤버-이름
    int age;        // 구조체 멤버-나이
    int height;     // 구조체 멤버-키
    float weight;   // 구조체 멤버-몸무게
} Person ;

struct Point2D {   // Point2D 구조체 정의
    int x;
    int y;
};
typedef struct Point2D Point2D;
```


![typedef struct](https://image.slidesharecdn.com/2typedef-160719064714/95/2-typedef-6-638.jpg?cb=1517373603)

같은 내용인데, 위와 같이 간단하게 쓸 수도 있다.

주로 어떨때 쓰면 좋냐면,


```c
#include <stdio.h>

typedef struct point {
    int x;
    int y;
} POINT;

POINT translate(POINT p, POINT delta);

int main(void)
{
    POINT p = { 2, 3 };
    POINT delta = { 10, 10 };
    POINT result;
    
    result = translate(p, delta);
    printf("새로운 점의 좌표는(%d, %d)입니다.\n", result.x, result.y);
    
    return 0;
}

POINT translate(POINT p, POINT delta)
{
    POINT new_p;
    
    new_p.x = p.x + delta.x;
    new_p.y = p.y + delta.y;
    
    return new_p;
}
```

여기서 POINT translate 함수는, typedef를 통해 POINT형을 정의하여 사용한다.

이 함수를 좀 더 설명하자면, 매개변수를 이용해서 새로운 POINT 값을 만들어서 반환한다.

결과적으로는 delta 만큼 이동됨.

### typedef 자료형의 필요성

1. 새로운 자료형을 정의하여 C의 자료형을 확장시키는 역할을 한다.
2. C코드의 이식성을 높여준다.
	- 코드를 하드웨어에 독립적으로 만드는데 도움이 된다.



## 포인터



### 포인터 변수

1. C에서 다른 객체를 **참조(refer)** 하기 위해 사용한다.
2. 구체적으로 컴퓨터 메모리에 있는 자료값이나 변수 혹은 함수의 주소를 가지는 변수


> 왜 포인터를 쓰는지?

- 일반변수를 통해서도 메모리에 값을 저장하고 불러올 수 있다.
- 그러나 포인터를 사용하면 일반적인 방식에서 허용하지 않는 접근이 가능하므로, 매우 편리한 도구이다.
- 도서관에 수십만권의 책이 있고, 이 중에서 내가 원하는 책을 찾고자 한다.
	- 우리가 찾고자 하는 책의 위치를 저장하고 있는 인덱스를 먼저 접근해서 찾은 다음, 실제 책이 있는곳에 가서 책을 찾으면 효과적이다.

### 변수형의 크기

- 변수의 크기에 따라서 메모리 공간이 달라진다.
- char -> 1byte / short -> 2byte / int -> 4byte, etc....
- sizeof 연산자로 컴파일러가 컴파일할 때 변수형 크기를 알 수 있다.


```c
#include <stdio.h>

int main(void)
{
    int i;
    char ch;
    short shrt;
    float flt;
    double dbl;
    
    printf("int형의 크기 = %lu \n", sizeof(int));
    printf("변수 i의 크기 = %lu \n", sizeof(i));
    
    printf("변수 ch의 크기 = %lu \n", sizeof(ch));
    printf("변수 shrt의 크기 = %lu \n", sizeof(shrt));
    printf("변수 flt의 크기 = %lu \n", sizeof(flt));
    printf("변수 dbl의 크기 = %lu \n", sizeof(dbl));
}
```

sizeof 는 byte 단위이다.

### 주소연산자 &

- 변수 i의 주소를 반환하는 연산자 : &i
- 메모리상의 주소값은 실행할 때 마다 바뀔 수 있다.

![주소연산자](https://image.slidesharecdn.com/3-160719065022/95/3-15-638.jpg?cb=1517373792)

![주소연산자 예제](https://image.slidesharecdn.com/3-160719065022/95/3-18-638.jpg?cb=1517373792)

### 포인터의 선언

```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
    int *myPointer;
    int myVar = 10;
    
    printf("myVar = %d\n", myVar);
    myPointer = &myVar;
    // myPointer 라는 변수는 myVar라는 변수의 주소값을 가진다.
    
    return 0;
}
```

myVar과 myPointer를 그림으로 나타내면 다음과 같다.

![포인터와 메모리구조 예제](https://image.slidesharecdn.com/3-160719065022/95/3-39-638.jpg?cb=1517373792)

### 간접 참조 연산자 *

포인터 변수가 가리키는 값을 가져오는 연산자다.

![간접참조 연산자](https://image.slidesharecdn.com/3-160719065022/95/3-49-638.jpg?cb=1517373792)


아까 main 함수에 다음과 같은 코드를 추가한다.

```c
*myPointer = 20;
    printf("\n*myPointer = 20으로 변경 \n");
    printf("myVar = %d\n", myVar);
```

![결과](https://image.slidesharecdn.com/3-160719065022/95/3-52-638.jpg?cb=1517373792)

> 포인트는, **간접적인 방법으로 myVar의 값을 변경시키는 것이다.**

### NULL 값

- NULL 은 C컴파일러에서 0으로 추정하는 값이다.
- NULL 은 포인터 변수를 초기화할 때 주로 사용하며, 포인터 변수가 아무것도 가리키지 않는다는 것을 나타낸다.


```c
#include <stdio.h>

int main(void) {
    
    printf("NULL value = %p\n", NULL);
    return 0;
}
```

NULL value 는 0x0으로 나온다.