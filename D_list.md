# 더블 링크드 리스트 사용법

- NODE* 인 pList 는 초기화시에 NULL

	pList = NULL;
 
 
- NODE 1을 추가시에 pList가 가르키는 것을 Node 1의 prev가 가르킴

- pList 는 추가되는 노드를 가르킴

	node1->prev = pList;   //빨간선
	pList = node1  //초록선
 
- NODE 2가 추가 되는 경우 동일하게 반복됩니다.

- pList 가 가르키던 건 Node 2의 prev가 가르킴

- pList 는 추가된 Node 2를 가르킴
 
	node2->prev = pList;  //초록선
	pList = node2;  //주황선
 
추가되는 노드는 항상 Single linked list 에서 뒤쪽으로 추가됩니다.

Node 내의 prev 라고 이름 지어진 이유이기도 하구요.


==============================================


초기에는 Head 와 Tail 두개의 Node를 미리 가지고 있습니다.


- 초기화로 head의 next 는 tail을    tail 의 prev 는 head 를 가르킵니다.

```c 
	NODE *head = myalloc();
	NODE *tail = myalloc();
	head->next = tail;
	tail->prev = head;
``` 
tail->prev 가 싱글링크드리스트의 pList 역할을 하고 있으므로

노드가 추가되면  tail의 prev에 추가하시면 됩니다.

```c
	NODE *node1 = myalloc();
	node1->v = 1;
 
	node1->prev = tail->prev;  //tail->prev 가 가르키던것은 내가 가르키고
	tail->prev = node1;   //tail->prev 는 나를 가르키게

//여기까지가 싱글링크드리스트와 동일하게 빨간선 연결을 해준것입니다.
//이후 초록색인 next 연결을 처리하면 됩니다.
 
	node1->next = node1->prev->next;   // 나의 next는  prev노드가 가르키던 next를
	node1->prev->next = node1;    //prev 노드의 next는 나를 가르키게
``` 

- 주의하실 점은 추가되는 node1 의 prev, next 를 먼저 지정해야 합니다. (너무 당연한 말이지만요)

두번째 노드를 추가하면 동일과정을 반복하면 됩니다.

```c
NODE * node2 = myalloc();
node2->v = 2;
  
node2->prev = tail->prev;
tail->prev = node2;
  
node2->next = node2->prev->next;
node2->prev->next = node2;
``` 

이제 검색하실 때는 head 에서 tail 까지  혹은 tail 에서 head 까지 검색하실 수 있습니다.

먼저 Head 에서 Tail 로 검색하는 경우

출발점은 head->next, 끝나는 점은 tail 까지 가시면 됩니다.

```c 
for(NODE* iter = head->next; iter != tail; iter = iter->next)
{
    if(iter->v == 1) cout << "Found.. " << iter->i;
}
``` 

다음으로 Tail 에서 Head로 검색하는 경우

출발점은 tail->prev, 끝나는 점은 head 가 되겠네요.

```c 
for(NODE *iter = tail->prev; iter != head; iter = iter->prev)
{
    if(iter->v == 1) cout << "Found.. " << iter->i;
}
``` 


이제 삭제하는 방법입니다.

2번 노드를 검색해서 삭제하는 경우, 아래와 같이 노드의 연결만 변경하면 됩니다.

(기존의 Node2 가 가진 연결은 안 지워도 됩니다. (그림은 보기 쉽게 지웠습니다.)

```c 
for(NODE *iter = head->next; iter != tail; iter = iter->next)
{
    if(iter->v == 2)
    {
        iter->prev->next = iter->next;    // prev 노드의 next를  내가 가르키던 next 로  (초록색연결)
        iter->next->prev = iter->prev;    // next 노드의 prev를  내가 가르키던 prev 로  (빨간색연결)
    }
}
```

==============

Dummy tail 을 추가한 싱글 링크드 리스트의 삭제편입니다.

```c++
#include <iostream>
 
using namespace std;
 
struct NODE {
 int v;
 NODE * prev;  //싱글 리스트를 위해 추가.
} a[20000];
 
int arr_idx = 0;
 
NODE *myalloc(void)
{
  return &a[arr_idx++];
}
 
void main(void){
 
  arr_idx = 0; //사용할 배열 초기화
 
  NODE *tail = myalloc();  //Dummy tail 노드를 미리 만들어 둡니다.
  tail->prev = nullptr;  //tail->prev 가 pList 역할을 대신 합니다.
 
  NODE *p;
  for (int i = 0; i<8; i++)
  {
    p = myalloc();
    p->v = i;
    p->prev = tail->prev;  // tail->prev 에 추가합니다.
    tail->prev = p;
  }
 
  //추가된 모든 노드 확인.
  for (NODE *iter = tail->prev; iter != nullptr; iter = iter->prev)
  {
    cout << iter->v << " ";
  }
 
  cout << endl;
 
  // 5를 찾아서 삭제
  NODE * next = tail; //노드 저장은 tail 부터
 
  for (NODE *iter = tail->prev; iter != nullptr; iter = iter->prev)
  {
    if (iter->v == 5)  //5를 찾으면
    {
       cout << "Deleted " << iter->v << endl;
       
       next->prev = iter->prev;  //저장해둔 노드의 prev를 변경하여 삭제
    }
 
    next = iter;  //노드를 저장합니다.
  }
  cout << endl;
 
  //삭제후 모든 노드 확인.
 
  for (NODE *iter = tail->prev; iter != nullptr; iter = iter->prev)
  {
    cout << iter->v << " ";
  }
  cout << endl;
}
```
