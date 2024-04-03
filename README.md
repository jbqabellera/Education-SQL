
# Teacher Education in the Philippines

## Problem
The Philippines faces an education crisis, particularly the unbalanced teacher-to-student ratio. Let's examine the number of teachers for the academic year 2021-2022.

## Data

The data includes number of teachers according to teaching level - Junior Highschool and Senior Highschool Teachers. There is information on region and different teaching positions. 

## SQL

### Junior Highschool Teachers

1. Classify the regions of JHS teachers into three main island groups in the Philippines - Luzon, Visayas, and Mindanao.

```
Select reg,
Case
	When reg IN ('Region I', 'Region II', 'Region III', 'Region IV-A', 'Region IV-B',
	'NCR', 'CAR', 'Region V')THEN 'Luzon'
	When reg IN ('Region VI', 'Region VII', 'Region VIII') THEN 'Visayas'
	ELSE 'Mindanao'
End as region_group
From jhs
Where reg <> 'Grand Total'
```

2. Calculate the proportion of JHS teachers with Instructor III status over total JHS teachers

```
Select reg, teacher_i,(teacher_i/total)*100 as percent_teacher_i
From jhs
order by 3 desc


Select reg, teacher_ii,(teacher_ii/total)*100 as percent_teacher_ii
From jhs
order by 3 desc

Select reg, teacher_iii,(teacher_iii/total)*100 as percent_teacher_iii
From jhs
order by 3 desc
```

3. Which region suffers from lack of JHS teachers?

```
Select reg, MIN(cast(total as int)) as fewest_teacher
From jhs
Where reg <> 'Grand Total'
Group by reg
Order by 2 
```

4. What is the average number of JHS teachers in the country?

```
Select reg, AVG(cast(total as int)) as average_teachers
From jhs
Where reg <> 'Grand Total'
Group by reg
Order by 2 
```

5. Which region has the highest number of JHS teachers?

```
Select reg, MAX(cast(total as int)) as highest_teacher
From jhs
Where reg <> 'Grand Total'
Group by reg
Order by 2 desc
```

6. Which region has less than 10,000 teachers?

```
Select reg, MAX(cast(total as int)) as highest_teacher
From jhs
Where reg <> 'Grand Total'
Group by reg
Having MAX(cast(total as int))<10000
Order by 2
```

7. Which region has the highest number of special science teachers?

```
Select reg, MAX(ssteacher_i) as highest_ssteacher
From jhs
Where reg <> 'Grand Total'
Group by reg
Order by 2 desc
```

### Junior and Senior Highschool Teachers

8. What is the average number of JHS and shs teachers?
```
Select jhs.reg
, MAX(jhs.total + shs.total) as total_teachers
, AVG(jhs.total) as avg_jhs_teachers, AVG(shs.total) as avg_shs_teachers
From jhs
Join shs
	On jhs.reg = shs.reg
Where jhs.reg <> 'Grand Total'
Group by jhs.reg
Order by jhs_and_shs_teachers desc
```

9. Which region has the highest number of JHS and SHS teachers?

```
Select jhs.reg, SUM(jhs.total + shs.total) as jhs_and_shs_teachers
From jhs
Join shs
	On jhs.reg = shs.reg
Where jhs.reg <> 'Grand Total'
Group by jhs.reg
Order by jhs_and_shs_teachers desc
