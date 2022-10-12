Queries:

-- How many people answered the survey per year.

SELECT SurveyID, COUNT(DISTINCT userID)
FROM Answer
GROUP BY surveyID;


-- Finding out how many answers each question has and how many questions the survey has.

SELECT q.questionid, q.questiontext, COUNT(a.answertext)
FROM Question q
JOIN Answer a
	ON q.questionid = a.questionid
GROUP BY q.questiontext
ORDER BY 1;

-- Check for Null values

SELECT COUNT(*)
FROM Answer
WHERE AnswerText IS NULL;

SELECT COUNT(*)
FROM Answer
WHERE UserID IS NULL;

SELECT COUNT(*)
FROM Answer
WHERE questionid IS NULL;

SELECT count(*)
FROM question WHERE questionid IS NULL


SELECT count(*)
FROM question WHERE questiontext IS NULL

-- Create a new table with only the questions we want to explore

CREATE TABLE new_answers AS
	SELECT *
	FROM Answer 
	WHERE questionid IN (1,2,7,8,10,18,33,96,114,118)


-- Question 1: general statistics:

CREATE TABLE age AS
SELECT a.AnswerText AS age
	FROM Question q
	JOIN Answer a
		ON q.questionid = a.questionid
	WHERE q.questionid = 1;
  

SELECT CASE
	WHEN age < 20 THEN "0-20"
	WHEN age BETWEEN 20 AND 29 THEN "20-29"
	WHEN age BETWEEN 30 AND 39 THEN "30-39"
	WHEN age BETWEEN 40 AND 49 THEN "40-49"
	WHEN age BETWEEN 50 AND 59 THEN "50-59"
	ELSE "60+"
	END AS age_group, COUNT(*)
FROM age
GROUP BY 1;
	

SELECT AVG(age)
FROM age;

-- Question 2: How many people have a mental health disorder?

UPDATE new_answers
SET AnswerText = "Maybe"
WHERE questionid = 33
AND AnswerText NOT IN ("Yes","No")

SELECT DISTINCT AnswerText AS "Have mental health disorder", COUNT(*) AS Count
FROM new_answers
WHERE questionid = 33
GROUP BY AnswerText
ORDER BY 2 DESC;

-- Question 3: Average age for people who seek treatment

WITH temp_table AS 
	(SELECT *
	FROM new_answers
	JOIN age
		ON new_answers.UserID = age.UserID)
		
SELECT CASE
	WHEN AnswerText = 0 THEN "NO"
	WHEN AnswerText = 1 THEN "YES"
	END AS "Seek treatment"
	, ROUND(AVG(age),0) AS "Average Age"
FROM temp_table
WHERE questionid = 7
GROUP BY AnswerText;

-- Question 4: Does gender affect likelihood to seek treatment

CREATE TABLE gender AS
	SELECT userID, AnswerText AS gender
	FROM new_answers
	WHERE questionid = 2;

UPDATE gender
SET gender = lower(gender);

UPDATE gender
SET gender = "other" 
WHERE gender NOT IN ("male", "female");

SELECT gender, COUNT(gender) AS count
FROM gender
GROUP BY gender;

	
WITH temp_table AS 
	(SELECT *
	FROM new_answers
	JOIN gender
		ON new_answers.UserID = gender.UserID)

SELECT gender, CASE
	WHEN AnswerText = 0 THEN "NO"
	WHEN AnswerText = 1 THEN "YES"
	END AS "Seek treatment", COUNT(answertext) AS count
FROM temp_table
WHERE questionid = 7
GROUP BY 1,2
		

-- Question 5: Do companies provide benefits/ learning resources?

UPDATE new_answers
SET AnswerText = "Not Sure"
WHERE AnswerText NOT IN ("Yes", "No")
AND questionid = 10

SELECT AnswerText AS "Provide_benifits", COUNT(*) AS Count
FROM new_answers
WHERE questionid = 10
GROUP BY 1;


UPDATE new_answers
SET AnswerText = "Not Sure"
WHERE AnswerText = "Don't know"
AND questionid = 96

SELECT AnswerText AS "Provide_resources", COUNT(*) AS Count
FROM new_answers
WHERE questionid = 96
GROUP BY 1;

--Question 6: Do people who work remotely have less mental disorders?

WITH temp_table AS
	(SELECT UserID, AnswerText AS "remote_work"
	FROM new_answers
	WHERE questionid = 118)
	
SELECT AnswerText AS "Mental_health_disorder",remote_work, COUNT(*)
FROM new_answers
JOIN temp_table
	ON new_answers.UserID = temp_table.UserID
WHERE Questionid = 33
GROUP BY remote_work,Mental_health_disorder 
