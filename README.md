Урок 4. Оценка и интерпретация полученной модели. Обсуждение курсового проекта.

1. Расскажите, как работает регуляризация в решающих деревьях, какие параметры мы штрафуем в данных алгоритмах?
2. По какому принципу рассчитывается "важность признака (feature_importance)" в ансамблях деревьев?


Регуляризация - метод борьбы с переобучением, с помощью добавления некоторого штрафа за сложность модели.
L1-регуляризация (lasso, регуляризация через манхэттенское расстояние)
L2-регуляризация (ridge, регуляризация Тихонова)

Метод регуляризации заключается в "штрафовании" модели за слишком большие веса путем добавления нового члена к ошибке:
𝑄(𝑤,𝑋)+𝜆||𝑤||2→min𝑤.
 
добавленный член  𝜆||𝑤||2
  - квадратичный регуляризатор, который представляет собой  𝐿2
 -норму вектора весов, то есть сумму квадратов весов  ∑𝑑𝑗=1𝑤2𝑗
 , коэффициент  𝜆
  при нем - коэффициент регуляризации.
Чем больше его  𝜆
 , тем меньшая сложность модели будет получаться в процессе такого обучения.
- Если увеличивать его, в какой-то момент оптимальным для модели окажется зануление всех весов
- А при слишком низких его значениях появляется вероятность чрезмерного усложнения модели и переобучения
- Подбираем по кросс-валидации
По сути, смысл регуляризации заключается в минимизации функционала ошибки с ограничением весов.
{𝑄(𝑤,𝑋)→𝑚𝑖𝑛||𝑤||2≤𝐶

2. Среднее снижение точности, вызываемое переменной, определяется во время фазы вычисления out-of-bag ошибки. Чем больше уменьшается точность предсказаний из-за исключения (или перестановки) одной переменной, тем важнее эта переменная, и поэтому переменные с бо́льшим средним уменьшением точности более важны для классификации данных. Среднее уменьшение неопределенности Джини (или ошибки mse в задачах регрессии) является мерой того, как каждая переменная способствует однородности узлов и листьев в окончательной модели случайного леса. Каждый раз, когда отдельная переменная используется для разбиения узла, неопределенность Джини для дочерних узлов рассчитывается и сравнивается с коэффициентом исходного узла. Неопределенность Джини является мерой однородности от 0 (однородной) до 1 (гетерогенной). Изменения в значении критерия разделения суммируются для каждой переменной и нормируются в конце вычисления. Переменные, которые приводят к узлам с более высокой чистотой, имеют более высокое снижение коэффициента Джини.

Урок 3. Построение модели классификации.

1. Для чего и в каких случаях полезны различные варианты усреднения для метрик качества классификации: micro, macro, weighted?
2. В чём разница между моделями xgboost, lightgbm и catboost или какие их основные особенности?

1.  ``'micro'``:
        Calculate metrics globally by counting the total true positives,
        false negatives and false positives.
    ``'macro'``:
        Calculate metrics for each label, and find their unweighted
        mean.  This does not take label imbalance into account.
    ``'weighted'``:
        Calculate metrics for each label, and find their average weighted
        by support (the number of true instances for each label). This
        alters 'macro' to account for label imbalance; it can result in an
        F-score that is not between precision and recall.

любые метрики построенные с micro это accuracy.

все метрики показывают насколько мы хорошо предсказываем.

взвешенная метрика - сумма метрик нормаирования на кол-во объектов

Всё верно, можно для наглядности ещё привести формулы для их подсчета: micro, действительно, является accuracy: (TP + TN) / (TP + FP + TN + FN), учитываем все верные классификации и делим на кол-во объектов. macro - общая precision = (precision_0 + precision_1 / 2, простое среднее подсчитанной метрики weighted - общая precision = zeros / size * precision_0 + ones / size * precision_1, взевешанное среднее, учитываем кол-во объектов каждого класса

2. 

xboost и lightgbm - это усовершенствованные модели градиентного бустинга, которые работают быстрее и точнее предсказывают. 
Xboost работает не с энтропией, а с похожестью. Насколько в рамках одного листа находится похожих объектов. Есть возможность стричь деревья, что позволяет значительно ускорить процесс обучения.

LightGMB 

Gradient-based One-Side Sampling (GOSS)
GOSS сохраняет наблюдения с большим градиентом и случайно сэмплирует выборку из наблюдений с маленькими градиентами.
Итого, выбирается a∗100% наблюдений из топа с большими градиентами.
Рандомно b∗100% наблюдений из остальной.

Exclusive Feature Bundling (EFB)
Разряжённые данные значат, что многие признаки никогда ненулевые вместе. Это позволяет совместить ("bundle") эти фичи в одну без потери информации.
Пример, у фичи А диапазон значений (0, 10) и у B - (0, 20). Добавляется смещение 10 к значениям фичи B, теперь их диапазон (10, 30). После, можно соединять A и B и использовать "feature bundle" с диапазоном (0, 30), чтобы заменить оригинальные A и B.

Деревья строятся не в глубину, а по отдельным листам. 

CatBoost 
Категоричный бустинг.
Симметричные деревья. Вопросы дублируются в соседней ветке. Это нужно для быстрой выдачи ответов. Самостоятельно работает с переобучением. 

* Предотвращение переобучения
    * Обучается несколько модель за одну итерацию и высчитываются остатки по наблюдениям на тех моделях, которые не видели его.
    * Высокий уровень рандома (перемешивание выборки и много случайности в начале построения дерева)
* Работа с категориальными признаками


Урок 2. Анализ данных и проверка статистических гипотез.

1. В чём различие между зависимыми и независимыми выборками?
2. Когда применяются параметрические статистические критерии, а когда — их непараметрические аналоги?

1. Следует отличать зависимые и независимые выборки, так как для них используются разные критерии при проверке гипотез.

Независимые выборки, если мы из генеральной совокупности случайным образомвыбрали какое-либо количество человек и поделили выбранных нами людей на две группы либо также случайно, либо относительно некоего признака, например, пола.

Зависимые выборки, если мы случайным образом выбрали из генеральной совокупности некоторые пары, сформировав из них две группы, (близнецы, муж-жена и т.д.) или же <мерили> одного и того же респондента до и после эксперимента. Иными словами, выборка парная, - когда один респондент в первой группе по какому-либо содержательному признаку сопоставляется с соответствующим респондентом во второй группе, например, муж из первой группы сравнивается со своей женой, которая во второй группе.

2. Критерий различия называют параметрическим, если он основан на конкретном типе распределения генеральной совокупности (как правило, нормальном) или использует параметры этой совокупности (среднее, дисперсии и т. д.).

Критерий различия называют непараметрическим, если он не базируется на предположении о типе распределения генеральной совокупности и не использует параметры этой совокупности. Поэтому для непараметрических критериев предлагается также использовать такой термин как ``критерий, свободный от распределения''.

При нормальном распределении генеральной совокупности параметрические критерии обладают большей мощностью по сравнению с непараметрическими (способны с большей достоверностью отвергать нулевую гипотезу, если последняя не верна).

Урок 1. Введение в задачу классификации. Постановка задачи и подготовка данных.

1. Приведите по 2 примера, когда лучше максимизировать Precision, а когда Recall.
2. Почему мы используем F-меру, почему, например, нельзя просто взять среднее от Precision и Recall?

1. Precision лучше максимизировать при состалвения вероятности прогноза погоды для принятие решения о вылете рейса из аэропорта. ( гроза/не гроза, туман/не туман). Recall (полнота) например при прогнозировании авиапроишествий среди большой авиации и авиации общего назначения
2. Чтобы не получать крайних результатов, нужно использовать F-меру, которая позволяет держать на должном уровне две метрики Precision и recall
