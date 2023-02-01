# 03-그리디

 - 정리한 문제
 1. 14916 거스름돈
 2. 2217 로프
 3. 11508 2+1 세일
 4. 13305 주유소
 <br>

 ## 14916 거스름돈

 ### 설명
 -  5원과 2원으로 거슬러 줄 때 최대한 5원이 많도록 거슬러 주면 최소동전으로 돌려줄 수 있다.
 -  최대로 거슬러 줄 수 있는 5원의 개수에서 -1 해가면서 2로 나누어 떨어지는지 확인해준다.
 -  마지막 경우까지 나누어 떨어지지 않으면 -1 (=거슬러 줄 수 없음) 을 출력한다.

```python
import sys
input = sys.stdin.readline

money = int(input())

for cnt5 in range(money//5, -1, -1):
  balance = money - (cnt5*5)
  if(balance%2 == 0):
    print(cnt5 + balance//2)
    break
  if(cnt5 == 0):
    print(-1)
```

 ## 2217 로프
 ### 설명
 - 사용할 로프의 개수를 N이라고 하면, 물체의 최대중량은 N x (N개 로프의 최대 중량 중 최소)가 된다.
 - 로프가 버틸 수 있는 최대 중량을 구하므로 내림차순 정렬 후 최대 중량이 큰 로프부터 차례로 포함시키며 계산하면서 최대값을 갱신해준다.

```python
import sys
input = sys.stdin.readline

num = int(input())

w = [int(input()) for i in range(num)]
w.sort(reverse = True)
max_weight = 0

for i in range(len(w)):
  max_weight = max(max_weight, (i+1)*w[i])

print(max_weight)
```

## 11508 2+1 세일

 ### 설명
 - 내림차순 정렬 후 3의 배수 - 1 (인덱스는 0부터 시작하므로)  자리의 수들만 빼주면 된다.
 - 문제를 읽고 너무 쉬워서 뭔가 함정이 있을거라 의심했는데, 아니었다.💦 그냥 그리디는 정렬을 자주 같이 이용해서 푼다는걸 의도한 문제같다.


```python
import sys
input = sys.stdin.readline

num = int(input())

price = [int(input()) for i in range(num)]
price.sort(reverse = True)
min_price = sum(price)

for i in range(num//3):
  idx = 3*(i+1) - 1
  min_price -= price[idx]

print(min_price)
```

## 13305 주유소

 ### 설명
 - 개인적으로 풀이를 떠올리는거 보다 구현이 생각보다 까다로웠던 문제였다.
 - 풀이의 포인트는 현재 도시에서 다음 도시의 주유소를 염탐하는 것이다.
 - 현재 도시의 주유소보다 다음 주유소의 가격이 비쌀 경우
	 - 그렇다면 다음 도시까지 가는 주유는 현재 도시에서 하는걸로 하고 다음 주유소로 넘어가 가격을 확인해준다.
- 그러다 현재 도시의 주유소보다 싼 가격의 주유소를 발견했다면 그 주유소까지 거리 x 현재 도시의 리터당 가격 만큼을 더해준다. 이때 현재위치를 발견한 주유소가 있는 도시로 바꿔준다.
- 마지막 도시일 경우에는 주유소 가격을 비교할 것도 없이 마지막 도시까지 갈 주유비를 더해줘야 한다.

```python
import sys
input = sys.stdin.readline

city_num = int(input())
length = list(map(int, input().split()))
price = list(map(int, input().split()))

min_price = 0
#현재 0번째 도시에 있다.
now = 0

for i in range(city_num):
  if(price[now] > price[i]) or (i == city_num-1):
    min_price += price[now]*sum(length[now:i])
    now = i

print(min_price)
```
