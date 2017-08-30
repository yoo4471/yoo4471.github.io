---
layout: post
comment: true
title: Storage Classes in C++
tags: [develop]
author: yoo4471
---

## Introduction
'A+B'를 구하는 문제의 해답에서 C++은 변수를 `int`로 선언하고, C++11 부터는 `auto`로 선언하여 사용하는 것을 알게 되었습니다. 좀 더 알아보기 위해 [Storage Classes in C++](https://www.tutorialspoint.com/cplusplus/cpp_storage_classes.htm)를 정리해 보았습니다.

## Storage Classes
- auto
- register
- static
- extern
- mutable

### The auto storage class
Auto storage class는 모든 지역 변수에 대해 기본적으로 적용됩니다.  

```Main
main() {
    auto intNumber = 1;
    auto floatNumber = 1.8;
    
    auto noInitial;          /* error */
    auto int intNumber = 10; /* error */
}  
```

- `auto`는 `initializer`의 값을 추론하여 알맞은 타입이 지정됩니다.
- `auto`를 사용할 때는 `initialize`를 같이 수행해야 합니다.
- `auto`와 타입 지정자인 `int`를 같이 사용할 수 없습니다.

### The register storage class
Register storage class는 RAM 대신 `register`에 저장되어야 하는 지역 변수를 정의하는 데 사용됩니다. `register` 변수는 `register`사이즈 (usually one word)를 최대로 가지며 RAM(메모리의 위치)가 없기 때문에 단항 연산자 `&`를 사용할 수 없습니다.  

```
main() {
    register int a;
    register int a = 10;
}
```

`register` 변수는 카운터와 같은 빠른 액세스가 필요한 변수에서만 사용해야 합니다. `register`로 정의한다고 해서 변수가 `register`에 저장된다는 의미는 아닙니다. 다시 말하면, 하드웨어 및 구현 제한에 따라 `register`에 **저장될 수도 있다**는 것을 의미합니다.  

### The static storage class
Static storage class는 지역 변수가 컴파일러에 의해 실행 범위(scope)에 들어오거나 나갈 때마다 생성(create)되거나 파괴(destroy)되는 것이 아니라 프로그램의 수명(life-time) 동안 유지되도록 합니다. 지역 변수를 `static`으로 만든다는 것은 함수 호출 간에 값이 유지된다는 것을 의미합니다.

`static` 수정자(modifier)는 전역 변수로도 사용할 수 있습니다. `static`으로 선언된 전역변수의 범위는 선언된 파일에 한정됩니다. C++에서 `static` 변수가 클래스의 데이터 멤버로 사용된다면, it causes only one copy of that member to be shared by all objects of its class.  

```
#include <iostream>

// Function declaration
void func(void);
 
static int count = 10; /* Global variable */
 
main() {
    while(count--) {
        func();
    }
    
    return 0;
}

// Function definition
void func( void ) {
    static int i = 5; // local static variable
    i++;
    std::cout << "i is " << i ;
    std::cout << " and count is " << count << std::endl;
}
```

위의 코드를 컴파일하고 실행하면 다음과 같은 결과가 생성됩니다.  

```
i is 6 and count is 9
i is 7 and count is 8
i is 8 and count is 7
i is 9 and count is 6
i is 10 and count is 5
i is 11 and count is 4
i is 12 and count is 3
i is 13 and count is 2
i is 14 and count is 1
i is 15 and count is 0
```

### The extern storage class
Extern storage class는 전역 변수를 프로그램의 모든 파일에서 참조할 수 있도록 만들 때 사용합니다. When you use 'extern' the variable cannot be initialized as all it does is point the variable name at a storage location that has been previously defined.  

`extern` 수정자(modifier)는 아래 설명된 것과 같이 전역 변수나 함수를 공유하는 두 개 이상의 파일이 있을 때 일반적으로 사용됩니다.  

#### First File: main.cpp  

```
#include <iostream>
 
int count ;
extern void write_extern();
 
main() {
    count = 5;
    write_extern();
}
```
#### Second File: support.cpp  
```  
#include <iostream>

extern int count;

void write_extern(void) {
    std::cout << "Count is " << count << std::endl;
}
``` 

`extern` 키워드는 다른 파일에서 카운트를 선언하는 데 사용됩니다.  

```
$g++ main.cpp support.cpp -o write  
```

실행하면 다음과 같은 결과가 생성됩니다.  

```
$./write
5
```

### The mutable storage class  
The mutable specifier applies only to class objects, which are discussed later in this tutorial. It allows a member of an object to override const member function. That is, a mutable member can be modified by a const member function.
