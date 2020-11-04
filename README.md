Урок 1. Введение в задачу классификации. Постановка задачи и подготовка данных.

1. Приведите по 2 примера, когда лучше максимизировать Precision, а когда Recall.
2. Почему мы используем F-меру, почему, например, нельзя просто взять среднее от Precision и Recall?

1. Precision лучше максимизировать при состалвения вероятности прогноза погоды для принятие решения о вылете рейса из аэропорта. ( гроза/не гроза, туман/не туман). Recall (полнота) например при прогнозировании авиапроишествий среди большой авиации и авиации общего назначения
2. Чтобы не получать крайних результатов, нужно использовать F-меру, которая позволяет держать на должном уровне две метрики Precision и recall

Урок 2. Анализ данных и проверка статистических гипотез.

1. В чём различие между зависимыми и независимыми выборками?
2. Когда применяются параметрические статистические критерии, а когда — их непараметрические аналоги?

1. Следует отличать зависимые и независимые выборки, так как для них используются разные критерии при проверке гипотез.

Независимые выборки, если мы из генеральной совокупности случайным образомвыбрали какое-либо количество человек и поделили выбранных нами людей на две группы либо также случайно, либо относительно некоего признака, например, пола.

Зависимые выборки, если мы случайным образом выбрали из генеральной совокупности некоторые пары, сформировав из них две группы, (близнецы, муж-жена и т.д.) или же <мерили> одного и того же респондента до и после эксперимента. Иными словами, выборка парная, - когда один респондент в первой группе по какому-либо содержательному признаку сопоставляется с соответствующим респондентом во второй группе, например, муж из первой группы сравнивается со своей женой, которая во второй группе.

2. Критерий различия называют параметрическим, если он основан на конкретном типе распределения генеральной совокупности (как правило, нормальном) или использует параметры этой совокупности (среднее, дисперсии и т. д.).

Критерий различия называют непараметрическим, если он не базируется на предположении о типе распределения генеральной совокупности и не использует параметры этой совокупности. Поэтому для непараметрических критериев предлагается также использовать такой термин как ``критерий, свободный от распределения''.

При нормальном распределении генеральной совокупности параметрические критерии обладают большей мощностью по сравнению с непараметрическими (способны с большей достоверностью отвергать нулевую гипотезу, если последняя не верна).
