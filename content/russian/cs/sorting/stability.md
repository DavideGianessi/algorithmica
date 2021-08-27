---
title: Стабильные сортировки
weight: 9
draft: true
---

Пусть нам задан массив пар `[{1, 'a'}, {3, 'b'}, {1, 'c'}]`, который мы хотим отсортировать по возрастанию первого элемента (числа) в паре.

Мы можем модифицировать сортировку подсчетом

```cpp
void count_sort(int *a, int n) {
    int c[100] = {0};

    for (int i = 0; i < n; i++)
        c[a[i]]++;

    int k = 0;
    for (int i = 0; i < 100; i++)
        while (c[i]--)
            a[k++] = i;
}
```

Массив $cnt$ будет иметь вид `[0, 2, 0, 1]`, а массив $pref$ -
`[0, 2, 2, 3]`. Посмотрим, что будет происходить с каждой итерацией
последнего цикла.

1.  $i = 0,\\ pref\[a\[i\].first\] - 1 = 1 \\Longrightarrow res =
    \\text{\[null, \\{1, 'a'\\}, null\]},\\ pref = \[0, 1, 2, 3\]$
2.  $i = 1,\\ pref\[a\[i\].first\] - 1 = 2 \\Longrightarrow res =
    \\text{\[null, \\{1, 'a'\\}, \\{3, 'b'\\}\]},\\ pref = \[0, 1, 2,
    2\]$
3.  $i = 2,\\ pref\[a\[i\].first\] - 1 = 0 \\Longrightarrow res =
    \\text{\[\\{1, 'c'\\}, \\{1, 'a'\\}, \\{3, 'b'\\}\]},\\ pref = \[0,
    0, 2, 2\]$

Элементы `{1, 'a'}` и `{1, 'c'}` теперь идут в обратном порядке. Почему
так происходит? Когда мы обрабатываем элемент $i$, мы уменьшаем
значение в $pref\[a\[i\]\]$ на единицу, то есть, когда мы
встретим еще один элемент (например с индексом $j \> i$) с таким
же значением, для него $pref\[a\[j\]\]$ окажется меньше, и сам элемент
$a\[j\]$ займет позицию с меньшим индексом в отсортированном массиве.
Чтобы этого не происходило, давайте изменим направление последнего
цикла на $n-1\\ldots0$. Таким образом код становится следующим:

``` Python
for i=0..n-1:
    cnt[a[i]]++
pref[0] = 0
for i = 1..C:
   pref[i] = pref[i - 1] + cnt[i]
for i=n-1..0:
    res[pref[a[i]] - 1] = a[i]
    pref[a[i]]--
```