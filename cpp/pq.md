## 우선순위 큐

#### 선언
```
#include <queue>
using namespace std;
priority_queue<int> pq;
```

#### 기능
```
#include <iostream>
#include <queue>
using namespace std;
priority_queue<int> pq;
 
int main() {
    pq.push(1);
    cout << pq.top() << endl;
    pq.push(2);
    pq.push(5);
    pq.pop();
 
    return 0;
}
```

- push
- pop
- empty
- size
- top


#### 출력
```
void print_pq (priority_queue<int> & pq){
    priority_queue<int> new_pq = pq;
    while(!new_pq.empty()){
        printf("%d ", new_pq.top());
        new_pq.pop();
    }
    printf('\n');
}
```

#### 정렬 

Max heap(내림차순)
```
priority_queue<int> pq;
```

Min heap(오름차순)
```
class Compare{
public:
    bool operator() (int lhs, int rhs){
        return lhs > rhs;
    }
};
 
priority_queue<int, vector<int>, Compare> pq;
```
오름차순 같은 방식
```
priority_queue<int, vector<int>, greater<int> > pq;
```
