# 트리 사용법

0, 1, 2, 3 ... 을 노드의 unique ID 로 가지는 트리에 대한 내용입니다.

예를 들어 아래와 같은 트리를 구성해 보려고 합니다.

ID 가 자연수로 이루어져 있는 특징이 있어, ID를 배열의 인덱스로 이용하여 트리정보를 구성하려고 합니다.

위와 같은 트리의 정보는 아래와 같이 부모와 자식의 정보만 알면 됩니다.

NODE 1 :  parent 는 없으며, Child 는 2, 3, 4

NODE 2 :  parent 는  1,   Child 는  5, 6

NODE 3 :  parent 는  1,   Child 는  없음

위와 같은 데이터를 구성하기 위해 Child 정보를 linked list 로 만드는 것입니다.

```c 
int parent_info[MAX_NODE];     //부모 정보를 저장할 배열입니다.
NODE *child_info[MAX_NODE];  //child 정보를 저장할 linked list 입니다.
```

parent_info는 새로 추가되었으며,

child_info는 예전의 hash table 과 같은 구조입니다. 이름만 바꼈네요.

ID 외에 Node에 저장할 정보가 있는 경우, 아래와 같이 전역으로 만드시면 됩니다.

구조체를 선호하시면 parent_info, child_info 와 모두 묶어서 Struct 으로 만드셔도 됩니다.

```c
char name[MAX_NODE][32];
int age[MAX_NODE];
int NumberOfChild[MAX_NODE];
```

이제 데이터를 입력 받는 방식입니다.

보통 트리 정보를 입력 받을때, parent child 연결 정보가 아래와 같이 주어지는데,

```
1 2
1 3
1 4
2 5
2 6
6 7
4 8
4 9
4 10
```

이를 저장하는 방법은 아래와 같습니다.

부모 정보 배열을 저장하는 라인이 추가 되었으며,

Child 정보를 부모의 Linked list 에 저장합니다. 저장하는 방법은 예전과 동일합니다.

```c
cin >> parent >> child;  //부모 - 자식간의 연결정보
 
parent_info[child] = parent;  //부모 정보를 child에 저장
 
p = myalloc();  //저장할 공간을 alloc 합니다.
p->id = child;
p->prev = child_info[parent];  //parent 의 linked list 에 child 정보를 저장합니다.
child_info[parent] = p;
``` 

위와 같이 데이터를 다 저장하고 나면, 전체 트리 정보는 아래와 같이 표시됩니다.

```
Tree info
( 0 )'s parent = -1 child = 
( 1 )'s parent = -1 child =  4 3 2
( 2 )'s parent = 1 child =  6 5
( 3 )'s parent = 1 child = 
( 4 )'s parent = 1 child =  10 9 8
( 5 )'s parent = 2 child = 
( 6 )'s parent = 2 child =  7
( 7 )'s parent = 6 child = 
( 8 )'s parent = 4 child = 
( 9 )'s parent = 4 child = 
( 10 )'s parent = 4 child = 
```

이렇게 만들어 놓은 트리 데이터에서 

최상위 노드를 찾는 방법은 배열에서 부모가 있는 어떤 노드던지 찾아서, 부모를 따라 올라가면 됩니다.

```c 
int node;
 for (node = MAX_NODE-1; node >= 0; node--)  //부모가 있는 노드 아무거나 검색 (1이 루트라, 일부러 뒤에서 검색)
  if (parent_info[node] != -1) break;
 
 cout << "Found any node " << node << endl;
 
 do {
  cout << "check " << node << " parent " << parent_info[node] << endl;
  node = parent_info[node];
 } while (parent_info[node] != -1);
 cout << "FOUND ROOT NODE : " << node << endl << endl;
``` 

Node를 traversing 하려면 재귀 호출을 이용해서 간단하게 할 수 있습니다.

```c
//1 이하의 모든 node traversing
traversing_tree(1);
 
void traversing_tree(int node)
{
 cout << node << " ";
 for (NODE * pp = child_info[node]; pp != nullptr; pp = pp->prev)
 {
  traversing_tree(pp->id);
 }
}
``` 

재귀 호출을 이용하는 경우 코드는 간단하지만, 깊이 우선 탐색이 되므로, 넓이 우선 탐색을 하려면 큐에 저장하는 방법을 사용해야 합니다.

마지막으로 테스트 케이스마다 초기화가 필요한 경우

heap array 와 tree 정보를 초기화 해주시면 됩니다.

```c
 arr_idx = 0;  //사용할 힙 배열 초기화
 for (int i = 0; i < MAX_NODE; i++)  //tree 정보 초기화
 {
  child_info[i] = nullptr;
  parent_info[i] = -1;
 }
``` 

* 코드가 별로 달라진게 없습니다. 변수 이름만 바꼈을 뿐....

* 배열로 노드에 직접 접근 하므로 접근하는 속도가 빠릅니다.

* 사람에 따라 다르겠지만 LCRS 보다 휠씬 쉽습니다.

* NODE 의 ID 가 문자열등 자연수가 아니거나, 너무 큰 수인 경우 약간의 변형으로 이용이 가능합니다.
