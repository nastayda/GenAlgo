http://art4art.narod.ru/ga_intro.html
Генетические алгоритмы (ГА) - это упрощенное моделирование процесса эволюции для решения програмистских задач.

Классический ГА реализуется следующим образом. Предположим, мы ищем решение некоторой задачи, представимое в двоичном виде (в виде последовательности нулей и единиц). Создается структура - "хромосома", состоящая из "генов".

"Хромосома" - это решение задачи. Например, 00010101010101010100000101011111100111

"Ген" - это атомарный кусочек хромосомы. В нашем случае - ноль или единица в определенной позиции.

Далее программируются операторы кроссовера и мутации.
Кроссовер (скрещивание) - это когда две хромосомы смешиваются и порождают третью хромосому, которая содержит в себе черты обеих родительских хромосом. Например, пусть мы имеем хромосомы 111111 и 000000. Выбираем случайную точку в хромосомах - допустим, между вторым и третьим геном. Тогда после кроссовера мы получим хромосому-потомка 110000 или 001111.
Мутация - это когда после кроссовера потомок в случайном порядке изменяется. Задается некоторая константа P - вероятность мутации гена . Допустим, после кроссовера мы получили хромосому 1100000. Применяя оператор мутации к каждому гену, мы с вероятностью Р меняем его на противоположный - ноль на единицу и единицу на ноль. Если, например, мы "попали" в заданную вероятность для 4-го гена, то после оператора мутации мы получим хромосому 110100.
Далее программируется фитнесс-функция. Она определяет "успешность" данной особи (хромосомы), т.е., близость ее к искомому ответу. Для сложных задач задание фитнесс-функции - наиболее непростая часть во всем генетическом алгоритме.
Кроме того, программируются методы селекции. Это отбор особей для скрещивания. Этот отбор должен происходить таким образом, чтобы с большей вероятностью скрещивались более успешные особи, т.е., особи, имеющие большее значение фитнесс-функции. Однако менее удачные особи также должны иметь шанс пройти кроссовер, ибо в их генах может быть что-то полезное.
Наиболее изученными являются пропорциональный(или рулеточный) отбор и турнирный отбор. Турнирный отбор работает быстрее, поэтому рассмотрим его. В случае турнирного отбора, для выбора особи отбираются N случайных особей. Затем из них отбирается особь с наибольшим значением фитнесс-функции. О пропорциональном отборе читать здесь: http://algolist.manual.ru/ai/ga/dioph.php
Дальше можно приступить к склеиванию всех этих кусочков и созданию самого ГА.
Вот шаги работы ГА:
Случайным образом создается исходная популяция. Как правило, задается в виде массива. Каждая особь(хромосома) в популяции - это случайный набор нулей и единиц заданной длины.
Каждая особь в популяции оценивается на успешность. Для этого применяется фитнесс-функция. Если искомое решение найдено, то алгоритм заканчивается (разумеется, при первой итерации вероятность этого очень мала).
Отбираются особи для кроссовера. Повторюсь: этот отбор должен происходить таким образом, чтобы с большей вероятностью скрещивались более успешные особи, т.е., особи, имеющие большее значение фитнесс-функции.
Отобранные особи скрещиваются, каждый потомок подвергается мутации с заданной вероятностью.
Из потомков формируется новая популяция, которая заменяет старую.
Возвращаемся к шагу 2.
Алгоритм может заканчиваться при достижении самых разных условий, например:
достигнуто оптимальное решение
пройдено максимальное заданное число итераций
прошло максимальное время, заданное для выполнения алгоритма
при переходе к новому поколению не происходит существенных изменений
и др.
Вот простенькие примеры реализации ГА на языке Java (все они легко могут быть переведены на другой язык):
1. Решение Диофантова Уравнения.
Нахождение целых решений уравнения a + 2*b + 3*c + 4*d = 30
Для селекции используется "пропорциональный отбор" (или "рулеточный" - Roulette Wheel Selection)
Файлы:
Chromosome.java - класс инкапсулирует хромосому и все, что с ней связано.
Diofant.java - все остальное. Запускать этот файл.


http://algolist.manual.ru/ai/ga/dioph.php
Пример ГА: Решение Диофантова уравнения
Перевод Кантор И.
Рассмотрим диофантово (только целые решения) уравнение: a+2b+3c+4d=30, где a, b, c и d - некоторые положительные целые. Применение ГА за очень короткое время находит искомое решение (a, b, c, d).
Конечно, Вы можете спросить: почему бы не использовать метод грубой силы: просто не подставить все возможные значения a, b, c, d (очевидно, 1 <= a,b,c,d <= 30) ?
Архитектура ГА-систем позволяет найти решение быстрее за счет более 'осмысленного' перебора. Мы не перебираем все подряд, но приближаемся от случайно выбранных решений к лучшим.
Для начала выберем 5 случайных решений: 1 =< a,b,c,d =< 30. Вообще говоря, мы можем использовать меньшее ограничение для b,c,d, но для упрощения пусть будет 30.
Хромосома
(a,b,c,d)
1
(1,28,15,3)
2
(14,9,2,4)
3
(13,5,7,3)
4
(23,8,16,19)
5
(9,13,5,2)
Таблица 1: 1-е поколение хромосом и их содержимое
Чтобы вычислить коэффициенты выживаемости (fitness), подставим каждое решение в выражение a+2b+3c+4d. Расстояние от полученного значения до 30 и будет нужным значением.
Хромосома
Коэффициент выживаемости
1
|114-30|=84
2
|54-30|=24
3
|56-30|=26
4
|163-30|=133
5
|58-30|=28
Таблица 2: Коэффициенты выживаемости первого поколения хромосом (набора решений)
Так как меньшие значения ближе к 30, то они более желательны. В нашем случае большие численные значения коэффициентов выживаемости подходят, увы, меньше. Чтобы создать систему, где хромосомы с более подходящими значениями имеют большие шансы оказаться родителями, мы должны вычислить, с какой вероятностью (в %) может быть выбрана каждая. Одно решение заключается в том, чтобы взять сумму обратных значений коэффициентов, и исходя из этого вычислять проценты. (Заметим, что все решения были сгенерированы Генератором Случайных Чисел - ГСЧ)
Хромосома
Подходящесть
1
(1/84)/0.135266 = 8.80%
2
(1/24)/0.135266 = 30.8%
3
(1/26)/0.135266 = 28.4%
4
(1/133)/0.135266 = 5.56%
5
(1/28)/0.135266 = 26.4%
Таблица 3: Вероятность оказаться родителем
Для выбора 5-и пар родителей (каждая из которых будет иметь 1 потомка, всего - 5 новых решений), представим, что у нас есть 10000-стонняя игральная кость, на 880 сторонах отмечена хромосома 1, на 3080 - хромосома 2, на 2640 сторонах - хромосома 3, на 556 - хромосома 4 и на 2640 сторонах отмечена хромосома 5. Чтобы выбрать первую пару кидаем кость два раза и выбираем выпавшие хромосомы. Таким же образом выбирая остальных, получаем:
Хромосома отца
Хромосома матери
3
1
5
2
3
5
2
5
5
3
Таблица 4: Симуляция выбора родителей
Каждый потомок содержит информацию о генах и отца и от матери. Вообще говоря, это можно обеспечить различными способами, однако в нашем случае можно использовать т.н. "кроссовер" (cross-over). Пусть мать содержит следующий набор решений: a1,b1,c1,d1, а отец - a2,b2,c2,d2, тогда возможно 6 различных кросс-оверов (| = разделительная линия):
Хромосома-отец
Хромосома-мать
Хромосома-потомок
a1 | b1,c1,d1
a2 | b2,c2,d2
a1,b2,c2,d2 or a2,b1,c1,d1
a1,b1 | c1,d1
a2,b2 | c2,d2
a1,b1,c2,d2 or a2,b2,c1,d1
a1,b1,c1 | d1
a2,b2,c2 | d2
a1,b1,c1,d2 or a2,b2,c2,d1
Таблица 5: Кросс-оверы между родителями
Есть достаточно много путей передачи информации потомку, и кросс-овер - только один из них. Расположение разделителя может быть абсолютно произвольным, как и то, отец или мать будут слева от черты.
А теперь попробуем проделать это с нашими потомками
Хромосома-отец
Хромосома-мать
Хромосома-потомок
(13 | 5,7,3)
(1 | 28,15,3)
(13,28,15,3)
(9,13 | 5,2)
(14,9 | 2,4)
(9,13,2,4)
(13,5,7 | 3)
(9,13,5 | 2)
(13,5,7,2)
(14 | 9,2,4)
(9 | 13,5,2)
(14,13,5,2)
(13,5 | 7, 3)
(9,13 | 5, 2)
(13,5,5,2)
Таблица 6: Симуляция кросс-оверов хромосом родителей
Теперь мы можем вычислить коэффициенты выживаемости (fitness) потомков.
Хромосома-потомок
Коэффициент выживаемости
(13,28,15,3)
|126-30|=96
(9,13,2,4)
|57-30|=27
(13,5,7,2)
|57-30|=22
(14,13,5,2)
|63-30|=33
(13,5,5,2)
|46-30|=16
Таблица 7: Коэффициенты выживаемости потомков (fitness)
Средняя приспособленность (fitness) потомков оказалась 38.8, в то время как у родителей этот коэффициент равнялся 59.4. Следующее поколение может мутировать. Например, мы можем заменить одно из значений какой-нибудь хромосомы на случайное целое от 1 до 30.
Продолжая таким образом, одна хромосома в конце концов достигнет коэффициента выживаемости 0, то есть станет решением.
Системы с большей популяцией (например, 50 вместо 5-и сходятся к желаемому уровню (0) более быстро и стабильно.
