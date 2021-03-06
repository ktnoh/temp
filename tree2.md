# 트리사용법( 자연수가 아닌경우 )

자연수가 아닌 ID 를 가지는 Tree 가 있는 경우 구성하는 방법은

Node의 ID 를 자연수 ID 로 변경해주면 됩니다.

각각의 NODE ID 를 자연수인 0, 1, 2 ~~ 로 변경해 주면 됩니다.

Tree name 이 저장이 필요한 경우 추가적인 전역 변수를 이용하거나

parent_info, child_info 와 함께 struct 으로 만들어 사용하면 됩니다.

```c
char name_info[MAX_NODE][7];
``` 

## 1. Tabel 을 이용한 변경

아래와 같은 테이블을 이용해서 ID "AAAAAA" 인 경우 0으로 바꾸어 사용하는 것입니다.

테이블에 참조할 값이 없는 경우 새로운 테이블을 추가하시면 됩니다. 

(첫번째 열은 배열의 index 와 동일하므로 코드 구현시에는 불필요 합니다.)

```c
0 AAAAAA
 
1 BBBBBBB
 
2 CCCCCC
``` 

```c
int num_table;
char name_table[MAX_NODE][7];
 
int getid_table(char *name)
{
 int i;
 for (i = 0; i < num_table; i++)  //전체 테이블에서
 {
  if (!strncmp(name, name_table[i], 6))  //name 을 검색합니다.
  {
   return i;
  }
}
 
 strncpy(name_table[i], name, 7);  //새로운 행을 추가합니다.
 num_table++;  //전체 테이블의 수를 늘입니다.
 
 return i;
}
``` 

초기화시에는 Table index인 num_table 만 0으로 초기화하면 됩니다.

Node ID 가 문자열, 매우 큰 숫자, 음수에 대해서도 대응이 가능할 것 같습니다.

Table 을 이용하는 방식은 사용이 간단한 반면

트리의 Node 수가 증가하는 경우 속도가 느려지는 단점이 있습니다.

## 2. Hash Table 을 이용하는 방법

Hash Table 을 이용하면 더 빠르게 사용이 가는합니다.

아래와 같이 Node name 으로 계산한 hash key 에 해당하는 Hash table 을 이용하여

검색해서 있으면, id를 리턴, 없으면 새로 추가한 다음 추가된 id 를 리턴 하시면 됩니다.

```c
Hash table( 0 ) :
Hash table( 1 ) : IIIIII(8)  DDDDDD(3)
Hash table( 2 ) :
Hash table( 3 ) : GGGGGG(6)  BBBBBB(1)
Hash table( 4 ) :
Hash table( 5 ) : JJJJJJ(9)  EEEEEE(4)
Hash table( 6 ) :
Hash table( 7 ) : HHHHHH(7)  CCCCCC(2)
Hash table( 8 ) :
Hash table( 9 ) : FFFFFF(5)  AAAAAA(0)
```

```c
struct HASH_NODE {
 int id;
 char name[7];
 HASH_NODE *prev;
} b[1000];
```

HASH_NODE 에는 name 과 id 까지 저장할 수 있도록 추가하였습니다.

```c
int getid_hash(char *name)
{
 int key = myhash(name);  //name 을 이용하여 hash key 를 만듭니다.
 
 
 for (HASH_NODE *pp = hash_table[key]; pp != nullptr; pp = pp->prev)  //Hash table(key) 에서 Name 을 검색합니다.
 {
  if (!strncmp(name, pp->name, 6))  //name 이 있는 경우  해당하는 id를 return 합니다.
   return pp->id;
 }
 
 //Hash Table 없으면 추가합니다.
 
 
 HASH_NODE *p = myalloc();
 p->id = id++;   // 새로운 id 를 부여합니다.
 strncpy(p->name, name, 7);
 p->prev = hash_table[key];
 hash_table[key] = p;
 
 return p->id;
}
``` 

초기화는 부여되는 id, hash_table, 배열의 인덱스인 hash_idx 를 초기화 해주셔야 합니다.

## 3. getid 이용한 tree 사용법

이제 Tree 연결 정보가 아래와 같이 Node name 으로  주어집니다.

```c
AAAAAA BBBBBB
AAAAAA CCCCCC
AAAAAA DDDDDD
BBBBBB EEEEEE
BBBBBB FFFFFF
FFFFFF GGGGGG
DDDDDD HHHHHH
DDDDDD IIIIII
DDDDDD JJJJJJ
```

주어진 Node name을 0, 1, 2, ~~~ 로 변경하여 사용하시면 됩니다.

```c
char pname[7], cname[7];
int parent, child;
 
cin >> pname >> cname;  //부모 - 자식간의 연결정보 
 
parent = getid_hash(pname);
 
 
child = getid_hash(cname);
``` 
