# 그래프 사용법

이번에는 Graph 를 사용하는 방법입니다.

아래와 같은 그래프 데이터 구조로 나타내는 방법입니다.

## 1. 배열을 이용하는 방법

  int map[5][5] 를 선언하고

   - 1 에서 출발하는 간선은  2, 3, 4 가 있으므로 map[1][2], map[1][3], map[1][4] 을 1로 체크

    - 2 에서 출발하는 간선은  3, 4 가 있으므로  map[2][3], map[2][4] 에 1로 체크

   하는 경우 아래와 같은 배열이 만들어 집니다.

```c
  0 1 2 3 4
0 0 0 0 0 0
1 0 0 1 1 1 
2 0 0 0 1 1
3 0 0 0 0 0
4 0 0 0 0 0
```

 코드로 표현하면 아래와 같습니다.

```c 
  int node, edge, from, to;
  
  cin >> node >> edge;
  
  for(int h = 0; h<=node; h++)  //map 배열 초기화
    for(int w = 0; w<=node; w++)
      map[h][w] = 0;
 
  for(int e = 0; e<edge; e++)
  {
    cin >> from >> to;
	map[from][to] = 1;   //간선 연결정보를 map에 표시(가중치가 있는 경우 1대신 가중치로)
  } 
``` 

각 노드에서 연결된 노드을 찾으려면 배열을 검색하면 되므로 간편하게 사용할 수 있습니다.

```c
  //1번에 연결된 노드 검색.
  cout << "Searching edge from 1 to  ";
 
  for(int m = 0; m<=node; m++)
  {
    if(map[1][m] != 0)  cout << m << " , ";
  }
```

* 간선의 가중치가 있으면, 1대신 가중치를 저장하면 됩니다.

* 방향성이 없는 graph 의 경우, map[from][to] = map[to][from] = 1 로 저장합니다.

* 크기가 큰 경우 메모리를 많이 사용하고, 초기화에 시간이 걸립니다.


## 2.  싱글 링크드 리스트를 이용하는 방법

연결된 노드 정보를 Single linked list 배열에 저장하는 방식입니다.

(parent 정보가 없는 Tree 라고 생각하시면 됩니다)

1번에 연결된 노드는 2, 3, 4 와   2번에 연결된 노드는 3,4  를 각각 Single linked list 로 표시합니다.

데이터를 모두 입력하고나면  single linked list의 배열은 아래와 같이 구성 됩니다.

0 :  null

1 :  3 -> 4 -> 2 -> null

2 :  4 -> 3 -> null

3 :  null

4 :  null
 

```c
  node* n;
 
  for(int e = 0; e < edge; e++)
  {
    cin >> from >> to;
 
    n = myalloc();
    n->v = to;    //연결된 노드를 저장
 
    n->prev = table[from];    //from 의 Single linked list 에 저장
    table[from] = n;
 
  }
``` 

각 노드에 연결된 노드를 검색할 때는  tabl[] 의 Single linked list 를 검색하시면 됩니다.

1번 노드에 연결된 노드를 찾는 경우, Table[1] 을 확인하면 됩니다.

```c
  //1번에 연결된 노드 검색.
  cout << "Searching edge from 1 to  ";
  for (node* iter = table[1]; iter != nullptr; iter = iter->prev)  // table[1] 의 Single linked list 에서 검색
  {
    cout << iter->v << " , ";
  }
``` 

* 초기화시에는 myalloc에서 사용하는 heap_cnt 와 Single linked list 가 저장되는 table[] 을 초기화 주면 됩니다.

* 방향성이 없는 경우 table[from] 에 to를 저장하고, table[to]에 from을 저장하면 됩니다.

* 가중치가 있는  Graph의 경우 node 구조체에 (value)를 추가하고, n->value 에 가중치를 저장하면 됩니다.

* 노드가 자연수가 아닌경우에는 이전 Tree 게시물을 참조해서 자연수로 변경하시면 됩니다.

* 같은 포멧의 코드를 계속 사용해서

* 실수할 가능성을 줄여 줍니다.

* 코딩 속도를 빠르게 해 줍니다.
