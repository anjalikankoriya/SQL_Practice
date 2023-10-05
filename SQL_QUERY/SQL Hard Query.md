
## Section: Hard

1. Show all of the patients grouped into weight groups. Show the total amount of patients in each weight group. Order the list by the weight group descending.
For example, if they weight 100 to 109, they are placed in the 100 weight group, 110-119 = 110 weight group, etc.

```bash
SELECT (weight / 10) * 10 AS weight_group, COUNT(*) AS patient_weight_group
FROM patients
GROUP BY weight_group
ORDER BY weight_group DESC;
```
2. Show patient_id, weight, height, isObese from the patients table. Display isObese as a boolean 0 or 1.
Obese is defined as weight(kg)/(height(m)2) >= 30.weight is in units kg and height is in units cm.
Show patient_id, weight, height, isObese from the patients table.

```bash
SELECT patient_id,
       weight,
       height,
       CASE
           WHEN weight / POWER(height / 100.00,2)>= 30
               THEN 1
           ELSE 0
           END AS isobese
FROM patients;
```
3. Show patient_id, first_name, last_name, and attending doctor's specialty.
Show only the patients who has a diagnosis as 'Epilepsy' and the doctor's first name is 'Lisa'.

Check patients, admissions, and doctors tables for required information.
```bash
select p.patient_id, p.first_name, p.last_name, d.specialty from patients as p
join admissions as a on  p.patient_id = a.patient_id 
join doctors as d on d.doctor_id = a.attending_doctor_id
where a.diagnosis = 'Epilepsy' and d.first_name = 'Lisa';
```
4. All patients who have gone through admissions, can see their medical documents on our site. Those patients are given a temporary password after their first admission. Show the patient_id and temp_password.

The password must be the following, in order:
- patient_id
- the numerical length of patient's last_name
- year of patient's birth_date.

```bash
select distinct(p.patient_id), Concat(p.patient_id,length(p.last_name),Year(p.birth_date)) as Temp_password from patients as p
join admissions as a on p.patient_id = a.patient_id;
```
5. Each admission costs $50 for patients without insurance, and $10 for patients with insurance. All patients with an even patient_id have insurance.

Give each patient a 'Yes' if they have insurance, and a 'No' if they don't have insurance. Add up the admission_total cost for each has_insurance group.

```bash
select 
case when patient_id % 2 = 0 then 'Yes'
else 'No'
end has_insurance, sum(case when patient_id % 2 =0 then 10
                      else 50 
                      end) as admission_total_cost
                      from admissions
                      group by has_insurance;
```
6. Show the provinces that has more patients identified as 'M' than 'F'. Must only show full province_name.

```bash
SELECT pr.province_name
FROM patients AS pa
  JOIN province_names AS pr ON pa.province_id = pr.province_id
GROUP BY pr.province_name
HAVING
  COUNT( CASE WHEN gender = 'M' THEN 1 END) > COUNT( CASE WHEN gender = 'F' THEN 1 END);
```
7. We are looking for a specific patient. Pull all columns for the patient who matches the following criteria:
- First_name contains an 'r' after the first two letters.
- Identifies their gender as 'F'
- Born in February, May, or December
- Their weight would be between 60kg and 80kg
- Their patient_id is an odd number
- They are from the city 'Kingston'

```bash
select * from patients where 
first_name like '__r%'and (patient_id % 2) = 1 and gender = 'F' and month(birth_date) 
in (2,5,12) and 
weight between 60 and 80 
and city = 'Kingston';
```
8. Show the percent of patients that have 'M' as their gender. Round the answer to the nearest hundreth number and in percent form.

```bash
select round(100* avg(gender = 'M'),2) || '%' as precentage_of_male from patients;
```
9. For each day display the total amount of admissions on that day. Display the amount changed from the previous date.

```bash
select admission_date,count(admission_date) as admission_day, count(admission_date)- lag(count(admission_date)) over 
(order by admission_date) as admission_count from admissions group by admission_date;
```
10. Sort the province names in ascending order in such a way that the province 'Ontario' is always on top.

```bash
select province_name from  province_names order by province_name = 'Ontario' desc, province_name; 
```
11. We need a breakdown for the total amount of admissions each doctor has started each year. Show the doctor_id, doctor_full_name, specialty, year, total_admissions for that year.

```bash
select d.doctor_id as doctor_id, concat(d.first_name, ' ', d.last_name) as Full_name, d.specialty, 
year(a.admission_date) as selected_year, count(*)as total_admission 
from doctors as d 
Left join admissions as a on d.doctor_id = a.attending_doctor_id 
group by Full_name, selected_year
order by doctor_id, selected_year;
```
