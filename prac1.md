# Практическое занятие №1. Введение, основы работы в командной строке

## Задача 1

Вывести отсортированный в алфавитном порядке список имен пользователей в файле passwd (вам понадобится grep).

## Решение

```bash
cat /etc/passwd | grep -oE '^[^:]+'
```
<img width="278" alt="image" src="https://github.com/user-attachments/assets/8a377b2c-34bb-4fed-a5c9-fc37eb91ca61">

---
## Задача 2

Вывести данные /etc/protocols в отформатированном и отсортированном порядке для 5 наибольших портов, как показано в примере ниже:

```
[root@localhost etc]# cat /etc/protocols ...
142 rohc
141 wesp
140 shim6
139 hip
138 manet
```

## Решение

```bash
tac /etc/protocols | head -5 | awk '{print $2, $1}'
```
<img width="348" alt="image" src="https://github.com/user-attachments/assets/2696ef94-a3f9-489a-9cf8-55e68d6492b4">

---
## Задача 3

Написать программу banner средствами bash для вывода текстов, как в следующем примере (размер баннера должен меняться!):

```
[root@localhost ~]# ./banner "Hello from RTU MIREA!"
+-----------------------+
| Hello from RTU MIREA! |
+-----------------------+
```

Перед отправкой решения проверьте его в ShellCheck на предупреждения.

## Решение

```bash
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
```
<img width="277" alt="image" src="https://github.com/user-attachments/assets/bd024794-f876-461b-8a31-0ce048a3d01e">

---
## Задача 4

Написать программу для вывода всех идентификаторов (по правилам C/C++ или Java) в файле (без повторений).

Пример для hello.c:

```
h hello include int main n printf return stdio void world
```

## Решение

```bash
grep -Eo '[_A-Za-z][_A-Za-z0-9]{0,30}' hello.c | sort | xargs
```
![img4](img/Pasted%20image%2020240908185233.png)
---
## Задача 5

Написать программу для регистрации пользовательской команды (правильные права доступа и копирование в /usr/local/bin).

Например, пусть программа называется reg:

В результате для banner задаются правильные права доступа и сам banner копируется в /usr/local/bin.

## Решение

```bash
#!/bin/bash
chmod +x "$1"
sudo cp "$1" /usr/local/bin
```
<img width="264" alt="image" src="https://github.com/user-attachments/assets/de5c31e4-abbc-4296-bb21-49e82a7f16b5">

---
## Задача 6

Написать программу для проверки наличия комментария в первой строке файлов с расширением c, js и py.

## Решение

```bash
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
```
<img width="294" alt="image" src="https://github.com/user-attachments/assets/dcc1e32a-ccb7-4fcb-b9ba-1348038bedc4">

---
## Задача 7

Написать программу для нахождения файлов-дубликатов (имеющих 1 или более копий содержимого) по заданному пути (и подкаталогам).

## Решение

```bash
#!/bin/bash
find "$1" -type f -printf "%p\n" | xargs md5sum | sort | uniq -w32 --all-repeated=separate
```
<img width="341" alt="image" src="https://github.com/user-attachments/assets/215e7cf0-857d-4fad-9f5c-c7412fcae37c">

---
## Задача 8

Написать программу, которая находит все файлы в данном каталоге с расширением, указанным в качестве аргумента и архивирует все эти файлы в архив tar.

## Решение

```bash
#!/bin/bash
find . -name "*.$1" -exec tar -rvf archive.tar {} \;
```
<img width="424" alt="image" src="https://github.com/user-attachments/assets/a9f8131a-5512-4554-b85b-3e7daeb25bc1">

---
## Задача 9

Написать программу, которая заменяет в файле последовательности из 4 пробелов на символ табуляции. Входной и выходной файлы задаются аргументами.

## Решение

```bash
#!/bin/bash
sed 's/    /    /g' "$1" > "$2"
```
<img width="270" alt="image" src="https://github.com/user-attachments/assets/2b0d436b-3b18-43af-ac04-62be5e2d6c02">

---
## Задача 10

Написать программу, которая выводит названия всех пустых текстовых файлов в указанной директории. Директория передается в программу параметром. 

## Решение

```bash
#!/bin/bash
find "$1" -type f -empty -name "*.txt"
```
<img width="232" alt="image" src="https://github.com/user-attachments/assets/8d0540bf-3895-4c2a-b01c-9475b43b17b4">

