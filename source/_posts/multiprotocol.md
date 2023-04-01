---
layout: posts
title: Multiprocess_1 tasks
date: 2023-04-01 15:37:02
updated: {{ date }}
tags: 
- cirrus
- multiprocess
categories: Teckknowledge
---

```python
import multiprocessing


core_num = multiprocessing.cpu_count()
print(f"corenum: {core_num}")
clients = ["eb380adf-8fe8-45dd-825f-4e7da940eca9","990f5d3c-5e5f-4af8-b68b-1329272ac3b4",
           "15cc0813-8a2e-4a90-9ba6-7a8ddf3bd32d","5bf8729e-fb28-476e-b054-5a0656dc4fed",
           "27a1e6bb-439b-475e-bbcf-9744622992cd","6491b68c-5360-4638-8194-da9f5431c4eb",
           "da2fd513-acdc-48e9-b9fe-c0de13e90eb0","f75d48b0-74ab-4d96-b8a7-4357985e78bc",
           "d76c8dbe-0821-4736-a4fa-9b60dc331fec","354f9ef4-1481-469a-9e6a-9cef076b4ac0"]



"""
将clients_list 按core_num 随机分布client数量，这里core=4，
那么会将 clients 分成四组，for 循环每次调用 chunks，都取一组给 for clients_，
然后 for clients_ 可以拿去做事。
"""
def chunks(clients_list, num):
    d, r = divmod(len(clients_list), num)
    print(f"d and r: {d}, {r}")
    for i in range(num):
        si = (d + 1) * (i if i < r else r) + d * (0 if i < r else i - r)
        print(f"si: {si}")
        yield clients_list[si:si + (d + 1 if i < r else d)]

for clients_ in chunks(clients, core_num):
    from pdb import set_trace
    set_trace()
    print(f"clients_: {clients_}")
    print(f"chunks: {chunks}")
```    
    
    
```bash

    corenum: 8
    d and r: 1, 2
    si: 0
    ipdb> l
    ipdb> clients_
    ['eb380adf-8fe8-45dd-825f-4e7da940eca9', '990f5d3c-5e5f-4af8-b68b-1329272ac3b4']
    ipdb> n
    clients_: ['eb380adf-8fe8-45dd-825f-4e7da940eca9', '990f5d3c-5e5f-4af8-b68b-1329272ac3b4']
    ipdb> n
    chunks: <function chunks at 0x10c2f6310>
    ipdb> s
    ipdb> n
    ipdb> i
    1
    ipdb> n
    ipdb> n
    si: 2
    ipdb> si=2,  i1<r2所以返回d+1=3, 也就是返回clients_list[2:2+3],即第3，4，5个元素]
    ipdb> n
    > [0;32m<ipython-input-1-fca5ddbf22de>[0m(30)[0;36m<module>[0;34m()[0m
    ipdb> n
    clients_: ['15cc0813-8a2e-4a90-9ba6-7a8ddf3bd32d', '5bf8729e-fb28-476e-b054-5a0656dc4fed']
    ipdb> s
     ipdb> n
    si: 4
 
    ipdb> i
    2
    ipdb> si=4, i=2不小于r=2,于是返回d，[4,6] 第5，6个元素
    *** SyntaxError: cannot assign to literal
    ipdb> n
    clients_: ['27a1e6bb-439b-475e-bbcf-9744622992cd']   
    ipdb> 哦 上一步应该是[4,5] d=1记错了
    *** SyntaxError: invalid syntax
    ipdb> n
    chunks: <function chunks at 0x10c2f6310>
    ipdb> s
    --Call--
    > [0;32m<ipython-input-1-fca5ddbf22de>[0m(25)[0;36mchunks[0;34m()[0m
    ipdb> n
    ipdb> n
    ipdb> n
    ipdb> n
    si: 5
    > [0;32m<ipython-input-1-fca5ddbf22de>[0m(25)[0;36mchunks[0;34m()[0m
    ipdb> i
    3
    ipdb> d
    *** Newest frame
    ipdb> si=5, 返回 5+1，也就是[5,6] 一个元素
    *** SyntaxError: invalid syntax
    ipdb> n
    ipdb> n
    clients_: ['6491b68c-5360-4638-8194-da9f5431c4eb']
    ipdb> n
    chunks: <function chunks at 0x10c2f6310>
    ipdb> s
    ipdb> n
    ipdb> n
    ipdb> n
    ipdb> n
    si: 6
    ipdb> i
    4
    ipdb> [6:6+1]
    *** SyntaxError: invalid syntax
    ipdb> n
    clients_: ['da2fd513-acdc-48e9-b9fe-c0de13e90eb0']
    ipdb> n
    chunks: <function chunks at 0x10c2f6310>
    ipdb> n
    ipdb> n
    si: 7
    ipdb> [7,8]
    [7, 8]
    ipdb> n
    ipdb> n
    ipdb> n
    chunks: <function chunks at 0x10c2f6310>
    ipdb> n
    si: 8
    ipdb> [8,9]
    [8, 9]
    clients_: ['d76c8dbe-0821-4736-a4fa-9b60dc331fec']
 
    [0m
    ipdb> [9,10] 到这一步就结束啦
    *** SyntaxError: invalid syntax
    ipdb> n
    clients_: ['354f9ef4-1481-469a-9e6a-9cef076b4ac0']
    ipdb> 分给了8个进程，第一个进程有3个client要执行，另外7个进程每个执行一个client
    *** SyntaxError: invalid character '，' (U+FF0C)
    ipdb> n
 ```

