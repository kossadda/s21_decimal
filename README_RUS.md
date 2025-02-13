# decimal_lib

Реализация собственной библиотеки decimal.h.

В данном проекте реализована библиотека decimal.h на языке программирования Си. Эта библиотека добавляет возможность работы с типом "decimal", который отсутствует в стандарте языка. Этот тип критически важен для, например, финансовых расчетов, где недопустимы погрешности вычислений, свойственные типам с плавающей точкой. В рамках этого проекта предполагается знакомство с задачами обработки финансовой информации, погружение в вопросы внутреннего представления различных типов данных и закрепление структурного подхода.

## Содержание

1. [Начало работы](#начало-работы)
    1. [Установка](#установка)
    2. [Использование](#использование)
2. [Описание проекта](#описание-проекта)
    1. [Описание пользовательского типа decimal_t](#описание-пользовательского-типа-decimal_t)
    2. [Двоичное представление decimal_t](#двоичное-представление-decimal_t)
3. [Функции](#функции)
    1. [Арифметические операторы](#арифметические-операторы)
    2. [Операторы сравнения](#операторы-сравнения)
    3. [Преобразователи](#преобразователи)
    4. [Другие функции](#другие-функции)
4. [Лицензия](#лицензия)



## Начало работы

Этот раздел содержит инструкции по началу работы с библиотекой decimal.h.

### Установка

Чтобы начать использовать библиотеку decimal.h, выполните следующие шаги:

1. Склонируйте репозиторий:

```bash
    git clone https://github.com/emmonbear/decimal_lib.git
```

2. Перейдите в каталог с проектом:

```bash
    cd decimal_lib/src
```

3. Соберите библиотеку с помощью команды Makefile:

```bash
    make decimal.a
```
4. Запустить тесты

```bash
    make test
```
*Необходимо установить библиотеку Check.h.*


5. Сформировать отчет о покрытии тестами:

```bash
    make gcov_report
```
*Необходимо установить gcovr.*

6. Запустить тесты на наличие утечек памяти:

```bash
    make valgrind
```
*Необходимо установить valgrind.*

7. Сформировать документацию:

```bash
    make dvi
```
*Документация формируется с использованием doxygen. Вам его необходимо предварительно установить.*

[Содержание](#содержание)

### Использование

После того как библиотека будет успешно собрана, вы можете начать использовать ее в своих проектах. Включите заголовочный файл decimal.h в свой код и начните использовать функции и типы данных библиотеки.

[Содержание](#содержание)

## Описание проекта

- Библиотека разработана на языке Си стандарта C11 с использованием компилятора gcc 
- Код библиотеки находится в папке src в ветке develop  
- Не использованы устаревшие и выведенные из употребления конструкции языка и библиотечные функции.
- При написании кода придерживался Google Style
- Решение оформлено как статическая библиотека (с заголовочным файлом decimal.h)
- Библиотека разработана в соответствии с принципами структурного программирования
- Подготовлено полное покрытие unit-тестами функций библиотеки c помощью библиотеки Check  
- Unit-тесты покрывают не менее 80% каждой функции  
- Предусмотрен Makefile для сборки библиотеки и тестов (с целями all, clean, test, decimal.a, gcov_report)  
- В цели gcov_report формируется отчёт gcov в виде html страницы. Для этого unit-тесты запускаются с флагами gcov  
- Запрещено использование типа __int128
- Определяемый тип поддерживает числа от -79,228,162,514,264,337,593,543,950,335 до +79,228,162,514,264,337,593,543,950,335.

[Содержание](#содержание)

### Описание пользовательского типа decimal_t

Тип decimal представляет десятичные числа в диапазоне от положительных 79,228,162,514,264,337,593,543,950,335 до отрицательных 79,228,162,514,264,337,593,543,950,335. Значение Decimal по умолчанию равно 0. Decimal подходит для финансовых расчетов, которые требуют большого количества значимых целых и дробных цифр и отсутствия ошибок округления. Этот тип не устраняет необходимость округления. Скорее, сводит к минимуму количество ошибок из-за округления.

Когда результат деления и умножения передается методу округления, результат не страдает от потери точности.

Decimal число - это значение с плавающей точкой, состоящее из знака, числового значения, где каждая цифра находится в диапазоне от 0 до 9, и коэффициента масштабирования, который указывает положение десятичной точки, разделяющей целые и дробные части числового значения.

Двоичное представление Decimal состоит из 1-разрядного знака, 96-разрядного целого числа и коэффициента масштабирования, используемого для деления 96-разрядного целого числа и указания того, какая его часть является десятичной дробью. Коэффициент масштабирования неявно равен числу 10, возведенному в степень в диапазоне от 0 до 28. Следовательно, двоичное представление Decimal имеет вид ((от -2^96 до 2^96) / 10^(от 0 до 28)), где -(2^96-1) равно минимальному значению, а 2^96-1 равно максимальному значению.

Коэффициент масштабирования также может сохранять любые конечные нули в Decimal. Эти конечные нули не влияют на значение в арифметических операциях или операциях сравнения. 

[Содержание](#содержание)

### Двоичное представление decimal_t

Двоичное представление decimal состоит из 1-разрядного знака, 96-разрядного целого числа и коэффициента масштабирования, используемого для деления целого числа и указания того, какая его часть является десятичной дробью. Коэффициент масштабирования неявно равен числу 10, возведенному в степень в диапазоне от 0 до 28.

decimal число может быть реализовано в виде четырехэлементного массива 32-разрядных целых чисел со знаком (`int bits[4];`).

`bits[0]`, `bits[1]`, и `bits[2]` содержат младшие, средние и старшие 32 бита 96-разрядного целого числа соответственно.

`bits[3]` содержит коэффициент масштабирования и знак, и состоит из следующих частей:
- Биты от 0 до 15, младшее слово, не используются и должны быть равны нулю.
- Биты с 16 по 23 должны содержать показатель степени от 0 до 28, который указывает степень 10 для разделения целого числа.
- Биты с 24 по 30 не используются и должны быть равны нулю.
- Бит 31 содержит знак; 0 означает положительный, а 1 означает отрицательный.

Обратите внимание, что битовое представление различает отрицательные и положительные нули. Эти значения могут считаться эквивалентными во всех операциях.

Пример:

```c
typedef struct 
{
    int bits[4];
} decimal_t;
```

## Функции
### Арифметические операторы

| Название оператора | Оператор  | Функция                                                                            | 
| ------ | ------ |------------------------------------------------------------------------------------|
| Сложение | + | int s21_add(s21_decimal value_1, s21_decimal value_2, s21_decimal *result)         |
| Вычитание | - | int s21_sub(s21_decimal value_1, s21_decimal value_2, s21_decimal *result)         |
| Умножение | * | int s21_mul(s21_decimal value_1, s21_decimal value_2, s21_decimal *result) | 
| Деление | / | int s21_div(s21_decimal value_1, s21_decimal value_2, s21_decimal *result) |

Функции возвращают код ошибки:  
- 0 - OK  
- 1 - число слишком велико или равно бесконечности
- 2 - число слишком мало или равно отрицательной бесконечности
- 3 - деление на 0

*Уточнение про числа, не вмещающиеся в мантиссу:*
- *При получении чисел, не вмещающихся в мантиссу при арифметических операциях, использовано банковское округление (например, 79,228,162,514,264,337,593,543,950,335 - 0.6 = 79,228,162,514,264,337,593,543,950,334)*

[Содержание](#содержание)

### Операторы сравнения

| Название оператора | Оператор  | Функция | 
| ------ | ------ | ------ |
| Меньше  | < | int s21_is_less(s21_decimal, s21_decimal) |
| Меньше или равно | <= | int s21_is_less_or_equal(s21_decimal, s21_decimal) | 
| Больше | \> |  int s21_is_greater(s21_decimal, s21_decimal) |
| Больше или равно | \>= | int s21_is_greater_or_equal(s21_decimal, s21_decimal) | 
| Равно | == |  int s21_is_equal(s21_decimal, s21_decimal) |
| Не равно | != |  int s21_is_not_equal(s21_decimal, s21_decimal) |

Возвращаемое значение:
- 0 - FALSE
- 1 - TRUE

[Содержание](#содержание)

### Преобразователи 

| Преобразователь | Функция | 
| ------ | ------ |
| Из int | int s21_from_int_to_decimal(int src, s21_decimal *dst) |
| Из float  | int s21_from_float_to_decimal(float src, s21_decimal *dst) |
| В int  | int s21_from_decimal_to_int(s21_decimal src, int *dst) |
| В float  | int s21_from_decimal_to_float(s21_decimal src, float *dst) |

Возвращаемое значение - код ошибки:
 - 0 - OK
 - 1 - ошибка конвертации

*Уточнение про преобразование числа типа float:*
- *Если числа слишком малы (0 < |x| < 1e-28), возвращается ошибка и значение, равное 0*
- *Если числа слишком велики (|x| > 79,228,162,514,264,337,593,543,950,335) или равны бесконечности, возвращается ошибка 1*
- *При обработке числа с типом float преобразовывать все содержащиеся в нём значимые десятичные цифры. Если таких цифр больше 7, то значение числа округляется к ближайшему, у которого не больше 7 значимых цифр.*

*Уточнение про преобразование из числа типа decimal в тип int:*
- *Если в числе типа decimal есть дробная часть, то она отбросывается (например, 0.9 преобразуется 0)*

[Содержание](#содержание)

### Другие функции

| Описание | Функция                                                  | 
| ------ |----------------------------------------------------------|
| Округляет указанное Decimal число до ближайшего целого числа в сторону отрицательной бесконечности. | int s21_floor(s21_decimal value, s21_decimal *result)    |	
| Округляет Decimal до ближайшего целого числа. | int s21_round(s21_decimal value, s21_decimal *result)    |
| Возвращает целые цифры указанного Decimal числа; любые дробные цифры отбрасываются, включая конечные нули. | int s21_truncate(s21_decimal value, s21_decimal *result) |
| Возвращает результат умножения указанного Decimal на -1. | int s21_negate(s21_decimal value, s21_decimal *result)   |

Возвращаемое значение - код ошибки:
 - 0 - OK
 - 1 - ошибка вычисления

[Содержание](#содержание)

## Лицензия

Авторские права (c) Москалев Илья и Габилов Первин. Все права защищены.

Лицензировано по [Лицензии MIT](./LICENSE).

[Содержание](#содержание)
