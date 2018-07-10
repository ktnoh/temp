# 해쉬 사용법

  ```c
unsigned long myhash(const char *str)
{
  unsigned long hash = 5381;
  int c;
  while (c = *str++)
  {
    hash = (((hash << 5) + hash) + c) % MAX_TABLE;
  }
  return hash % MAX_TABLE;
}
``` 

사용할 hash table 은 지난번의 single linked list 를 배열로 만든 것입니다.

당연히 다 아시는 개념이겠지만, single linked list 가 여러개 있다고 생각하시면 됩니다.

```c
struct NODE {
  char data[7];
  NODE * prev;  //싱글 리스트를 위해 추가.
} a[20000];    //지난번과 동일하게 데이터 저장을 위한 힙입니다.

NODE *hTable[MAX_TABLE];  //Hash Table 입니다.
```

저장할 데이터를 참고로 적당한 key를 뽑아서 hTable[ key ] 인 single linked list에 저장하는 방식입니다.

```c
int key;
char in[7] = "aaaaaa';

key = (int)myhash(in);

p = myalloc();
strncpy(p->data, in, 7);
p->prev = hTable[key];
hTable[key] = p;
```

single linked list 와 비교해서 바뀐 내용은 저장을 pList 가 아닌 hTable[key] 에 한다는 점과 이전에는 저장되는 데이터가 int 였으나, 지금은 6자리의 문자열이라는 점 입니다.

마지막으로 Hash table 에서 검색하는 경우는

- 검색 문자열에서 Hash key 생성

- Hash table [key] 에서 검색

하시면 됩니다.

```c
char search[7] = "vrvipy";

int k = (int)myhash(search);

for (NODE * pp = hTable[k]; pp != NULL; pp = pp->prev)
{
  if (!strncmp(search, pp->data, 6))
    cout << "FOUND : " << pp->data << endl << endl;
}
```

Test case 마다 초기화 하실때는 arr_idx 와 Hash table 을 모두 초기화하셔야 합니다.

```c
arr_idx = 0;
for(int i = 0; i < MAX_TABLE; i++){
  hTable[i] = nullptr;
}
```

추가로 삭제에 관한 사항입니다.

Sinlgle linked list, Hash 는 삭제를 고려하여 만들어진 것이 아니라 삭제코드는 좀 어렵습니다.

삭제가 반드시 필요한 경우에는 dummy tail 을 넣어서 삭제하는 방법을 쓰시는 것이 더 편합니다.

(#6. 더블 링크드 리스트의 삭제를 참고하시기 바랍니다)

아래는 전통적인 삭제 코드입니다.

삭제는  해시키를 계산한 후,

hTable[key] 에서 삭제할 데이터를 검색, 검색된 노드의 이전 노드의 prev에 검색된 노드의 prev를 넣어주면 됩니다.

Single linked list 는 이전 노드를 찾아 갈 수 없으므로,
이전 노드의 prev 를 저장해야 하는 어려움이 있습니다.

```c
  char search[7] = "vrvipy";
  int k = (int)myhash(search);
  NODE **del = &hTable[k];  //이전 노드의 위치를 저장할 포인터 변수

  for (NODE * iter = hTable[k]; iter != nullptr; iter = iter->prev){
    //hTable[k]에서 모두 검색
    if (!strncmp(search, iter->data, 6)) {   //삭제할 데이터를 찾으면

      cout << "FOUND & DEL : " << iter->data << endl << endl;
      *del = iter->prev;   //검색된 노드의 prev를   이전 노드 prev에 저장
    }
    del = &iter->prev;  //이전 노드의 prev의 주소를 갖고 있음(포인터의 주소)
  }
```
