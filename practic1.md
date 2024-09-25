Практическое занятие №1. Введение, основы работы в командной строке
Задача 1
Вывести отсортированный в алфавитном порядке список имен пользователей в файле passwd (вам понадобится grep).

Решение
cat /etc/passwd | grep -oE '^[^:]+'
img1
Задача 2
Вывести данные /etc/protocols в отформатированном и отсортированном порядке для 5 наибольших портов, как показано в примере ниже:

[root@localhost etc]# cat /etc/protocols ...
142 rohc
141 wesp
140 shim6
139 hip
138 manet
Решение
tac /etc/protocols | head -5 | awk '{print $2, $1}'
img2
Задача 3
Написать программу banner средствами bash для вывода текстов, как в следующем примере (размер баннера должен меняться!):

[root@localhost ~]# ./banner "Hello from RTU MIREA!"
+-----------------------+
| Hello from RTU MIREA! |
+-----------------------+
Перед отправкой решения проверьте его в ShellCheck на предупреждения.

Решение
#!/bin/bash
arg1=$1
size=${#arg1}
echo -n "+"
for ((i = -2; i < size; ++i)); do
    echo -n "-"
done
echo "+"
echo "| $arg1 |"
echo -n "+"
for ((i = -2; i < size; ++i)); do
    echo -n "-"
done
echo "+"
img3
Задача 4
Написать программу для вывода всех идентификаторов (по правилам C/C++ или Java) в файле (без повторений).

Пример для hello.c:

h hello include int main n printf return stdio void world
Решение
grep -Eo '[_A-Za-z][_A-Za-z0-9]{0,30}' hello.c | sort | xargs
img4
Задача 5
Написать программу для регистрации пользовательской команды (правильные права доступа и копирование в /usr/local/bin).

Например, пусть программа называется reg:

В результате для banner задаются правильные права доступа и сам banner копируется в /usr/local/bin.

Решение
#!/bin/bash
chmod +x "$1"
sudo cp "$1" /usr/local/bin
img5
Задача 6
Написать программу для проверки наличия комментария в первой строке файлов с расширением c, js и py.

Решение
#!/bin/bash
for filename in "$@";
do
        if [[ "$filename" =~ .*(c|js)$ ]]; then
                if [[ "$(head -1 "$filename")" =~ ^(\/\*|\/\/) ]]; then
                        echo "comment!"
                else
                        echo "comment not found!"
                fi
        elif [[ "$filename" =~ .*py$ ]]; then
                if [[ "$(head -1 "$filename")" =~ ^# ]]; then
                        echo "comment!"
                else
                        echo "comment not found!"
                fi
        fi
done
img6
Задача 7
Написать программу для нахождения файлов-дубликатов (имеющих 1 или более копий содержимого) по заданному пути (и подкаталогам).

Решение
#!/bin/bash
find "$1" -type f -printf "%p\n" | xargs md5sum | sort | uniq -w32 --all-repeated=separate
img7
Задача 8
Написать программу, которая находит все файлы в данном каталоге с расширением, указанным в качестве аргумента и архивирует все эти файлы в архив tar.

Решение
#!/bin/bash
find . -name "*.$1" -exec tar -rvf archive.tar {} \;
img8
Задача 9
Написать программу, которая заменяет в файле последовательности из 4 пробелов на символ табуляции. Входной и выходной файлы задаются аргументами.

Решение
#!/bin/bash
sed 's/    /    /g' "$1" > "$2"
img9
Задача 10
Написать программу, которая выводит названия всех пустых текстовых файлов в указанной директории. Директория передается в программу параметром.

Решение
#!/bin/bash
find "$1" -type f -empty -name "*.txt"
img10
