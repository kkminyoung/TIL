### Ranged-based for loops
```
for (element_declaration : array) 
    statement;
```
루프는 각 array의 요소를 반복하여 element_declaration에 선언된 변수에 현재 배열 요소의 값을 할당한다. 최상의 결과를 얻으려면 element_declaration이 배열 요소와 같은 자료형이어야 한다. 그렇지 않으면 형 변환이 발생한다.

ex)
```
int fibonacci[] = { 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89 }; 
for (int number : fibonacci) 
   cout << number << ' ';
return 0;
```
### Ranged-based for loops and the auto keyword

element_declaration은 배열 요소와 같은 자료형을 가져야 하므로, auto 키워드를 사용해서 C++이 자료형을 추론하도록 하는 것이 이상적이다.
```
for (auto number : fibonacci)
    std::cout << number << ' ';
```
### Ranged-based for loops and references
위에서 본 예제들에서 element_declaration은 값으로 선언된다.
```
int array[5] = { 9, 7, 5, 3, 1 };
for (auto element: array) 
       std::cout << element << ' ';
```
반복된 각 배열 요소가 element에 복사된다. 배열 요소를 복사하는 것은 비용이 많이들 수 있다. 다행히도 다음과 같이 참조를 사용할 수 있다.

```
int array[5] = { 9, 7, 5, 3, 1 }; 
for (auto &element: array)
   cout << element << ' ';
```
읽기 전용으로 사용하려는 경우 element를 const로 만드는 것이 좋다.

```
int array[5] = { 9, 7, 5, 3, 1 };
    for (const auto& element: array) 
        std::cout << element << ' ';
```
성능상의 이유로 ranged-based for 루프에서 참조 또는 const 참조를 사용하는 게 좋다.

### Rewriting the max scores example using a ranged-based for loop
다음은 범위 기반 for 루프(ranged-based for loop)를 사용해서 다시 작성한 처음 예제다.

```
#include <iostream>

int main()
{
    const int numStudents = 5;
    int scores[numStudents] = { 84, 92, 76, 81, 56 };
    int maxScore = 0; // keep track of our largest score
    for (const auto& score: scores) // iterate over array scores, assigning each value in turn to variable score
        if (score > maxScore)
            maxScore = score;

    std::cout << "The best score was " << maxScore << '\n';

    return 0;
}
```
 
위 예에서, 더는 수동으로 배열을 첨자화할 필요가 없다. 변수 score을 통해 배열 요소에 직접 접근할 수 있다.

#### Ranged-based for loops and non-arrays
범위 기반 for 루프(ranged-based for loop)는 고정 배열뿐만 아니라 std::vector, std::list, std::set, std::map과 같은 구조에서도 작동한다. 방금 언급한 것들을 아직 배우지 않았으므로 무엇인지 몰라도 걱정하지 않아도 된다. ranged-based for 문이 유연하다는 것만 기억하면 된다.
```
#include <vector>
#include <iostream>

int main()
{
    std::vector<int> fibonacci = { 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89 }; // note use of std::vector here rather than a fixed array
    for (const auto& number : fibonacci)
        std::cout << number << ' ';

    return 0;
}
```
#### Ranged-based for loops doesn’t work with pointers to an array
포인터로 변환된 배열에서 범위 기반 for 루프를 사용할 수 없다. (배열의 크기를 알지 못하기 때문이다.)
```
#include <iostream>

int sumArray(int array[]) // array is a pointer
{
    int sum = 0;
    for (const auto& number : array) // compile error, the size of array isn't known
        sum += number;

    return sum;   
}

int main()
{
     int array[5] = { 9, 7, 5, 3, 1 };
     std::cout << sumArray(array); // array decays into a pointer here
     return 0;
}
```

