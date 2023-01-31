# 02-DataStructure2

 - 정리한 문제
 1. 14425 문자열 집합
 2. 11279 최대 힙
 3. 2075 N번째 큰 수
 4. 21939 문제 추천 시스템 Version 1
 <br>

 ## 14425 문자열 집합

 ### 설명
 - N개의 문자열로 이루어진 집합 S가 주어졌을 때, 주어지는 M개 문자열 중에서 집합 S에 속하는 문자열이 몇 개인지 구하는 문제이다.
 - N개의 문자열이 주어지면 그 문자열을 key값으로 0을 value값으로 하는 딕셔너리를 만든다.
 - M개의 문자열을 차례로 돌면서 집합S안에 존재하면 count 값을 +1 해준다. 딕셔너리는 해시테이블로 구현되어 있어 내용을 순차적으로 보는 것이 아니라 해당 단어를 바로 찾아가기 때문에  이 과정은  시간복잡도가 O(1)이다.

```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

s = {}
count = 0

for i in range(n):
  str = input().rstrip()
  s[str] = 0

for i in range(m):
  problem = input().rstrip()
  if(problem in s):
    count += 1

print(count)

```

 ## 11279 최대 힙
 ### 설명
 - 우선순위 큐를 활용해 푸는 문제이다.
 - heapq는 기본으로 최소 힙이기 때문에, 값을 넣을 때 - 부호를 붙여 넣어 최대힙으로 만들어준다.

```python
import sys
import heapq
input = sys.stdin.readline

n = int(input())
heap = []

for i in range(n):
  num = int(input())
  if(num == 0):
    if(heap):
      print(-heapq.heappop(heap))
    else:
      print(0)
  else:
    heapq.heappush(heap, -num)
```

## 2075 N번째 큰 수

 ### 설명
 - N×N의 표에 채워진 수들 중 N번째로 큰 수를 찾는 문제이다. (특징 : 모든 수는 자신의 한 칸 위에 있는 수보다 크다)
 - 메모리 제한이 있어 N×N개의 수를 다 저장하는 방법으론 불가능하다.
 - N번째로 큰 수를 찾는 것이므로 크기가 N인 최소 힙을 이용하여 푼다.
	 - 힙의 크기가 N보다 작을 경우
		 - 힙에 수를 push해준다.
	- 힙의 크기가 N보다 크거나 같을 경우
		- 주어진 수가 현재 힙에서 제일 작은 수보다 작다면 그냥 PASS
		- 주어진 수가 현재 힙에서 제일 작은 수보다 크다면 pop하고 주어진 수를 넣어준다.


```python
import sys
import heapq
input = sys.stdin.readline

n = int(input())

heap = []

for _ in range(n):
  for i in list(map(int, input().split())):
    if(len(heap) < n):
      heapq.heappush(heap, i)
    else:
      if(heap[0] < i):
        heapq.heappop(heap)
        heapq.heappush(heap, i)

print(heapq.heappop(heap))
```

## 21939 문제 추천 시스템 Version 1
 조건들이 까다로워 많이 틀렸던 문제였다. 결국 **추천 문제 리스트에 없는 문제 번호만 입력으로 주어진다. 이전에 추천 문제 리스트에 있던 문제 번호가 다른 난이도로 다시 들어 올 수 있다** 라는 조건을 어떻게 해결할지 모르겠어서 해당부분은 구글링을 참고해서 풀었다. 💦

 ### 풀이 설명
 - 문제 번호, 난이도를 가진 문제들이 주어졌을 때, 명령어에 따라 가장 어려운 문제의 번호를 출력하거나, 가장 쉬운 문제의 번호를 출력한다.
 - 난이도가 높은 순으로 된 힙, 난이도가 낮은 순으로 된 힙 두개를 사용한다. 즉, **이중 우선순위큐** 를 활용한다.
 - 이중 우선순위 큐를 사용할 때 포인트는 **두 큐를 서로 동기화** 해주는 것이다.
	 - 나는 문제 번호를 key로 가지고 boolean값을 value로 가진 dict를 활용하여 해당 문제가 리스트 내에 있는지 없는지 구분해주고, 없다면 취급해주지 않는 방법으로 동기화 해주었다.
- 이 문제에 주어진 조건 중 **추천 문제 리스트에 없는 문제 번호만 입력으로 주어진다. 이전에 추천 문제 리스트에 있던 문제 번호가 다른 난이도로 다시 들어 올 수 있다**라는 조건이 좀 어려웠는데, 상황을 따져 보자면

> `10번 문제[❌]는 heqp에서 pop되었거나 "solved" 처리되어 리스트에 없는 문제, 10번 문제[⭕]는 새로 들어와 리스트에 있는 문제라고 하자.`

- 10번 문제[❌]가 heqp에서 pop되었거나 "solved" 처리되어 in_list[10] = False인 상황
- 10번 문제[⭕]를 "add"하라는 명령이 들어옴
- 아직 in_list[10] = False일 뿐이지 실제로 heap내에는 10번 문제[❌]가 들어있을 수 있다.
- 따라서 그냥 add를 해버리고 in_list[10] = Ture로 바꿀 경우 이후 턴에서 10번 문제[❌]가 리턴 될 수 있다.
- 해결 ➡️ add하기 전에 heap의 top부분에  in_list[*] = False인 것이 있다면 다 POP해서 없애준다.

 ### 코드 설명
 - 중복된 코드를 함수로 묶어 메인로직을 가독성 있게 해주었다.
 1. addToBothHeap(problem, level) 함수
	 - 문제 번호와 난이도를 매개변수로 받아 이중 우선순위 큐에 넣어준다. 이때 in_list를 True로 설정해준다.  
 2. popMaxHeapWhileTopInList() 함수
	 -  Max_Heap의 top에 있는 원소가 리스트 내에 있는 함수일 때까지 계속 pop해준다.
 3. popMinHeapWhileTopInList() 함수
	 - Min_Heap의 top에 있는 원소가 리스트 내에 있는 함수일 때까지 계속 pop해준다.
<br>

```python
import sys
import heapq
from collections import defaultdict
input = sys.stdin.readline

max_heap = []
min_heap = []
in_list = defaultdict(bool)

def addToBothHeap(problem, level):
  heapq.heappush(max_heap, (-level, -problem))
  heapq.heappush(min_heap, (level, problem))
  in_list[problem] = True

def popMaxHeapWhileTopInList():
  while(not in_list[-max_heap[0][1]]):
        heapq.heappop(max_heap)

def popMinHeapWhileTopInList():
  while(not in_list[min_heap[0][1]]):
        heapq.heappop(min_heap)

n = int(input())

for i in range(n):
  problem, level = map(int, input().split())
  addToBothHeap(problem, level)

m = int(input())

for i in range(m):
  operation = input().split()
  if(operation[0] == "recommend"):
    if(operation[1] == '1'):
      popMaxHeapWhileTopInList()
      print(-max_heap[0][1])
    else:
      popMinHeapWhileTopInList()
      print(min_heap[0][1])
  elif(operation[0] == "add"):
    popMaxHeapWhileTopInList()
    popMinHeapWhileTopInList()
    problem, level = int(operation[1]), int(operation[2])
    addToBothHeap(problem, level)
  elif(operation[0] == "solved"):
    problemNum = int(operation[1])
    in_list[problemNum] = False


```
