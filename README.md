<h1 name="content" align="center"><a href=""><img src="https://github.com/user-attachments/assets/e080adec-6af7-4bd2-b232-d43cb37024ac" width="20" height="20"/></a> MSSQL</h1>

<p align="center">
  <a href="#lab1"><img alt="lab1" src="https://img.shields.io/badge/Lab1-6B8E23"></a> 
  <a href="#lab2"><img alt="lab2" src="https://img.shields.io/badge/Lab2-6B8E23"></a> 
</p>

<a id="lab1"></a>
# <img src="https://github.com/user-attachments/assets/e080adec-6af7-4bd2-b232-d43cb37024ac" width="20" height="20"/> Лабараторная №1
[Назад](#content)
<h3 align="center">
  <a href="#client"></a>
  Автоматизация распределения учебной нагрузки. Вариант 18.
</h3>

Разработать ER-модель данной предметной области: выделить сущности, их атрибуты, связи между сущностями. 
Для каждой сущности указать ее имя, атрибут (или набор атрибутов), являющийся первичным ключом, список остальных атрибутов.
Для каждого атрибута указать его тип, ограничения, может ли он быть пустым, является ли он первичным ключом.
Для каждой связи между сущностями указать: 
- тип связи (1:1, 1:M, M:N)
- обязательность
ER-модель д.б. представлена в виде ER-диаграммы (картинка)
По имеющейся ER-модели создать реляционную модель данных и отобразить ее в виде списка сущностей с их атрибутами и типами атрибутов,  для атрибутов указать, явл. ли он первичным или внешним ключом

Информация о преподавателях: ФИО, список дисциплин, которые он может вести, обязательное кол-во часов нагрузки (отдельно лекции и практика).
Учебный план (составляется на текущий учебный год): дисциплина, специальность, курс, семестр, кол-во часов (отдельно лекции и практика), вид отчетности (зачет, экзамен), преподаватель.
Реализовать:
- Вывод нагрузки конкретного преподавателя;
- Вывод учебной нагрузки для специальности (дисциплина, кол-во часов, вид (лекция /практика), преподаватель).
- Подчсет кол-ва часов нагрузки преподавателя, поиск самого «загруженного преподавателя»
- Вывод списка экзаменов и зачетов в ближайшую сессию для специальности на конкретном курсе

#### ER модель:
![ER модель](/pic/ER.png)


#### Реляционная модель:
![REL модель](/pic/REL.png)

<a id="lab2"></a>
# <img src="https://github.com/user-attachments/assets/e080adec-6af7-4bd2-b232-d43cb37024ac" width="20" height="20"/> Лабараторная №2
[Назад](#content)


В соответствии с реляционной моделью данных, разработанной в Лаб.№1, создать реляционную БД на учебном сервере БД :
- создать таблицы, определить первичные ключи и иные ограничения
- определить связи между таблицами
- создать диаграмму
- заполнить все таблицы адекватной информацией (не меньше 10 записей в таблицах, наличие примеров для связей типа 1:M )


```
CREATE TABLE Дисциплина (
    Дисциплина_ID INT PRIMARY KEY IDENTITY(1,1),
    Название NVARCHAR(50) NOT NULL
);

CREATE TABLE Специальность (
    Специальность_ID INT PRIMARY KEY IDENTITY(1,1),
    Название NVARCHAR(50) NOT NULL
);

CREATE TABLE Тип_занятия (
    Тип_занятия_ID INT PRIMARY KEY IDENTITY(1,1),
    Название NVARCHAR(8) NOT NULL CHECK (Название IN ('Лекция','Практика'))
);

CREATE TABLE Тип_отчётности (
    Тип_отчётности_ID INT PRIMARY KEY IDENTITY(1,1),
    Название NVARCHAR(7) NOT NULL CHECK (Название IN ('Зачёт','Экзамен'))
);

CREATE TABLE Преподаватель (
    Преподаватель_ID INT PRIMARY KEY IDENTITY(1,1),
    ФИО NVARCHAR(150) NOT NULL,
    Обязательные_часы_лекции INT NOT NULL,
    Обязательные_часы_практика INT NOT NULL
);

CREATE TABLE Учебный_план (
    Учебный_план_ID INT PRIMARY KEY IDENTITY(1,1),
    Курс INT NOT NULL,
    Семестр INT NOT NULL CHECK (Семестр IN (1,2)), 
    Учебный_год NVARCHAR(9) NOT NULL,
    Часы_лекций INT NOT NULL,
    Часы_практик INT NOT NULL,
    Специальность_ID INT NOT NULL FOREIGN KEY REFERENCES Специальность(Специальность_ID),
    Дисциплина_ID INT NOT NULL FOREIGN KEY REFERENCES Дисциплина(Дисциплина_ID),
    Тип_отчётности_ID INT NOT NULL FOREIGN KEY REFERENCES Тип_отчётности(Тип_отчётности_ID)
);

CREATE TABLE Нагрузка (
    Нагрузка_ID INT PRIMARY KEY IDENTITY(1,1),
    Назначенные_часы INT NOT NULL,
    Учебный_план_ID INT NOT NULL FOREIGN KEY REFERENCES Учебный_план(Учебный_план_ID),
    Преподаватель_ID INT NOT NULL FOREIGN KEY REFERENCES Преподаватель(Преподаватель_ID),
    Тип_занятия_ID INT NOT NULL FOREIGN KEY REFERENCES Тип_занятия(Тип_занятия_ID)
);

CREATE TABLE Дисциплина_Специальность (
    Дисциплина_ID INT NOT NULL FOREIGN KEY REFERENCES Дисциплина(Дисциплина_ID),
    Специальность_ID INT NOT NULL FOREIGN KEY REFERENCES Специальность(Специальность_ID),
    PRIMARY KEY (Дисциплина_ID, Специальность_ID)
);

CREATE TABLE Преподаватель_Дисциплина (
    Преподаватель_ID INT NOT NULL FOREIGN KEY REFERENCES Преподаватель(Преподаватель_ID),
    Дисциплина_ID INT NOT NULL FOREIGN KEY REFERENCES Дисциплина(Дисциплина_ID),
    PRIMARY KEY (Преподаватель_ID, Дисциплина_ID)
);

```
