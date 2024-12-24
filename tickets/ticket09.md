## 9.1 Кластеризация. DBSCAN. Agglomerative clustering, критерии объединения, органичение на связность

[Момент в лекции](https://youtu.be/mR3t3xN1J_I?si=apFFTRGotDSLSeRY&t=3610)

###  DBSCAN

####  Преимущества алгоритма

* Число кластеров, на которые нужно разделить, изначально не задано.

* Алгоритм позволяет выделять аномалии.

* Векторное пространство не требуется.

####  Недостатки алгоритма

* Необходимо подбирать $\epsilon, m$.

####  Алгоритм

Core samples (ядерные точки) - точки, которые содержат в своей $\epsilon$ окрестности хотя бы $m$ других точек.

1. Изначально каждая точка - отдельный кластер.

2. Вычисляем все core samples.

3. Объединяем core samples в один кластер, если они находятся в $\epsilon$ окрестности друг друга.

4.  Все точки, которые находятся в $\epsilon$ окрестности, также относим к соотвествующему кластеру. Остальные точки - аномалии. 

### Agglomerative clustering

Относится к алгоритмам иерархической кластеризации (множество алгоритмов кластеризации, направленных на создание иерархии вложенных разбиений исходного множества объектов). То есть в результате агломеративной кластеризации
получим множество разбиений на кластеры.

####  Преимущества алгоритма

1. Получаем множество разбиений на кластеры.

2. Есть способы избежать необходимости векторного пространства

#### Критерии объединения

1. maximum - максимальное расстояние между точками двух кластеров.

2. average - среднее расстояние между парой точкой, первая находится в первом кластере, вторая - во втором.

3. ward - дисперсия объединенного кластера. Представим, что объединили два кластера. Найдем центроид и посчитаем variance.

* $\sum (\mu - x)^2$, если есть векторное пространство

* $\sum_{i} \sum_{j} (x_i - x_j)^2$, если векторного пространства нет.

То есть алгомеративную кластеризацию можно использовать без векторного пространства.

#### Алгоритм

1. Каждая точка - отдельный кластер.

2. Находим 2 кластера, расстояние по заданному критерию между которыми минимально. Объединяем их.

3. Повторяем 2, пока не останется 1 кластер.

#### Ограничение на связность

Чтобы вместо такой ситуации:

<img src="images/tickets09_1.png" width="300" />

получить такую (*связные компоненты*):

<img src="images/tickets09_2.png" width="300" />

Добавим ограничение на связность - объединяем кластеры только, если минимальное расстояние между точками этих кластеров $\leq \epsilon$. В таком случае результатом агломеративной кластеризации может получиться не 1 кластер, если для оставшихся не выполняется ограничение на связность.

## 9.2 Нейронные сети. Функции активации. Функции выхода и ошибки для классификации и регрессии

[Момент в лекции](https://youtu.be/kzhP504D4v8?si=4O1d9XevNrVhSpt-&t=3376)

### Функции активации

Функции активации так называются, потому что активируем нейрон

Функции активации:

1. Sigmoid $\sigma(s) = \frac{1}{1 + e^{-s}} \in [0; 1]$

2. Tanh $\sigma(s) = \frac{e^s - s^{-s}}{e^s + e^{-s}} \in [-1; 1]$

3. ReLU $\sigma(s) = \max(0, s) \in [0; +\infty)$

Функции активации применяются на промежуточных слоях нейронной сети.

### Функции выхода и ошибки

|Задача|Функция выхода|Ошибка|
|------|--------------|------|
|Бинарная классификация|Сигмоид|NLLL|
|Регрессия|s|MSE|
|Мультиклассовая классификация|SoftMax|Cross Entropy|

### Пояснение для мультиклассовой классификации

Для мультиклассовой классификации нужна функция на выходе, чтобы преобразовать нейроны в вероятности классов. Например, первый нейрон в $P(y = 0 | x)$, второй $P(y = 1 | x)$ и так далее.

$x_j^L = \sigma^L(s_j^L) = \frac{e^{s_j^L}}{\sum e^{s_i^L}}$

$L(w) = -\sum o_i \cdot \log(x_i^L)$ - функция ошибки. $o$ - индикатор.

$$
o_i = \begin{cases}
1, y = i \\
0, иначе
\end{cases}
$$

Можно сделать хитрее: взять $\log$ от выходного вектора и умножить на вектор $o$ - one hot encoding. Такой NLLL называется Cross-Entropy.