---
layout: post
title:  "(LEETCODE)933. Number of Recent Calls"
summary: "You have a RecentCounter class which counts the number of recent requests within a certain time frame."
author: seunghwan
date: '2024-02-17 00:00:00 +0530'
category: ['leetcode', 'code test']
tags: Queue
usemathjax: false
permalink: /blog/leetcode/Number-of-Recent-Calls/
---
## 724. Find Pivot Index

### Situation

You have a `RecentCounter` class which counts the number of recent requests within a certain time frame.

Implement the `RecentCounter` class:

- `RecentCounter()` Initializes the counter with zero recent requests.
- `int ping(int t)` Adds a new request at time `t`, where `t` represents some time in milliseconds, and returns the number of requests that has happened in the past `3000` milliseconds (including the new request). Specifically, return the number of requests that have happened in the inclusive range `[t - 3000, t]`.

It is **guaranteed** that every call to `ping` uses a strictly larger value of `t` than the previous call.

**코드 아래를 보면 obj = RecentCounter()이 있는데 이는 Input에 “RecentCounter"와 같아 Output에 null을 넣어줄 필요가 없습니다.

### Task

- __init__에 필요한 배열을 생성합니다.
- ping() 함수에 조건에 맞는 로직*(`[t - 3000, t]` )을 추가해줍니다.

### Action

```python
class RecentCounter:

    def __init__(self):
        self.counter = []

    def ping(self, t: int) -> int:
        self.counter.append(t)
        cnt = 0
        for i in self.counter:
            if(t - 3000) <= i and i <= t:
                cnt += 1
        return cnt
```

### Result

Runtime: 8123 ms
Memory: 22.07 MB

*다른 방식의 코드

```python
class RecentCounter:

    def __init__(self):
        self.slide_window = deque()

    def ping(self, t: int) -> int:
        # step 1). append the current call
        self.slide_window.append(t)

        # step 2). invalidate the outdated pings
        while self.slide_window[0] < t - 3000:
            self.slide_window.popleft()

        return len(self.slide_window)

# Your RecentCounter object will be instantiated and called as such:
# obj = RecentCounter()
# param_1 = obj.ping(t)
```