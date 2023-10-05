
# SQL QUERY PRACTICE

The aim of this repository is to help learners and professionals who want to improve and grow their SQL knowledge for their careers. I thought (https://www.sql-practice.com) the website was user-friendly and informative, but neither GitHub nor the website released the entire solution book. As a result, you may now find answers to each problem and alternative options.


VIEW DATABASE SCHEMA:

![image](https://github.com/anjalikankoriya/SQL_Practice/assets/111731531/d27adf8b-355f-42db-9f67-71bde88f6ff7)

The test contains 52 questions and consists of three categories:

- Easy
- Medium
- Hard

You can use the following entity relationship diagram (ERD) to figure out what data to store, the entities, their attributes, and how entities relate to other entities.


## Section: Easy

1. Show first name, last name, and gender of patients whose gender is 'M'

```bash
SELECT first_name, last_name, gender FROM patients where gender = 'M';
```
2. Show first name and last name of patients who does not have allergies. (null).

```bash
SELECT first_name, last_name FROM patients where allergies is null;
```
3. Show first name of patients that start with the letter 'C'

```bash
SELECT first_name FROM patients where first_name like 'C%';
```
4. Show first name and last name of patients that weight within the range of 100 to 120 (inclusive).

```bash
SELECT first_name, last_name FROM patients where weight between 100 and 120 ;
```
5. Update the patients table for the allergies column. If the patient's allergies is null then replace it with 'NKA'.

```bash
 update patients set allergies = 'NKA' where allergies is null;
```
6. Show first name and last name concatinated into one column to show their full name.

```bash
select concat(first_name,' ', last_name) as Full_NAME from patients;
```
7. Show first name, last name, and the full province name of each patient.
Example: 'Ontario' instead of 'ON'

```bash
select first_name, last_name, province_name from patients as p Inner join 
province_names as PN on p.province_id = PN.province_id;
```
8. Show how many patients have a birth_date with 2010 as the birth year.

```bash
select COUNT(patients.birth_date) from patients
where year(birth_date) = 2010;
```
9. Show the first_name, last_name, and height of the patient with the greatest height.

```bash
select first_name, last_name, MAX(height) from patients;
```
10. Show all columns for patients who have one of the following patient_ids: 1,45,534,879,1000.

```bash
select * from patients where patient_id in (1,45,534,879,1000);
```
11. Show the total number of admissions.

```bash
select count(patient_id) from admissions;
```
12. Show all the columns from admissions where the patient was admitted and discharged on the same day.

```bash
select * from admissions where admission_date = discharge_date;
```
13. Show the patient id and the total number of admissions for patient_id 579.

```bash
select patient_id, count(admission_date) as Total_admission from admissions where patient_id = 579;
```
14. Based on the cities that our patients live in, show unique cities that are in province_id 'NS'?

```bash
select distinct(city) from patients where province_id = 'NS';
```
15. Write a query to find the first_name, last name and birth date of patients who has height greater than 160 and weight greater than 70.

```bash
select first_name, last_name, birth_date from patients where height > 160
and weight > 70;
```
16. Write a query to find list of patients first_name, last_name, and allergies from Hamilton where allergies are not null.

```bash 
SELECT first_name, last_name, allergies FROM patients where
city = 'Hamilton' and allergies is not null;
```
17. Based on cities where our patient lives in, write a query to display the list of unique city starting with a vowel (a, e, i, o, u). Show the result order in ascending by city.

```bash
select distinct(city) from patients where city like 'A%'
or city like 'E%' 
or city like 'I%'
or city like 'O%'
or city like 'U%'
or city like 'a%'
or city like 'e%'
or city like 'i%'
or city like 'o%'
or city like 'u%'
 order by city asc;
 ```

__________________________________________________________________________________________________
## Section: Medium

1. Show unique birth years from patients and order them by ascending.

```bash
select distinct year(birth_date)as years from patients 
order by years asc;
```
2. Show unique first names from the patients table which only occurs once in the list. For example, if two or more people are named 'John' in the first_name column then don't include their name in the output list. If only 1 person is named 'Leo' then include them in the output.

```bash
select first_name from patients group by first_name having 
count(first_name = 'John')=1;
```
3. Show patient_id and first_name from patients where their first_name start and ends with 's' and is at least 6 characters long.

```bash
select patient_id, first_name from patients where first_name
like 'S%____%S';
```
4. Show patient_id, first_name, last_name from patients whos diagnosis is 'Dementia'. Primary diagnosis is stored in the admissions table.

```bash
select P.patient_id, P.first_name, P.last_name from patients as P
join admissions as A on P.patient_id = A.patient_id  where 
diagnosis = 'Dementia';
```
5. Display every patient's first_name. Order the list by the length of each name and then by alphbetically.

```bash
select first_name from patients order by length(first_name), first_name;
```
6. Show the total amount of male patients and the total amount of female patients in the patients table. Display the two results in the same row.

```bash
select(select count(*) from patients where gender = 'M') as Male,
(select count(*) from patients where gender = 'F') as Female;
```
```bash 
SELECT 
  SUM(Gender = 'M') as male_count, 
  SUM(Gender = 'F') AS female_count
FROM patients;
```
7. Show first and last name, allergies from patients which have allergies to either 'Penicillin' or 'Morphine'. Show results ordered ascending by allergies then by first_name then by last_name.

```bash
select first_name, last_name, allergies from patients where 
allergies in ('Penicillin','Morphine') order by allergies, 
first_name, last_name asc;
```
8. Show patient_id, diagnosis from admissions. Find patients admitted multiple times for the same diagnosis.

```bash
select patient_id, diagnosis from admissions 
group by patient_id, diagnosis having
count(diagnosis= diagnosis) > 1;
```
9. Show the city and the total number of patients in the city. Order from most to least patients and then by city name ascending.

```bash
select city, count(*)as total_patient from patients group by city
order by total_patient desc, city asc;
```
10. Show first name, last name and role of every person that is either patient or doctor. The roles are either "Patient" or "Doctor".

```bash
select first_name, last_name, 'Patient' as role from patients
union All 
select first_name, last_name, 'Doctor' as role from doctors;
```
11. Show all allergies ordered by popularity. Remove NULL values from query.

```bash 
select allergies, count(*) as total_allergies from patients
where allergies is not null group by allergies
order by total_allergies desc;
```
12. Show all patient's first_name, last_name, and birth_date who were born in the 1970s decade. Sort the list starting from the earliest birth_date.

```bash
select first_name, last_name, birth_date from patients where 
year(birth_date) between 1970 and 1979 order by birth_date asc;
```
13. We want to display each patient's full name in a single column. Their last_name in all upper letters must appear first, then first_name in all lower case letters. Separate the last_name and first_name with a comma. Order the list by the first_name in decending order
EX: SMITH,jane.

```bash
SELECT concat(upper(last_name),',', lower(first_name)) as full_Name
FROM patients 
order by first_name desc;
```
14. Show the province_id(s), sum of height; where the total sum of its patient's height is greater than or equal to 7,000.

```bash
SELECT pr.province_id, SUM(pa.height) AS sum_height
FROM province_names pr
         JOIN patients pa ON pr.province_id = pa.province_id
GROUP BY pr.province_id
HAVING SUM(pa.height) >= 7000;
```
15. Show the difference between the largest weight and smallest weight for patients with the last name 'Maroni'

```bash
select (max(weight)-min(weight)) as weight_difference from patients where last_name = 'Maroni';
```
16. Show all of the days of the month (1-31) and how many admission_dates occurred on that day. Sort by the day with most admissions to least admissions.

```bash
select day(admission_date) as day_number, count(patient_id) as patitent_occured from 
admissions group by day_number order by patitent_occured desc;
```
17. Show all columns for patient_id 542's most recent admission_date.

```bash
select * from admissions where patient_id = 542 order by admission_date desc limit 1;
```
18. Show patient_id, attending_doctor_id, and diagnosis for admissions that match one of the two criteria:
- patient_id is an odd number and attending_doctor_id is either 1, 5, or 19.
- attending_doctor_id contains a 2 and the length of patient_id is 3 characters.

```bash
select  patient_id, attending_doctor_id,diagnosis from admissions where patient_id % 2 = 1 and 
attending_doctor_id in (1, 5,19) or (attending_doctor_id like '%2%' and length(patient_id) =3);
```
19. Show first_name, last_name, and the total number of admissions attended for each doctor. Every admission has been attended by a doctor.

```bash
select first_name, last_name, count(attending_doctor_id) as total_admission from admissions join 
doctors on doctors.doctor_id = admissions.attending_doctor_id group by first_name, last_name;
```
20. For each doctor, display their id, full name, and the first and last admission date they attended.

```bash
select d.doctor_id, concat(d.first_name,' ',d.last_name)as full_name, min(a.admission_date) as Last_attend,
max(a.admission_date)as First_attend from doctors as d 
join admissions as a on d.doctor_id = a.attending_doctor_id group by d.doctor_id, full_name;
```
21. Display the total amount of patients for each province. Order by descending.

```bash
select count(patient_id) as patient_count, province_name 
from province_names join patients on 
province_names.province_id = patients.province_id
group by province_name order by patient_count desc ;
```
22. For every admission, display the patient's full name, their admission diagnosis, and their doctor's full name who diagnosed their problem.

```bash
select concat(p.first_name,' ',p.last_name)as Patient_Name, a.diagnosis, 
concat(d.first_name,' ',d.last_name)as Doctor_Name from patients as p 
join admissions as a on p.patient_id = a.patient_id 
join doctors as d on d.doctor_id = a.attending_doctor_id;
```
23. display the number of duplicate patients based on their first_name and last_name.

```bash
select first_name, last_name, count(*) as duplicate_patient from patients 
group by first_name, last_name having count(*) >1;
```
24. Display patient's full name, height in the units feet rounded to 1 decimal, weight in the unit pounds rounded to 0 decimals, birth_date, gender non abbreviated.
- Convert CM to feet by dividing by 30.48.
- Convert KG to pounds by multiplying by 2.205.

```bash
select concat(first_name,' ',last_name) as Patient_Name, round(height/30.48,1) as Height,
round(weight*2.205,0) as Weight, birth_date,
case
   when gender = 'M' 
   then 'Male'
   else 'Female' 
end as Gender
from patients;
```
25. Show patient_id, first_name, last_name from patients whose does not have any records in the admissions table. (Their patient_id does not exist in any admissions.patient_id rows.)

```bash
select p.patient_id, first_name, last_name from patients as p
left join admissions on p.patient_id = admissions.patient_id
where admissions.patient_id is null;
```
