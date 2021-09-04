# Data Normalization and Entity-Relationship Diagramming

An assignment to normalize the structure of data and establish a set of Entity-Relationship Diagrams for the data.


# Final Report 

## 1. Original Table 

| assignment_id | student_id | due_date | professor | assignment_topic                | classroom | grade | relevant_reading    | professor_email   |
| :------------ | :--------- | :------- | :-------- | :------------------------------ | :-------- | :---- | :------------------ | :---------------- |
| 1             | 1          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 80    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 2             | 7          | 18.11.21 | Logston   | Single table queries            | 60FA 314  | 25    | Dümmlers Chapter 11 | e.logston@foo.edu |
| 1             | 4          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 75    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 5             | 2          | 05.05.21 | Logston   | Python and pandas               | 60FA 314  | 92    | Dümmlers Chapter 14 | e.logston@foo.edu |
| 4             | 2          | 04.07.21 | Nevarez   | Spreadsheet aggregate functions | WWH 201   | 65    | Zehnder Page 87     | i.nevarez@foo.edu |
| ...           | ...        | ...      | ...       | ...                             | ...       | ...   | ...                 | ...               |

## 2. Original Table not in 4NF

Lets assume that assignment_id and student_id are composite keys of this table. 
* Second normal form is violated when a non-key field is a fact about a subset of a key. This fact is broken for the table above because "assignment_topic" is a fact only about "assignment_id" and not "student_id". 
* Third normal form is violated when a non-key field is a fact about another non-key field. This fact is broken for the table above because "professor_email" is about "professor". Also, 3NF cannot be satisfied if 2NF isn't.
* Under fourth normal form, a record type should not contain two or more independent multi-valued facts about an entity. This fact is broken for the table above because we have "classroom" and "assignment_topic", which are two independent multi-valued facts. Also, 4NF is violated because 3NF has not been satisfied. 


## 3. 4NF Compliant Tables

### Student Table 
Assuming that a given student can only be in one section of a course. 
Primary Key -> student_id

| student_id | section_id | 
|------------|------------| 
| 1          | 1          | 
| 2          | 1          | 
| 3          | 4          | 
| 4          | 2          | 
| 5          | 3          |
| ...        | ...        |

### Proffesor Table 
Primary Key -> prof_id
| prof_id | prof_email       | prof_name | 
|---------|------------------|-----------| 
| 1       | melvin@foo.edu   | Melvin    | 
| 2       | logston@foo.edu  | Longston  | 
| 3       | nevarez@foo.edu  | Nevarez   |
| ...     | ...              | ...       |  

### Section Table 
Assuming that a section is uniquely identified using a professor and class_room.
Primary Key -> section_id
| section_id | class_room      | prof_id   | 
|------------|-----------------|-----------| 
| 1          | WWH 101         | 1         |
| 2          | 60FA 314        | 2         | 
| 3          | WWH 201         | 3         | 
|...         |...              |...        |

### Assignment Table
Primary Key -> assignment_id
| assignment_id | assignment_topic                | relevant_reading    | due_date | prof_id |
|---------------|---------------------------------|---------------------|----------|---------| 
| 1             | Data normalization              | Deumlich Chapter 3  | 23.02.21 | 1       |
| 2             | Single table queries            | Deumlich Chapter 11 | 18.11.21 | 2       |
| 3             | Qauntumn Data                   | Deumlich Chapter 12 | 22.06.21 | 5       |
| 4             | Spreadsheet aggregate functions | Zehnder Page 87     | 04.07.21 | 3       |
| 5             | Python and pandas               | Dümmlers Chapter 14 | 05.05.21 | 2       |
| ...           | ...                             | ...                 | ...      | ...     |

### Grade Table
Primary Key -> grade_id
| grade_id | assignment_id | student_id | grade |
|----------|---------------|------------|-------|
| 1        | 1             | 1          | 80    |
| 2        | 2             | 7          | 25    |
| 3        | 1             | 4          | 75    |
| 4        | 5             | 2          | 92    |
| 5        | 4             | 2          | 65    |
|...       |...            |...         |...    |


## 4. ER Diagram

![ER Diagram](https://github.com/database-design-assignments/database-normalization-entity-relationship-diagramming-tazasahar/blob/master/images/DDI_WORKSHOP%20(1).svg)

## 5. 4NF Compliant

1. Student Table 
    * 2NF compliant because each student_id is the unique attribute of a student entity and section_id describes a fact about the student_id only. This is assuming that a student can only attend one section.
    * 3NF compliant because section_id, non-key, describes only student_id, primary key.
    * 4NF compliant because it does not contain more than one independent multi-valued fact about the    entity student. 

2. Professor Table
    * 2NF compliant because each prof_id is the unique attribute of a single professor. Each non-key fields provides a fact about the proffessor. 
    * 3NF compliant because prof_name and prof_email is not a fact about another non-key field. 
    * 4NF compliant because it does not contain more than one independent multi-valued fact about the    entity professor (prof_email is related to prof_name).

3. Section Table
    * 2NF compliant because each section_if is the unique attribute of a single section and class_room and prof_id is only about the section. 
    * 3NF compliant because prof_id and class_room is not a fact about other non-key fields. It is only a fact about the section_id (primary key)
    * 4NF complient because no more than one independent multi-valued fact exists about the entity section. As such, class_room and prof_id are needed to distinguish the section. 

4. Assignment Table
    * 2NF compliant because each assignment_id is the unique code of a single assignment and all of non-key fields descibes a fact about the key field only.
    * 3NF compliant because no non-key field in our table is about other non-key fields. 
    * 4NF compliant because there is no more than one multi-valued facts about the entity assignment. All non-key fields are dependent to each other. 

5. Grade Table 
    * 2NF compliant because each grade_id is the unique code of a single grade comprising of the assignment_id, student_id and grade. As such all non-key fields are strictly about the key field. 
    * 3NF compliant because assignment_id, student_id, and grade are all about grade_id (key). 
    * 4NF compliant becuase assignment_if, student_id, and grade are all dependent to each other to produce a unique record. Also, there is only on independent multi-valued fact about the grade entity. 