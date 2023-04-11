---
layout: posts
title: Multiprocess_2 concurrent
date: 2023-04-11 23:17:02
updated: {{ date }}
tags: 
- cirrus
- multiprocess
categories: Teckknowledge
---

## 收获
<mark>1. time.perf_counter()  程序运行时间包括sleeping</mark>, 不用再 time.time()-starttime了。
2. time.process_counter() cpu运行时间，不包括sleeping
<mark>3. concurrent.futures: 两个process 并行运行 </mark>
4. 注意 python3.7以上才能打印出秒数
5. 注意不要用和包名相同的文件名
<mark>6. Pool.map 可以取代 map(func, list)</mark>



```python
import concurrent.futures

import time

def sleep_for_a_bit(secondes):
    print(f"Sleeping {secondes} second(s)")
    time.sleep(secondes)
    return "Done sleeping"

with concurrent.futures.ProcessPoolExecutor() as executor:
    if __name__ == "__main__":
        f1 = executor.submit(sleep_for_a_bit, 1)
        f2 = executor.submit(sleep_for_a_bit, 1)
        print(f1.result())
        print(f2.result())
        
finish = time.perf_counter()

print("Finished in time: ", finish)
```

    ~/Desktop 🔥» python ccc.py                                      vivi@vivis-MBP
    Finished in time:  0.103320167
    Finished in time:  0.100869
    Sleeping 1 second(s)
    Sleeping 1 second(s)
    Finished in time:  0.099204041
    Finished in time:  0.095198
    Finished in time:  0.077157417
    Finished in time:  0.084327958
    Finished in time:  0.080791833
    Finished in time:  0.084751625
    Done sleeping
    Done sleeping
    Finished in time:  1.282027666
    

##  Pool.map(func, list) 来取代 list(map(func, list))
1. 但是看上去没发现节省时间

```python

x = list(map(req_get, create_list.keys()))

def run_get():
    with Pool(core_num) as p:
        x = p.map(req_get, create_list.keys())
        return x

```
    If x equals get_list:369,  369
    [2023-04-11 14:08:28] [Line: 184] INFO - Total running time: 53.477982
    [2023-04-11 14:08:28] [Line: 185] INFO - Create clients number:369
    [2023-04-11 14:08:28] [Line: 186] INFO - Get clients number: 369
    [2023-04-11 14:08:28] [Line: 187] ERROR - Create clients fail number: 0
    [2023-04-11 14:08:28] [Line: 188] ERROR - Create clients fail information: []
    369
    369
    369
    369