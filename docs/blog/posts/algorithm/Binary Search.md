---
draft: false
authors:
  - hyewon
date: 2024-01-03
categories:
  - Algorithm
---
# Binary Search(이분탐색)
<!-- more -->
mid, lt, rt모두 인덱스이다.  
찾고자하는 숫자 m  
mid번째 위치하는 숫자가 내가 찾고자하는 m인지를 확인한다.  
만약 찾고자하는 숫자가 mid 보다 작다면, mid 이상의 숫자들은 확인할 필요가 없으므로 rt를 mid-1로 옮긴다.
반대로 찾고자하는 숫자가 mid 보다 크다면, mid 이하의 숫자들은 확인할 필요가 없으므로 lt를 mid+1로 옮긴다.

```python
n, m = map(int, input().split())
a = list(map(int, input().split()))
a.sort()
lt = 0
rt = n-1

while lt <= rt:
    mid = (lt + rt) // 2
    if a[mid] == m:
        print(mid+1)
        break
    elif a[mid] > m:
        rt = mid - 1
    else:
        lt = mid + 1
```