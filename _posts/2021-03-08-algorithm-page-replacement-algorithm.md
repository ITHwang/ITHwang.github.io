---
layout: single
title: "페이지 교체 알고리즘(Page Replacement Algorithm)이 필요한 이유"
---

페이지 교체 알고리즘 중 하나인 [LRU 알고리즘](https://github.com/ITHwang/ITHwang.github.io/blob/master/_posts/2021-03-07-algorithm-lru-cpp.md)에 대해 포스팅하다가, 이러한 알고리즘이 왜 필요한지 알아야 할 것 같았다.

이유는 <u>'효과적인 page replacement를 통해 메모리를 최적화하기 위해'</u>다. 다만 이 글에서는 page replacement가 발생하는 <strong>paging 기법</strong>과 이 기법을 뒷받침해주는 <strong>virtual memory</strong>에 대해 살펴보면서 더 깊은 이해를 도모하고자 한다.

## Virtual memory(or logical memory)
---

1. Concept

Memory와 정보를 교환하는 CPU가 memory에서 참조할 수 있는 데이터 양은 상대적으로 적다. 이에 가상으로 disk의 데이터까지 담긴 virtual memory를 이용해, CPU가 disk의 데이터까지 참조할 수 있는 것처럼 설계하여 작은 memory size를 보완했다.(실제 memory를 virtual memory와 구분하기 위해 보통 physical을 붙인다.)

2. Page table

따라서 유저는 virtual memory로부터 두 유형의 데이터를 참조할 수 있게 된다. 하나는 physical memory 안에 있어서 바로 참조되는 데이터이며, 다른 하나는 physical memory 안에 없어서 disk에서 찾아 저장된 후 참조되는 데이터다. 그리고 이들을 구분하기 위해 page table이 만들어진다.

<img src="https://github.com/ITHwang/ITHwang.github.io/blob/master/_images/20210308-page-replacement-1.png" alt="page-replacement-1" width="400"/>

page table에서 valid bit이면 physical memory에 존재하는 데이터이며, invalid bit이면 physical memory에 존재하지 않아 disk까지 가서 찾아야 하는 데이터다. 

3. Page fault

위 그림을 예로 들어 유저가 logical memory에서 `A`를 참조한다면, page table에서 physical memory 4번 frame 위치에 있다는 것을 알 수 있어 바로 가져올 수 있다. 그러나 `B`를 참조한다면 page table에서 찾을 수 없으므로 physical memory에 없다는 것을 알 수 있어, os가 disk에서 `B`를 찾아 physical memory에 저장하고 유저에게 가져다 준다. 

`B`의 참조 예시처럼 데이터가 실제 메모리에 없어서 os의 도움을 받아야 하는 상황을 <strong><u>page fault</u></strong>라고 한다. 아래 그림에서 page fault가 발생했을 때 일어나는 과정을 살펴볼 수 있다.

<img src="https://github.com/ITHwang/ITHwang.github.io/blob/master/_images/20210308-page-replacement-2.png" alt="page-replacement-2" width="400"/>

1. `load M` 명령을 받아 page table에서 `M` 참조를 시도한다.
2. 참조하지 못해 **page fault**가 발생하고 os에게 도움을 요청한다.
3. os는 disk에 있는 `M`을 참조한다.
4. `M`을 physical memory 속 빈 공간에 저장한다.
5. page table을 갱신한다.
6. `load M`을 다시 수행한다. 

## Paging
---

1. Concept

이처럼 관리되는 메모리를 단위로 수행하는 기법이 **paging**이다. 구분해야 할 점은, virtual memory는 <u>page</u> 단위로 정보를 저장하는 반면 physical memory는 <u>frame</u> 단위로 저장한다는 것이다. 그러나 둘 다 같은 크기의 단위고 역할도 같아서 단순 용어 차이로 보면 될 것 같다.

<img src="https://github.com/ITHwang/ITHwang.github.io/blob/master/_images/20210308-page-replacement-3.png" alt="page-replacement-3" width="400"/>

그림을 보니 아마도 virtual memory와 physical memory을 mapping해주는 page table에서 (page, frame) 쌍을 만들기 위해 용어로 구분지은 것이 아닌가 싶다.

2. Page replacement

page replacement는 physical memory와 disk 사이의 swap을 의미한다. 이 swap은 page fault가 발생하고 os가 disk에 있던 정보를 physical memory에 저장하려는데 physical memory가 꽉 찬 경우에 일어난다. 어쩔 수 없이 os는 physical memory에서 하나를 disk로 빼고 저장하려 했던 것을 뺀 자리에 배치한다.

<img src="https://github.com/ITHwang/ITHwang.github.io/blob/master/_images/20210308-page-replacement-4.png" alt="page-replacement-4" width="400"/>

1. os가 physical memory에서 하나를 빼 자리를 만든다.
2. 뺀 데이터에 대해 page table을 갱신한다.(from valid to invalid)
3. 넣고자 하는 데이터를 빈 자리에 넣는다.
4. 넣은 데이터에 대해 page table을 갱신한다.(from invalid to valid)

## 네 결국,
---

page replacement가 필요할 때 physical memory에서 빼고자 하는 <u><i>victim 데이터를 어떤 데이터로 정할 것인지</i></u>가 중요하다. 신중해야 할 것이, user interface로부터 자주 참조되는 데이터를 계속 disk로 swap out하면 page fault와 page replacement가 더욱 빈번히 발생하면서 불필요한 비용이 커지기 때문이다. 

반면, 거의 참조되지 않는 데이터를 disk로 swap out해놓으면 이 데이터를 swap in할 상황이 적게 발생하므로, page fault와 page replacement가 또한 적게 발생하면서 메모리 참조 비용을 덜 수 있다.

따라서 페이지 교체 알고리즘은 victim 데이터를 결정하는 알고리즘이며 메모리 최적화를 위한 알고리즘이다. 예시로 FIFO(First In First Out), OPT(OPTimal replacement), [LRU(Least Recently Used)]((https://github.com/ITHwang/ITHwang.github.io/blob/master/_posts/2021-03-07-algorithm-lru-cpp.md)), LFU(Least Frequently Used) 등이 있다.

## Reference
---

중앙대학교 전자전기공학부 운영체제(박호현 교수님, 2020-2) 9, 10주차