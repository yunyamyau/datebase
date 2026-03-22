<h1 name="content" align="center"><a href=""><img src="https://github.com/user-attachments/assets/e080adec-6af7-4bd2-b232-d43cb37024ac" width="20" height="20"/></a> MSSQL</h1>

<p align="center">
  <a href="#lab1"><img alt="lab1" src="https://img.shields.io/badge/Lab1-283e06"></a> 
  <a href="#lab2"><img alt="lab2" src="https://img.shields.io/badge/Lab2-31470b"></a> 
  <a href="#lab3"><img alt="lab3" src="https://img.shields.io/badge/Lab3-3b5110"></a> 
  <a href="#lab4"><img alt="lab4" src="https://img.shields.io/badge/Lab4-445a14"></a> 
  <a href="#lab5"><img alt="lab5" src="https://img.shields.io/badge/Lab5-586e14"></a> 
  <a href="#lab6"><img alt="lab6" src="https://img.shields.io/badge/Lab6-778c43"></a> 
  <a href="#lab7"><img alt="lab7" src="https://img.shields.io/badge/Lab7-96ac60"></a> 
</p>

# Лабараторная №1
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
# Лабараторная №2
[Назад](#content)


В соответствии с реляционной моделью данных, разработанной в Лаб.№1, создать реляционную БД на учебном сервере БД :
- создать таблицы, определить первичные ключи и иные ограничения
- определить связи между таблицами
- создать диаграмму
- заполнить все таблицы адекватной информацией (не меньше 10 записей в таблицах, наличие примеров для связей типа 1:M )


```
CREATE TABLE Дисциплина (
    ID INT PRIMARY KEY IDENTITY,
    Название NVARCHAR(50) NOT NULL
);

CREATE TABLE Специальность (
    ID INT PRIMARY KEY IDENTITY,
    Название NVARCHAR(50) NOT NULL
);

CREATE TABLE Тип_занятия (
    ID INT PRIMARY KEY IDENTITY,
    Название NVARCHAR(8) NOT NULL CHECK (Название IN ('Лекция','Практика'))
);

CREATE TABLE Тип_отчётности (
    ID INT PRIMARY KEY IDENTITY,
    Название NVARCHAR(7) NOT NULL CHECK (Название IN ('Зачёт','Экзамен'))
);

CREATE TABLE Преподаватель (
    ID INT PRIMARY KEY IDENTITY,
    ФИО NVARCHAR(150) NOT NULL,
    [Обязательные часы(лекции)] TINYINT NOT NULL,
    [Обязательные часы(практика)] TINYINT NOT NULL
);

CREATE TABLE Учебный_план (
    ID INT PRIMARY KEY IDENTITY,
    Курс TINYINT NOT NULL,
    Семестр TINYINT NOT NULL CHECK (Семестр IN (1,2)),
    [Учебный год] NVARCHAR(9) NOT NULL,
    [Часы лекций] TINYINT NOT NULL,
    [Часы практик] TINYINT NOT NULL,
    Специальность_ID INT NOT NULL FOREIGN KEY REFERENCES Специальность(ID),
    Дисциплина_ID INT NOT NULL FOREIGN KEY REFERENCES Дисциплина(ID),
    Тип_отчётности_ID INT NOT NULL FOREIGN KEY REFERENCES Тип_отчётности(ID)
);

CREATE TABLE Нагрузка (
    ID INT PRIMARY KEY IDENTITY,
    [Назначенные часы] TINYINT NOT NULL,
    Учебный_план_ID INT NOT NULL FOREIGN KEY REFERENCES Учебный_план(ID),
    Преподаватель_ID INT NOT NULL FOREIGN KEY REFERENCES Преподаватель(ID),
    Тип_занятия_ID INT NOT NULL FOREIGN KEY REFERENCES Тип_занятия(ID)
);

CREATE TABLE Дисциплина_Специальность (
    Дисциплина_ID INT NOT NULL FOREIGN KEY REFERENCES Дисциплина(ID),
    Специальность_ID INT NOT NULL FOREIGN KEY REFERENCES Специальность(ID),
    PRIMARY KEY (Дисциплина_ID, Специальность_ID)
);

CREATE TABLE Преподаватель_Дисциплина (
    Преподаватель_ID INT NOT NULL FOREIGN KEY REFERENCES Преподаватель(ID),
    Дисциплина_ID INT NOT NULL FOREIGN KEY REFERENCES Дисциплина(ID),
    PRIMARY KEY (Преподаватель_ID, Дисциплина_ID)
);
```

#### Диаграмма:
![Диаграмма](/pic/диаграмма.png)

#### Заполнение таблиц:
```
INSERT INTO Дисциплина (Название) VALUES
('Математический анализ'),
('Теория вероятности'), 
('История'),
('Основы Российской государственности'),
('Языки машинного программирования'),
('Основы информатики'),
('Комплексный анализ'),
('Физкультура'),
('Дифференциальные уравнения'),
('Физика');
```
![Дисциплина](/pic/Дисциплина.png)
```
INSERT INTO Специальность (Название) VALUES
('Прикладная математика и информатика'),
('Математика и компьютерные науки'),
('Фундаментальная математика и механика'),
('Прикладная математика и физика'),
('Бизнес-информатика'),
('Программная инженерия'),
('Математическое обеспечение'),
('Информатика и вычислительная техника'),
('Статистика и анализ данных'),
('Информационная безопасность');
```
![Специальность](/pic/Специальность.png)
```
INSERT INTO Преподаватель (ФИО, [Обязательные часы(лекции)], [Обязательные часы(практика)]) VALUES
('Иванов Александр Сергеевич', 120, 80),
('Петрова Мария Владимировна', 100, 100),
('Сидоров Дмитрий Иванович', 150, 60),
('Козлова Елена Петровна', 80, 120),
('Никитин Алексей Викторович', 130, 70),
('Фёдорова Ольга Дмитриевна', 90, 110),
('Громов Павел Сергеевич', 140, 50),
('Смирнова Анна Михайловна', 110, 90),
('Васильев Игорь Николаевич', 95, 105),
('Зайцева Татьяна Викторовна', 125, 75);
```
![Преподаватель](/pic/Преподаватель.png)
```
INSERT INTO Преподаватель_Дисциплина (Преподаватель_ID, Дисциплина_ID) VALUES
(1,1), (1,2), (1,7), (1,9),  
(2,5), (2,6), (2,8),
(3,10), (3,1),
(4,3), (4,4), 
(5,2), (5,9), (5,10),
(6,5), (6,6), (6,8),
(7,1), (7,7), (7,9),
(8,3), (8,4), (8,8),
(9,5), (9,6), (9,10),
(10,2), (10,7), (10,9);
```
![Преподаватель Дисциплина](/pic/Преподаватель%20дисциплина.png)
```
INSERT INTO Дисциплина_Специальность (Дисциплина_ID, Специальность_ID) VALUES
(1,1), (2,1), (5,1), (6,1), (7,1), (9,1), (10,1), 
(1,2), (2,2), (5,2), (6,2), (7,2), (9,2),
(1,3), (2,3), (7,3), (9,3), (10,3),
(1,4), (2,4), (7,4), (9,4), (10,4),
(2,5), (5,5), (6,5), (8,5),
(1,6), (2,6), (5,6), (6,6), (9,6),
(1,7), (2,7), (5,7), (6,7), (9,7),
(1,8), (2,8), (5,8), (6,8), (10,8),
(1,9), (2,9), (5,9), (6,9),
(1,10), (2,10), (5,10), (6,10), (9,10),
(3,1), (3,2), (3,3), (3,4), (3,5), (3,6), (3,7), (3,8), (3,9), (3,10),
(4,1), (4,2), (4,3), (4,4), (4,5), (4,6), (4,7), (4,8), (4,9), (4,10),
(8,1), (8,2), (8,3), (8,4), (8,6), (8,7), (8,8), (8,9), (8,10);
```
![Дисциплина Специальность](/pic/Дисциплина%20специальность.png)
```
INSERT INTO Учебный_план (Курс, Семестр, [Учебный год], [Часы лекций], [Часы практик], Специальность_ID, Дисциплина_ID, Тип_отчётности_ID) VALUES
(1, 1, '2023-2024', 60, 40, 1, 1, 2), 
(1, 1, '2023-2024', 50, 30, 1, 6, 2),
(1, 2, '2023-2024', 70, 35, 1, 2, 2),
(1, 1, '2023-2024', 40, 20, 2, 1, 2),
(1, 2, '2023-2024', 55, 25, 2, 5, 2),
(1, 1, '2023-2024', 65, 30, 3, 1, 2),
(1, 2, '2023-2024', 60, 30, 3, 10, 2),
(1, 1, '2023-2024', 45, 15, 4, 3, 1),
(1, 1, '2023-2024', 30, 30, 4, 8, 1), 
(1, 2, '2023-2024', 35, 15, 5, 4, 1);

INSERT INTO Тип_отчётности (Название) VALUES ('Зачёт'), ('Экзамен');
```
![Учебный план](/pic/Учебный%20план.png)
```
INSERT INTO Нагрузка ([Назначенные часы], Учебный_план_ID, Преподаватель_ID, Тип_занятия_ID) VALUES
(60, 1, 1, 1), (40, 1, 1, 2),
(50, 2, 2, 1), (30, 2, 2, 2),
(70, 3, 3, 1), (35, 3, 3, 2),
(55, 5, 5, 1), (25, 5, 5, 2),
(60, 7, 7, 1), (30, 7, 7, 2),
(45, 8, 4, 1), (15, 8, 4, 2),
(30, 9, 6, 1), (30, 9, 6, 2),
(35, 10, 8, 1), (15, 10, 8, 2);

INSERT INTO Тип_занятия (Название) VALUES ('Лекция'), ('Практика');
```
![Нагрузка](/pic/Нагрузка.png)



<a id="lab3"></a>
# Лабараторная №3
[Назад](#content)
> Цель: изучить конструкции языка SQL для манипулирования данными в СУБД  MSSQL.

📄 [Часть 1](./lab_3/3%20лаба%201%20часть.docx)
 
📄 [Часть 2](./lab_3/3%20лаба%202%20часть.docx)


<a id="lab4"></a>
# Лабараторная №4
[Назад](#content)




<a id="lab6"></a>
# Лабараторная №6
[Назад](#content)

![модель](/pic/dd6.png)

```

CREATE TABLE Graph_teacher (
    id INT PRIMARY KEY,
    ФИО NVARCHAR(150) NOT NULL,
    Обязательные_часы_лекции TINYINT NOT NULL,
    Обязательные_часы_практика TINYINT NOT NULL
) AS NODE;

CREATE TABLE Graph_discipline (
    id INT PRIMARY KEY,
    Название NVARCHAR(50) NOT NULL
) AS NODE;

CREATE TABLE Graph_specialization (
    id INT PRIMARY KEY,
    Название NVARCHAR(50) NOT NULL
) AS NODE;

CREATE TABLE Graph_load (
    id INT PRIMARY KEY,
    Назначенные_часы TINYINT NOT NULL
) AS NODE;

CREATE TABLE Graph_lesson_type (
    id INT PRIMARY KEY,
    Название NVARCHAR(8) NOT NULL CHECK (Название IN ('Лекция','Практика'))
) AS NODE;

CREATE TABLE Graph_study_plan (
    id INT PRIMARY KEY,
    Курс TINYINT NOT NULL,
    Семестр TINYINT NOT NULL CHECK (Семестр IN (1,2)),
    Учебный_год NVARCHAR(9) NOT NULL,
    Часы_лекций TINYINT NOT NULL,
    Часы_практик TINYINT NOT NULL
) AS NODE;

CREATE TABLE Graph_assessment_type (
    id INT PRIMARY KEY,
    Название NVARCHAR(7) NOT NULL CHECK (Название IN ('Зачёт','Экзамен'))
) AS NODE;


CREATE TABLE CAN_TEACH AS EDGE;
CREATE TABLE RELATED_TO_SPECIALTY AS EDGE;
CREATE TABLE ASSIGNED_TO_TEACHER AS EDGE;
CREATE TABLE CONTAINS_DISCIPLINE AS EDGE;
CREATE TABLE INCLUDES_SPECIALTY AS EDGE;
CREATE TABLE HAS_LESSON_TYPE AS EDGE;
CREATE TABLE HAS_ASSESSMENT_TYPE AS EDGE;
CREATE TABLE BELONGS_TO_STUDY_PLAN AS EDGE;


INSERT INTO Graph_teacher (id, ФИО, Обязательные_часы_лекции, Обязательные_часы_практика)
SELECT ID, ФИО, [Обязательные часы(лекции)], [Обязательные часы(практика)]
FROM Преподаватель;

INSERT INTO Graph_discipline (id, Название)
SELECT ID, Название
FROM Дисциплина;

INSERT INTO Graph_specialization (id, Название)
SELECT ID, Название
FROM Специальность;

INSERT INTO Graph_lesson_type (id, Название)
SELECT ID, Название
FROM Тип_занятия;

INSERT INTO Graph_assessment_type (id, Название)
SELECT ID, Название
FROM Тип_отчётности;

INSERT INTO Graph_study_plan (id, Курс, Семестр, Учебный_год, Часы_лекций, Часы_практик)
SELECT ID, Курс, Семестр, [Учебный год], [Часы лекций], [Часы практик]
FROM Учебный_план;

INSERT INTO Graph_load (id, Назначенные_часы)
SELECT ID, [Назначенные часы]
FROM Нагрузка;
GO


INSERT INTO CAN_TEACH ($from_id, $to_id)
SELECT t.$node_id, d.$node_id
FROM Graph_teacher t
JOIN Graph_discipline d ON EXISTS (
    SELECT 1 FROM Преподаватель_Дисциплина pd
    WHERE pd.Преподаватель_ID = t.id AND pd.Дисциплина_ID = d.id
);


INSERT INTO RELATED_TO_SPECIALTY ($from_id, $to_id)
SELECT d.$node_id, s.$node_id
FROM Graph_discipline d
JOIN Graph_specialization s ON EXISTS (
    SELECT 1 FROM Дисциплина_Специальность ds
    WHERE ds.Дисциплина_ID = d.id AND ds.Специальность_ID = s.id
);


INSERT INTO ASSIGNED_TO_TEACHER ($from_id, $to_id)
SELECT l.$node_id, t.$node_id
FROM Graph_load l
JOIN Graph_teacher t ON EXISTS (
    SELECT 1 FROM Нагрузка n
    WHERE n.ID = l.id AND n.Преподаватель_ID = t.id
);


INSERT INTO CONTAINS_DISCIPLINE ($from_id, $to_id)
SELECT l.$node_id, d.$node_id
FROM Graph_load l
JOIN Graph_discipline d ON EXISTS (
    SELECT 1 FROM Нагрузка n
    JOIN Учебный_план up ON n.Учебный_план_ID = up.ID
    WHERE n.ID = l.id AND up.Дисциплина_ID = d.id
);


INSERT INTO INCLUDES_SPECIALTY ($from_id, $to_id)
SELECT sp.$node_id, s.$node_id
FROM Graph_study_plan sp
JOIN Graph_specialization s ON EXISTS (
    SELECT 1 FROM Учебный_план up
    WHERE up.ID = sp.id AND up.Специальность_ID = s.id
);


INSERT INTO HAS_LESSON_TYPE ($from_id, $to_id)
SELECT l.$node_id, lt.$node_id
FROM Graph_load l
JOIN Graph_lesson_type lt ON EXISTS (
    SELECT 1 FROM Нагрузка n
    WHERE n.ID = l.id AND n.Тип_занятия_ID = lt.id
);


INSERT INTO HAS_ASSESSMENT_TYPE ($from_id, $to_id)
SELECT sp.$node_id, at.$node_id
FROM Graph_study_plan sp
JOIN Graph_assessment_type at ON EXISTS (
    SELECT 1 FROM Учебный_план up
    WHERE up.ID = sp.id AND up.Тип_отчётности_ID = at.id
);


INSERT INTO BELONGS_TO_STUDY_PLAN ($from_id, $to_id)
SELECT l.$node_id, sp.$node_id
FROM Graph_load l
JOIN Graph_study_plan sp ON EXISTS (
    SELECT 1 FROM Нагрузка n
    WHERE n.ID = l.id AND n.Учебный_план_ID = sp.id
);
GO
```

```
-- Для каждой дисциплины вывести преподавателей, которые могут ее вести
SELECT
    d.Название AS Дисциплина,
    STRING_AGG(t.ФИО, ', ') AS Преподаватели
FROM 
    Graph_discipline d,
    Graph_teacher t,
    CAN_TEACH ct
WHERE MATCH(t-(ct)->d)
GROUP BY d.Название
ORDER BY d.Название
```
![модель](/pic/Снимок%20экрана%202026-01-12%20064832.png)

```
-- Найти преподавателей с максимальной нагрузкой по лекциям
SELECT TOP 1 WITH TIES
    t.ФИО,
    SUM(l.Назначенные_часы) AS Общее_число_часов_лекций
FROM 
    Graph_teacher t,
    Graph_load l,
    Graph_lesson_type lt,
    ASSIGNED_TO_TEACHER,
    HAS_LESSON_TYPE
WHERE MATCH(t<-(ASSIGNED_TO_TEACHER)-l-(HAS_LESSON_TYPE)->lt)
    AND lt.Название = 'Лекция'
GROUP BY t.ФИО
ORDER BY SUM(l.Назначенные_часы) DESC
```
![модель](/pic/Снимок%20экрана%202026-01-12%20064711.png)

```
-- Для каждой дисциплины и каждой специальности вывести количество лет
SELECT
    s.Название AS Специальность,
    d.Название AS Дисциплина,
    COUNT(DISTINCT sp.Учебный_год) * 0.5 AS Продолжительность_в_годах
FROM 
    Graph_discipline d,
    Graph_specialization s,
    Graph_study_plan sp,
    RELATED_TO_SPECIALTY,
    INCLUDES_SPECIALTY
WHERE MATCH(d-(RELATED_TO_SPECIALTY)->s<-(INCLUDES_SPECIALTY)-sp)
GROUP BY s.Название, d.Название
ORDER BY s.Название, d.Название
```
![модель](/pic/Снимок%20экрана%202026-01-12%20064737.png)
![модель](/pic/Снимок%20экрана%202026-01-12%20064747.png)

```
-- Найти дисциплины, по которым есть лекции, но нет практики
SELECT DISTINCT
    s.Название AS Специальность,
    sp.Курс,
    d.Название AS Дисциплина
FROM 
    Graph_discipline d,
    Graph_specialization s,
    Graph_study_plan sp,
    RELATED_TO_SPECIALTY,
    INCLUDES_SPECIALTY
WHERE MATCH(d-(RELATED_TO_SPECIALTY)->s<-(INCLUDES_SPECIALTY)-sp)
    AND sp.Часы_лекций > 0 
    AND sp.Часы_практик = 0
ORDER BY s.Название, sp.Курс, d.Название
```
![модель](/pic/Снимок%20экрана%202026-01-12%20064825.png)
![модель](/pic/Снимок%20экрана%202026-01-12%20064832.png)

```
--  Для каждого преподавателя выдать дисциплины (лекции и практику отдельно)
SELECT
    t.ФИО,
    ISNULL(STRING_AGG(CASE WHEN sp.Часы_лекций > 0 THEN d.Название END, ', '), '') AS Лекции,
    ISNULL(STRING_AGG(CASE WHEN sp.Часы_практик > 0 THEN d.Название END, ', '), '') AS Практика
FROM 
    Graph_teacher t,
    Graph_discipline d,
    Graph_specialization s,
    Graph_study_plan sp,
    CAN_TEACH,
    RELATED_TO_SPECIALTY,
    INCLUDES_SPECIALTY
WHERE MATCH(t-(CAN_TEACH)->d-(RELATED_TO_SPECIALTY)->s<-(INCLUDES_SPECIALTY)-sp)
GROUP BY t.ФИО
ORDER BY t.ФИО
```
![модель](/pic/Снимок%20экрана%202026-01-12%20071630.png)



<a id="lab7"></a>
# Лабараторная №7
[Назад](#content)

Задание 1
Используя базу, полученную в лабораторной 2, создать транзакцию, произвести ее откат и фиксацию. Показать, что данные существовали до отката, удалились после отката, снова были добавлены, и затем были успешно зафиксированы. При необходимости используйте точки сохранения и вложенные транзакции.
```

SELECT 
    * 
FROM 
    Нагрузка n
    JOIN Преподаватель p ON n.Преподаватель_ID = p.ID
WHERE 
    p.ФИО = 'Смирнова Анна Михайловна'


-- Начало транзакции
BEGIN TRANSACTION;

DECLARE @PrepID INT = (SELECT ID FROM Преподаватель WHERE ФИО = 'Смирнова Анна Михайловна');
DECLARE @PlanID INT = 1;

---- Добавляем нагрузку (лекции) - 120 часов (превышает лимит 110)
--INSERT INTO Нагрузка ([Назначенные часы], Учебный_план_ID, Преподаватель_ID, Тип_занятия_ID)
--VALUES (120, @PlanID, @PrepID, 1);

-- Добавляем нагрузку (лекции) - 20 часов
INSERT INTO Нагрузка ([Назначенные часы], Учебный_план_ID, Преподаватель_ID, Тип_занятия_ID)
VALUES (20, @PlanID, @PrepID, 1);

DECLARE @LoadID INT = SCOPE_IDENTITY();

SELECT * FROM Нагрузка WHERE ID = @LoadID;

-- Сумму лекций
DECLARE @TotalLectures INT;
SELECT 
    @TotalLectures = SUM([Назначенные часы]) 
FROM 
    Нагрузка 
WHERE 
    Преподаватель_ID = @PrepID AND Тип_занятия_ID = 1;

DECLARE @RequiredLectures TINYINT = (SELECT [Обязательные часы(лекции)] FROM Преподаватель WHERE ID = @PrepID);

IF @TotalLectures > @RequiredLectures
BEGIN
    PRINT 'Превышение обязательных часов! Выполняется откат.';
    ROLLBACK TRANSACTION ;    
END
ELSE
BEGIN
    COMMIT TRANSACTION;
END



SELECT 
    * 
FROM 
    Нагрузка n
    JOIN Преподаватель p ON n.Преподаватель_ID = p.ID
WHERE 
    p.ФИО = 'Смирнова Анна Михайловна'

```
![модель](/pic/Снимок%20экрана%202026-03-22%20181658.png)
![модель](/pic/Снимок%20экрана%202026-03-22%201302.png)
Задание 2
Подготовить SQL-скрипты для выполнения проверок изолированности транзакций. Ваши скрипты должны работать с одной из таблиц, созданных в лабораторной работе №2.


