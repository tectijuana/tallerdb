# **Corza Morales Ian Kurt 22211544**

## 1- Change the query shown so that it displays Nobel prizes for 1950.
**SELECT yr, subject, winner
  FROM nobel
 WHERE yr = 1950**
![Screenshot 2025-02-07 140703](https://github.com/user-attachments/assets/0e41f801-b8be-4c8e-bb96-df1cab48f46e)

## 2- Show who won the 1962 prize for literature.
**SELECT winner
  FROM nobel
 WHERE yr = 1962
   AND subject = 'literature'**
   ![Screenshot 2025-02-07 140715](https://github.com/user-attachments/assets/b9368acb-7339-447d-afe3-1209e1a17ebe)

## 3- Show the year and subject that won 'Albert Einstein' his prize.
**SELECT yr, subject
FROM nobel
 WHERE winner = 'Albert Einstein'**
![Screenshot 2025-02-07 140728](https://github.com/user-attachments/assets/3d6552c0-c897-45be-9ccf-eba86af2398e)

## 4- Give the name of the 'peace' winners since the year 2000, including 2000.
**Select winner winner_peace
FROM nobel
WHERE subject = 'peace'
AND yr >= 2000 **
![Screenshot 2025-02-07 141310](https://github.com/user-attachments/assets/520235e8-a5de-4130-ab39-6f19ba0ab084)

## 5- Show all details (yr, subject, winner) of the literature prize winners for 1980 to 1989 inclusive.
SELECT yr, subject, winner
FROM nobel
WHERE subject = 'literature' 
AND yr BETWEEN 1980 AND 1989

![Screenshot 2025-02-07 141646](https://github.com/user-attachments/assets/bdf0c3d9-f7bb-42fa-aed2-a2ab7eda387a)

## 6- Show all details of the presidential winners:
### *Theodore Roosevelt
### *Thomas Woodrow Wilson
### *Jimmy Carter
### *Barack Obama

**SELECT * FROM nobel
WHERE winner IN ('Theodore Roosevelt', 'Thomas Woodrow Wilson', 'Jimmy Carter', 'Barack Obama')**

![Screenshot 2025-02-07 141903](https://github.com/user-attachments/assets/5eeed53f-183e-4332-8f7b-5043a8dd1d86)

## 7- Show the winners with first name John

**SELECT winner FROM nobel
WHERE winner LIKE 'John%'**

![Screenshot 2025-02-07 142108](https://github.com/user-attachments/assets/fe40dfe8-3462-4653-a79b-cb172c6a93c1)

## 8- Show the year, subject, and name of physics winners for 1980 together with the chemistry winners for 1984.

**SELECT * FROM nobel 
WHERE (yr = 1980 AND subject = 'Physics')
   OR (yr = 1984 AND subject = 'Chemistry');**
   
![Screenshot 2025-02-07 142912](https://github.com/user-attachments/assets/c28c0daa-e0aa-4e74-9354-c4e31188340c)

## 9-Show the year, subject, and name of winners for 1980 excluding chemistry and medicine
**SELECT * FROM nobel
WHERE (yr = 1980)
AND subject NOT in ('medicine', 'chemistry')**

![Screenshot 2025-02-07 143652](https://github.com/user-attachments/assets/6d879e6b-df94-4676-9a3e-d21fc6233313)

## 10-Show year, subject, and name of people who won a 'Medicine' prize in an early year (before 1910, not including 1910) together with winners of a 'Literature' prize in a later year (after 2004, including 2004)
**SELECT * FROM nobel
WHERE (yr < 1910 AND subject = 'Medicine')
 OR ( yr >= 2004 AND subject  = 'Literature');**

![Screenshot 2025-02-07 144052](https://github.com/user-attachments/assets/06efe999-20ac-4991-b910-7c4ad97c5625)

## 11-Find all details of the prize won by PETER GRÜNBERG
**SELECT * FROM nobel 
WHERE winner = 'PETER GRünBERG'**
![Screenshot 2025-02-07 144405](https://github.com/user-attachments/assets/a439c0de-aea4-4bb4-8a06-84d96abc2bf9)


## 12-Find all details of the prize won by EUGENE O'NEILL
**SELECT * FROM nobel 
WHERE winner = "Eugene O'Neill"**
![Screenshot 2025-02-07 144522](https://github.com/user-attachments/assets/eca15dff-973c-442f-adaa-9133c9e8b55b)


## 13-List the winners, year and subject where the winner starts with Sir. Show the the most recent first, then by name order.
**SELECT winner, yr, subject FROM nobel
WHERE winner LIKE 'Sir%'
ORDER BY yr DESC , winner ASC;**

![Screenshot 2025-02-07 145431](https://github.com/user-attachments/assets/62e0d374-2b04-48f5-9071-83dffd18955a)


## 14-Show the 1984 winners and subject ordered by subject and winner name; but list chemistry and physics last
**SELECT Winner, Subject
FROM nobel
WHERE yr = 1984
ORDER BY 
  CASE 
    WHEN Subject IN ('Chemistry', 'Physics') THEN 1 
    ELSE 0 
  END, 
  Subject, 
  Winner;**
![Screenshot 2025-02-07 150446](https://github.com/user-attachments/assets/e144427e-2098-4f95-a8b2-65b0783be755)
