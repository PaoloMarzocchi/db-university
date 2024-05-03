## Group by

- Contare quanti iscritti ci sono stati ogni anno 

    SELECT COUNT(*) AS `enrolled_students`, YEAR(`enrolment_date`) AS 'year'
    FROM `students`
    GROUP BY year(`enrolment_date`);

- Contare gli insegnanti che hanno l'ufficio nello stesso edificio

    SELECT COUNT(*) AS `teachers`, `office_address`
    FROM `teachers`
    GROUP BY `office_address`;

- Calcolare la media dei voti di ogni appello d'esame

    SELECT ROUND(AVG(`vote`),2) AS `average_grades`, `exam_id`
    FROM `exam_student`
    GROUP BY `exam_id`;

- Contare quanti corsi di laurea ci sono per ogni dipartimento

    SELECT COUNT(*) AS `degrees_number`, `department_id` 
    FROM `degrees` 
    GROUP BY `department_id`;

## Joins

- Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

    1.  <!-- Studenti iscritti solo al corso di laurea in economia -->
        SELECT `students`.`*`,`degrees`.`name`
        FROM `students`
        JOIN `degrees` ON `degrees`.`id` = `students`.`degree_id`
        WHERE `degrees`.`name` = 'Corso di Laurea in Economia';

    2. <!-- Studenti iscritti a tutti i corsi di economia -->
        SELECT `students`.`*`,`degrees`.`name`
        FROM `students`
        JOIN `degrees` ON `degrees`.`id` = `students`.`degree_id`
        WHERE `degrees`.`name` LIKE('%economia%');

- Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze

    SELECT `degrees`.`*`, `departments`.`name`
    FROM `degrees`
    JOIN `departments` ON `departments`.`id` = `degrees`.`department_id`
    WHERE `degrees`.`level` = 'magistrale'
    AND `departments`.`name` = 'Dipartimento di Neuroscienze';

- Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

    SELECT `courses`.`*`, `teachers`.`name`,`teachers`.`surname`,`teachers`.`email`
    FROM `courses`
    JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id`
    JOIN `teachers` ON `teachers`.`id` = `course_teacher`.`teacher_id`
    WHERE `course_teacher`.`teacher_id` = 44;

- Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo 
  dipartimento, in ordine alfabetico per cognome e nome

    SELECT `students`.`surname`,`students`.`name`,`degrees`.`name` AS `degree_name`,`degrees`.`level`,`degrees`.`address`,`degrees`.`website`,`departments`.`name` AS `department_name`
    FROM `students`
    JOIN `degrees` ON `degrees`.`id` = `students`.`degree_id`
    JOIN `departments` ON `departments`.`id` = `degrees`.`department_id`
    ORDER BY `students`.`surname`,`students`.`name`;

- Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

    SELECT `degrees`.`*`,`courses`.`name` AS `course_name`,`courses`.`description` AS `course_description`,`teachers`.`name` AS `teacher_name`,`teachers`.`surname` AS `teacher_surname`
    FROM `degrees`
    JOIN `courses` ON `degrees`.`id` = `courses`.`degree_id`
    JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id`
    JOIN `teachers` ON `teachers`.`id` = `course_teacher`.`teacher_id`;

- Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)

    SELECT `teachers`.`*`,`departments`.`name` AS `department_name`
    FROM `teachers`
    JOIN `course_teacher` ON `teachers`.`id` = `course_teacher`.`teacher_id`
    JOIN `courses` ON `courses`.`id` = `course_teacher`.`course_id`
    JOIN `degrees` ON `degrees`.`id` = `courses`.`degree_id`
    JOIN `departments` ON `departments`.`id` = `degrees`.`department_id`
    WHERE `departments`.`name` = 'Dipartimento di Matematica'

BONUS: 

- Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente,  filtrare i tentativi con voto minimo 18.

    SELECT `exam_student`.`exam_id`,`exam_student`.`student_id`,`students`.`name` AS `student_name`,`students`.`surname` AS `student_surname`,COUNT(`exam_student`.`student_id`) AS `number_attempts`,MAX(`exam_student`.`vote`) AS `max_vote`
    FROM `students`
    JOIN `exam_student` ON `students`.`id` = `exam_student`.`student_id`
    JOIN `exams` ON `exams`.`id` = `exam_student`.`exam_id`
    GROUP BY `exam_student`.`exam_id`,`exam_student`.`student_id`
    HAVING `max_vote` >= 18;