# Домашнее задание к занятию "Процессы, управление процессами" - Иевлева Анастасия

### Задание 1

Рассмотрим загрузку данных и многопоточность. В описанных ниже ситуациях поможет ли использование нескольких потоков для скачивания уменьшить время общей загрузки?

1. 100 файлов на разных Web-серверах, суммарным объёмом 10 Гбайт, через подключение со скоростью 1Мбит\с;
2. 100 файлов на разных Web-серверах, суммарным объёмом 10 Гбайт, через подключение со скоростью 10 Гбит\с;
3. 1 файл объёмом 10 Гбайт находящийся в торрентах;
4. 1 файл объёмом 10 Гбайт находящийся на FTP-сервере;
5. 10 файлов объёмом по 1 Гб находящихся в общей папке компьютера секретаря.

*Приведите ответ для каждого случай в свободной форме (лучше использовать один поток скачивания, несколько, всё равно) со своим комментарием.*

**Ответ:**

1. Несколько потоков  (т.к. скорость слишком низкая);
2. Один поток (т.к. скорость высокая);
3. Несколько потоков (особенность торрента - чем больше потоков, тем выше скорость);
4. Один поток (т.к. при загрузке с FTP-сервера нельзя разбить 1 файл на несколько потоков);
5. Один поток (т.к. вывод с диска)

---

**Задание 1.1**

Попробуйте высказать предположение о **количестве потоков** скачивания, для случаев приведенных выше, если загрузка данных происходит на следующие системы:

- Компьютер Windows 10 64-bit\ i5-xxxx \16 Gb\ 2 TB HDD
- Компьютер Windows 10 32-bit\ i7-xxxx\ 8 Gb\ 2 TB HDD
- Ноутбук Windows 10 64-bit\ i7-xxxx\ 32 Gb\ 500 GB HDD
- Ноутбук Windows 10 64-bit\ i7-xxxx\ 32 Gb\ 2 TB HDD

- Компьютер Windows 8.1 32-bit\ i3-xxxx\ 8 Gb\ 1 TB SSD

- Компьютер Windows 10 64-bit\ i3-xxxx\ 8 Gb\ 1 TB HDD (RAID)

*Необязательно рассматривать все возможные комбинации, достаточно описать своими словами отличия.*

**Примечания:**

1) другими запущенными процессами на компьютерах можно пренебречь;

2) производительность CPU: i7 > i5 > i3.

**Ответ:**

Думаю, в основном стоит ориентироваться на линейку процессора, так как процессоры Intel Core i3, как правило, имеют 2 ядра, i5 обычно имеют 4 ядра и 4 потока, что означает, что они способны обрабатывать 4 независимых задачи одновременно. Процессоры  i7 имеют 4 или более физических ядра, которые способны обрабатывать 8 или более потоков, что позволяет им выполнять больше задач одновременно. 

**Задание 1.2**

Какой из приведенных выше компьютеров постоянно "тормозит" и почему?

**Ответ:**

Тормозит последний компьютер, так как у него слабый процессор и мало оперативной памяти для такой ОС.

---

### Задание 2

Объясните, что делает команда:

`ps -aux | grep root | wc -l  >> root`

*Ответ напишите в свободной форме.*

**Примечание:**

Если вы встречаете неизвестную команду Linux, либо неизвестные параметры команды, то можете вызвать встроенную помощь:
`man <команда>`

Например:

- man ps;
- man grep;
- man wc.

**Ответ:**

Команда выводит количество всех запущенных процессов пользователя root в файл “root” (данные записываются в конец файла, его содержимое не удаляется).

---

### Задание 3

Напишите команду, которая выводит все запущенные процессы пользователя root в файл *"user_root_ps"*.

**Ответ:**

ps aux | grep root >> user_root_ps

---

### Задание 4

Начинающий администратор захотел вывести все запущенные процессы пользователя с логином "2" в файл *"user_2_ps"*.

Для этого он набрал команду:

`ps -U 2> user_2_ps`

Затем, он аналогично повторил для пользователя с логином "5" вывод в файл "user_5_ps":

`ps -U 5> user_5_ps`

**Вопрос:** 

Почему вывод этих команд и содержимое файлов сильно отличаются друг от друга?  Как должны выглядеть правильные команды?

**Примечание:**

Если у вас в системе нет пользователей "2" и/или "5" (это нормальная ситуация), то утилита ps выводит только одну строку:

`  PID TTY          TIME CMD      `

**Ответ:**

Здесь допущена ошибка - пропущен пробел между логином пользователя и >. 

Из-за этого в первом случае синтаксис ‘2>’ был воспринят как второй поток (т.е. вывод ошибок в файл). Сама команда ничего не выдала, а в файл user_2_ps записались ошибки. 

Во втором случае ошибки были выведены на экран, поскольку пятого потока нет и команда неверная. В файл user_5_ps ничего записано не было.

Правильные команды:

- ps -U 2 > user_2_ps
- ps -U 5 > user_5_ps
