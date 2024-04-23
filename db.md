Modellizzare la struttura di un database per memorizzare tutti i dati riguardanti una università:
- sono presenti diversi Dipartimenti (es.: Lettere e Filosofia, Matematica, Ingegneria ecc.);
- ogni Dipartimento offre più Corsi di Laurea (es.: Civiltà e Letterature Classiche, Informatica, Ingegneria Elettronica ecc..)
- ogni Corso di Laurea prevede diversi Corsi (es.: Letteratura Latina, Sistemi Operativi 1, Analisi Matematica 2 ecc.);
- ogni Corso può essere tenuto da diversi Insegnanti;
- ogni Corso prevede più appelli d'Esame;
- ogni Studente è iscritto ad un solo Corso di Laurea;
- ogni Studente può iscriversi a più appelli di Esame;
- per ogni appello d'Esame a cui lo Studente ha partecipato, è necessario memorizzare il voto ottenuto, anche se non sufficiente.
Pensiamo a quali entità (tabelle) creare per il nostro database e cerchiamo poi di stabilirne le relazioni. Infine, andiamo a definire le colonne e i tipi di dato di ogni tabella.



<!-- ogni studente può avere più appelli di esame e un solo corso di laurea -->
table name: students

- id | BIGINT | INDEX | AI | PK | UNIQUE | NOTNULL
- degree_courses_id | FK
- name | VARCHAR(20) | NOTNULL
- lastname | VARCHAR(20) | NOTNULL
- serial_number | INDEX | VARCHAR(20) | NOTNULL | UNIQUE
- email | VARCHAR(255) | NOTNULL


<!-- in ogni dipartimento ci sono più corsi di laurea -->
table name: departments

- id | BIGINT | INDEX | AI | PK | UNIQUE | NOTNULL
- name | VARCHAR(50) | NOTNULL
- location | VARCHAR(20) | NULL
- contact | VARCHAR(20) | NULL



<!-- in ogni corso di laurea ci sono più corsi -->
table name: degree_courses

- id | BIGINT | INDEX | AI | PK | UNIQUE | NOTNULL
- deparments_id | FK 
- name | VARCHAR(50) | NOTNULL



<!-- in ogni corso ci sono più studenti, più appelli di esami e più insegnanti -->
table name: courses

- id | BIGINT | INDEX | AI | PK | UNIQUE | NOTNULL
- degree_courses_id | FK
- name | VARCHAR(50) | NOTNULL
- cfu | TINYINT | NULL



<!-- ogni teacher ha più corsi -->
table name: teachers

- id | BIGINT | INDEX | AI | PK | UNIQUE | NOTNULL
- name | VARCHAR(20) | NOTNULL
- lastname | VARCHAR(20) | NOTNULL
- email | VARCHAR(255) | NOTNULL
- serial_number | VARCHAR(20) | INDEX | UNIQUE | NOTNULL 


table name: exam_calls

- id | BIGINT | INDEX | AI | PK | UNIQUE | NOTNULL
- courses_id | FK 
- data | DATETIME | NOTNULL
- type | VARCHAR(15) | NULL



table name: exam_calls_students

- id | BIGINT | INDEX | AI | PK | UNIQUE | NOTNULL
- exam_calls_id
- students_id
- vote | TINYINT | NOTNULL