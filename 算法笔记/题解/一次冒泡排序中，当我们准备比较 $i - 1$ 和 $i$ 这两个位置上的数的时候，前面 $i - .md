---
title: >-
  一次冒泡排序中，当我们准备比较 $i - 1$ 和 $i$ 这两个位置上的数的时候，前面 $i - 1$ 个数中的最大值一定已经移到了 $i - 1$
  这个位置
updated: 2023-09-14 13:09:37Z
created: 2023-09-14 09:01:41Z
latitude: 30.64980000
longitude: 104.05550000
altitude: 0.0000
---

一次冒泡排序中，当我们准备比较 $i - 1$ 和 $i$ 这两个位置上的数的时候，前面 $i - 1$ 个数中的最大值一定已经移到了 $i - 1$ 这个位置上，如果该数大于 $a_i$，那么我们将会交换这两个数的位置，换种说法，每次冒泡排序将会使得每个排在 $a_i$ 前面并且比它大的数的个数减少 $1$。一直减为 0 为止。 