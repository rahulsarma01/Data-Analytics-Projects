# 🕵🏻‍♂️ SQL Murder Mystery: Can you find out whodunnit?

![174092-clue-illustration](https://user-images.githubusercontent.com/113444489/229263292-d3a10dda-3c84-491d-a0f9-cc3445f7398a.png)

## 📚 Table of Contents
- [Case Task](#case-task)
- [Entity Relationship Diagram](#entity-relationship-diagram)
- [Questions and Solutions](#questions-and-solutions)

***
## ⚒️ Case Task

As a data professional, you've been hired by the detective assigned on the case to help solve a murder case that took place in SQL City. The detective has provided you with some initial information but need your help to uncover more clues. The murder occurred sometime on Jan.15, 2018.

## Entity Relationship Diagram

![schema](https://user-images.githubusercontent.com/113444489/229263670-392c15c1-daee-4c86-a0ba-914883c772f2.png)

## 🙋🏻‍♂️ Questions and Solutions

- Start by retrieving the corresponding crime scene report from the police department’s database.
    
    ```sql
    SELECT description FROM crime_scene_report
    WHERE date = '20180115' AND type = 'murder' AND city = 'SQL City'
    ```
    ![Screenshot (13)](https://user-images.githubusercontent.com/113444489/229264046-95d1e212-7793-4501-8a50-f5405aa0b64a.png)

There were 2 witnesses. The first witness lives at the last house on "Northwestern Dr". The second witness, named Annabel, lives somewhere on "Franklin Ave".

---

- Look for the witnesses

  ```sql
    -- The criteria is divided into two parts and stored in two CTE(Common Table Expression)s called witness_1 and witness_2 respectively.
    WITH witness_1 AS (
    SELECT id FROM person
    WHERE address_street_name = 'Northwestern Dr'
    ORDER BY address_number DESC LIMIT 1
    ), witness_2 AS (
    SELECT id FROM person
    WHERE INSTR(name, 'Annabel') > 0 AND address_street_name = 'Franklin Ave'


    -- Two CTEs are combined into a single table called witnesses using the UNION operator 
    ), witnesses AS (
    SELECT *, 1 AS witness FROM witness_1
    UNION
    SELECT *, 2 AS witness FROM witness_2
    )

    -- The witnesses table is then joined with the interview table using the person_id column to retrieve the transcripts of the interviews.
    SELECT witness, transcript FROM witnesses
    LEFT JOIN interview ON witnesses.id = interview.person_id
    ```
    ![Screenshot (14)](https://user-images.githubusercontent.com/113444489/229265082-b4872a90-6e2a-41f2-986c-3d4682958efa.png)

---

- Cross-reference the gym memberships, gym check-ins, and drivers license

  ```sql
  WITH gym_checkins AS (
    
    SELECT 
           person_id, 
           name
    FROM 
       get_fit_now_member
    LEFT JOIN get_fit_now_check_in 
    ON get_fit_now_member.id = get_fit_now_check_in.membership_id
    WHERE membership_status = 'gold'   -- Only gold members have those bags(witness_1)
      AND id LIKE '48Z%'               -- membership number on the bag started with "48Z"(witness_1)
      AND check_in_date = '20180109'   -- Witness_2 recognized him on January the 9th
  
   ), suspects AS (
    SELECT 
      gym_checkins.person_id, 
      gym_checkins.name, 
      plate_number, 
      gender
    FROM 
      gym_checkins
    LEFT JOIN person ON gym_checkins.person_id = person.id
    LEFT JOIN drivers_license ON person.license_id = drivers_license.id
  )
  SELECT * 
  FROM suspects
  WHERE INSTR(plate_number, 'H42W') > 0 AND gender = 'male' --  The man got into a car with a plate that included "H42W" (witness_1)
  ```


![Screenshot (15)](https://user-images.githubusercontent.com/113444489/229265539-7b31a8ca-6427-487c-9f77-ab7726ebbc40.png)

We got him! Let's match the answer:


![Screenshot (16)](https://user-images.githubusercontent.com/113444489/229265650-09d8f162-b0e2-4010-a5bf-f40999e3dfe1.png)

---

## Further Investigation

- Access transcript of the murderer

  ```sql
    SELECT 
      transcript 
    FROM 
      interview 
    WHERE person_id = 67318
    ```
  ![Screenshot (17)](https://user-images.githubusercontent.com/113444489/229265868-2d8ec79e-93e0-4ed1-9386-a206f79c60d8.png)

---

## Cross-reference the clues

  ```sql
    WITH red_haired_tesla_drivers AS (
    
    SELECT id AS license_id
    FROM drivers_license
    WHERE gender = 'female' AND hair_color = 'red'      -- She has red hair
      AND car_make = 'Tesla' AND car_model = 'Model S'  -- and she drives a Tesla Model S
      AND height >= 64 AND height <= 68                 -- she's around 5'5" (65") or 5'7" (67")
    
    ), rich_suspects AS (
    
    SELECT person.id AS person_id, name, annual_income
    FROM red_haired_tesla_drivers AS rhtd
    LEFT JOIN person ON rhtd.license_id = person.license_id
    LEFT JOIN income ON person.ssn = income.ssn
    
    ), symphony_attenders AS (
    
    SELECT person_id, COUNT(1) AS no_checkins
    FROM facebook_event_checkin
    WHERE event_name = 'SQL Symphony Concert' -- she attended the SQL Symphony Concert
      AND `date` IS IN '201712'               -- event date: December 2017
    GROUP BY person_id
    HAVING no_checkins = 3                    -- checked in 3 times
    )
   
    SELECT name, annual_income
    FROM rich_suspects
    INNER JOIN symphony_attenders ON rich_suspects.person_id = symphony_attenders.person_id
  ```
    







| name | annual_income |
| --- | --- |
| Miranda Priestly | 310000 |


 We got our real villain behind this crime. Let's match our answer:
 
 ![Screenshot (19)](https://user-images.githubusercontent.com/113444489/229266490-e98e48f0-a5b3-4dff-a519-3b894173a153.png)

 
 ---
 
 *Thank you for taking the time to read my analysis. If you enjoyed it, please give a star ⭐*
 
 
 
 
 
 
 


