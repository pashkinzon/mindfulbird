# Mindful Bird - SQL Database Project

In an ever-changing world, where stress and emotional challenges can be overwhelming, mental health and counseling 
services play a pivotal role in supporting individuals' well-being. To cater to the growing demand for accessible therapeutic assistance,
I embarked on an exciting journey to create an Online Therapist Database using SQL. This project showcases my skills in designing 
and managing a relational database, as well as my ability to create a comprehensive platform for finding therapists online.

# Project Goal

The primary objective of this project was to develop an efficient and user-friendly database system that facilitates 
seamless connections between clients and therapists. By crafting an intelligently structured database, my goal was to demonstrate 
how SQL can be harnessed to build a robust foundation for an online platform that empowers users to discover and connect with their ideal therapists.

# Project Overview

The foundation of the Online Therapist Database revolves around carefully designed tables that capture crucial entities of the platform. 

**Clients Table**

This table stores information about clients using the service, including their unique ID, first name, last name, email, and a brief bio.

```sql
CREATE TABLE Clients (
    id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    bio TEXT
);
```

**Therapists Table**

This table holds data about therapists registered on the platform. It includes their unique ID, first name, last name, email, bio, and registration date.

```sql
CREATE TABLE Therapists (
    id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    bio TEXT,
    registration_date DATE
);
```

**Specializations Table**

This table allows therapists to specify their areas of expertise. It contains unique IDs and specialization names.

```sql
CREATE TABLE Specializations (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100)
);
```

**Client Reviews Table**

This table records reviews submitted by clients for their therapy sessions. It includes a unique review ID, client ID, therapist ID, rating, review text, and date.

```sql
CREATE TABLE ClientReviews (
    id INT AUTO_INCREMENT PRIMARY KEY,
    client_id INT,
    therapist_id INT,
    rating FLOAT,
    review_text TEXT,
    date DATE,
    FOREIGN KEY (client_id) REFERENCES Clients(id),
    FOREIGN KEY (therapist_id) REFERENCES Therapists(id)
);
```

**Availability Table**

The Availability table stores information about therapists' weekly schedules, including unique IDs, therapist IDs, the day of the week, start time, and end time.

```sql
CREATE TABLE Availability (
    id INT AUTO_INCREMENT PRIMARY KEY,
    therapist_id INT,
    day_of_week VARCHAR(20),
    start_time TIME,
    end_time TIME,
    FOREIGN KEY (therapist_id) REFERENCES Therapists(id)
);
```

**Sessions Table**

This table acts as a bridge between clients and therapists, storing session data such as unique IDs, client IDs, therapist IDs, session date, a flag indicating whether the session happened, and the payment status.

```sql
CREATE TABLE Sessions (
    id INT AUTO_INCREMENT PRIMARY KEY,
    client_id INT,
    therapist_id INT,
    date DATE,
    if_happened BOOLEAN,
    payment_status ENUM('pending', 'completed', 'cancelled'), 
    FOREIGN KEY (client_id) REFERENCES Clients(id),
    FOREIGN KEY (therapist_id) REFERENCES Therapists(id)
);
```

**Session Notes Table**

The Session Notes table holds notes related to therapy sessions. It contains unique IDs, session IDs, general session notes, and private notes intended only for therapists.

```sql
CREATE TABLE SessionNotes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    session_id INT,
    notes_text TEXT,
    therapist_private_notes TEXT,
    FOREIGN KEY (session_id) REFERENCES Sessions(id)
);
```

**Payment Transactions Table**

This table records payment transactions associated with therapy sessions. It includes unique IDs, session IDs, client IDs, therapist IDs, payment amounts, and dates.

```sql
CREATE TABLE PaymentTransactions (
    id INT AUTO_INCREMENT PRIMARY KEY,
    session_id INT, 
    client_id INT,
    therapist_id INT,
    amount DECIMAL(10, 2),
    date DATE,
    FOREIGN KEY (session_id) REFERENCES Sessions(id), 
    FOREIGN KEY (client_id) REFERENCES Clients(id),
    FOREIGN KEY (therapist_id) REFERENCES Therapists(id)
);
```

**Participants Table**

The Participants table serves as a centralized entity for both clients and therapists. It includes unique IDs, first name, last name, email, bio, and a type flag indicating whether the participant is a client or a therapist.

```sql
CREATE TABLE Participants (
    id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    bio TEXT,
    type ENUM('client', 'therapist') 
);
```

**Chat Messages Table**

This table facilitates communication between clients and therapists within a session chat. It includes unique message IDs, session IDs, sender IDs, receiver IDs, message text, and timestamps.

```sql
CREATE TABLE ChatMessages (
    id INT AUTO_INCREMENT PRIMARY KEY,
    session_id INT,
    sender_id INT,
    receiver_id INT,
    message_text TEXT,
    timestamp DATETIME,
    FOREIGN KEY (session_id) REFERENCES Sessions(id),
    FOREIGN KEY (sender_id) REFERENCES Participants(id) ON DELETE CASCADE,
    FOREIGN KEY (receiver_id) REFERENCES Participants(id) ON DELETE CASCADE
);
```

**Therapist Credentials Table**

This table holds data about therapists' credentials, such as degrees or certifications. It includes unique IDs, therapist IDs, qualification names, issuing institutions, and completion years.

```sql
CREATE TABLE Credentials (
    id INT AUTO_INCREMENT PRIMARY KEY,
    therapist_id INT,
    qualification VARCHAR(100),
    issuing_institution VARCHAR(100),
    completion_year INT,
    FOREIGN KEY (therapist_id) REFERENCES Therapists(id)
);
```

**Therapist Languages Table**

This table stores the languages in which therapists can communicate. It includes unique IDs, therapist IDs, and language names.

```sql
CREATE TABLE Languages (
    id INT AUTO_INCREMENT PRIMARY KEY,
    therapist_id INT,
    language VARCHAR(50),
    FOREIGN KEY (therapist_id) REFERENCES Therapists(id)
);
```
The schema of the resulting database was then provided to the data scientists' team. 
[This schema serves as a visual representation of the SQL tables and the relationships between 
them](https://drawsql.app/teams/pavel-polishchuk-team/diagrams/mindful-bird-sql-project), aiding in understanding the database structure and data flow.


<figure>
  <img src="https://i.postimg.cc/GthQcGwc/SQL-Diagram.png" alt="SQL-Diagram" width="600">
  <figcaption>Figure 1: SQL Database Schema</figcaption>
</figure>


# Data Input

The entire dataset for the SQL database was automatically generated using advanced AI tools, ensuring that all 
data matches were generated purely by coincidence and do not represent real-world associations or relationships.

The sample data for this SQL database can be found in the GitHub repository within the SQL code file.

# Data Manipulation and Demonstration 

The Online Therapist Database project not only encompasses the creation of the database but also entails a range of SQL queries 
to showcase the system's functionality. I can execute queries to retrieve information such as therapist availability, client activity, 
client reviews, payment transactions, and more.

**Therapist Overview**

This block of code retrieves essential information about therapists, including their ID, first name, last name, email, and registration date.

```sql
SELECT id, first_name, last_name, email, registration_date
FROM Therapists;
```

|id |first_name  |last_name|email                       |registration_date|
|---|------------|---------|----------------------------|-----------------|
|1  |Dr. Sarah   |Johnson  |sarah.johnson@example.com   |2022-11-15       |
|2  |Dr. Michael |Smith    |michael.smith@example.com   |2022-12-10       |
|3  |Dr. Jennifer|Brown    |jennifer.brown@example.com  |2023-01-05       |
|4  |Dr. David   |Thomas   |david.thomas@example.com    |2023-02-20       |
|5  |Dr. Emily   |Garcia   |emily.garcia@example.com    |2023-03-01       |
|6  |Dr. William |Martinez |william.martinez@example.com|2023-04-02       |
|7  |Dr. Olivia  |Lopez    |olivia.lopez@example.com    |2023-04-15       |
|8  |Dr. Ethan   |Rodriguez|ethan.rodriguez@example.com |2023-05-05       |
|9  |Dr. Amelia  |Wilson   |amelia.wilson@example.com   |2023-05-20       |
|10 |Dr. James   |Gomez    |james.gomez@example.com     |2023-06-10       |


**Client Overview**

This code block fetches a concise overview of clients, displaying their ID, first name, last name, and email.

```sql
SELECT id, first_name, last_name, email
FROM Clients;
```

|id |first_name  |last_name|email                       |
|---|------------|---------|----------------------------|
|1  |John        |Doe      |john.doe@example.com        |
|2  |Jane        |Smith    |jane.smith@example.com      |
|3  |Michael     |Brown    |michael.brown@example.com   |
|4  |Emily       |Johnson  |emily.johnson@example.com   |
|5  |Sophia      |Lee      |sophia.lee@example.com      |
|6  |William     |Wong     |william.wong@example.com    |
|7  |Olivia      |Chen     |olivia.chen@example.com     |
|8  |James       |Kim      |james.kim@example.com       |
|9  |Sophie      |Nguyen   |sophie.nguyen@example.com   |
|10 |Alexander   |Garcia   |alexander.garcia@example.com|
|11 |Ava         |Martinez |ava.martinez@example.com    |
|12 |Liam        |Lopez    |liam.lopez@example.com      |
|13 |Isabella    |Torres   |isabella.torres@example.com |
|14 |Benjamin    |Rivera   |benjamin.rivera@example.com |
|15 |Mia         |Perez    |mia.perez@example.com       |
|16 |Ethan       |Gomez    |ethan.gomez@example.com     |
|17 |Amelia      |Diaz     |amelia.diaz@example.com     |
|18 |Oliver      |Martin   |oliver.martin@example.com   |
|19 |Harper      |Rodriguez|harper.rodriguez@example.com|
|20 |Evelyn      |Wilson   |evelyn.wilson@example.com   |


**Therapist Availability Overview**

The following code generates an overview of therapist availability, presenting the therapist's full name, the day of the week, start time, and end time of their available sessions.

```sql
SELECT CONCAT(t.first_name, ' ', t.last_name) AS therapist_name, 
       a.day_of_week,
       TIME_FORMAT(a.start_time, '%H:%i') AS start_time,
       TIME_FORMAT(a.end_time, '%H:%i') AS end_time
FROM Therapists t
JOIN Availability a ON t.id = a.therapist_id;
```

|therapist_name|day_of_week |start_time|end_time                    |
|--------------|------------|----------|----------------------------|
|Dr. Sarah Johnson|Monday      |09:00     |16:00                       |
|Dr. Sarah Johnson|Tuesday     |10:00     |17:00                       |
|Dr. Sarah Johnson|Wednesday   |09:30     |14:30                       |
|Dr. Michael Smith|Tuesday     |13:00     |20:00                       |
|Dr. Michael Smith|Wednesday   |11:00     |18:00                       |
|Dr. Michael Smith|Thursday    |09:00     |16:00                       |
|Dr. Jennifer Brown|Monday      |10:00     |18:00                       |
|Dr. Jennifer Brown|Tuesday     |10:00     |16:00                       |
|Dr. Jennifer Brown|Wednesday   |11:30     |17:30                       |
|Dr. David Thomas|Tuesday     |09:00     |15:00                       |
|Dr. David Thomas|Thursday    |12:00     |19:00                       |
|Dr. David Thomas|Friday      |09:00     |14:00                       |
|Dr. Emily Garcia|Monday      |12:00     |18:00                       |
|Dr. Emily Garcia|Wednesday   |10:00     |16:00                       |
|Dr. Emily Garcia|Thursday    |11:00     |17:00                       |
|Dr. William Martinez|Tuesday     |14:00     |20:00                       |
|Dr. William Martinez|Wednesday   |13:00     |19:00                       |
|Dr. William Martinez|Friday      |10:00     |15:00                       |
|Dr. Olivia Lopez|Wednesday   |09:00     |17:00                       |
|Dr. Olivia Lopez|Thursday    |10:00     |16:00                       |
|Dr. Olivia Lopez|Friday      |11:00     |18:00                       |
|Dr. Ethan Rodriguez|Monday      |13:00     |19:00                       |
|Dr. Ethan Rodriguez|Wednesday   |11:00     |17:00                       |
|Dr. Ethan Rodriguez|Thursday    |09:00     |16:00                       |
|Dr. Amelia Wilson|Tuesday     |12:00     |18:00                       |
|Dr. Amelia Wilson|Thursday    |10:30     |17:30                       |
|Dr. Amelia Wilson|Friday      |09:30     |14:30                       |
|Dr. James Gomez|Monday      |10:00     |17:00                       |
|Dr. James Gomez|Wednesday   |09:00     |16:00                       |
|Dr. James Gomez|Friday      |12:00     |18:00                       |



**Therapist Languages**

This block of code retrieves the languages spoken by therapists, along with their respective first names.

```sql
SELECT t.first_name AS therapist_first_name, l.language
FROM Languages l
JOIN Therapists t ON l.therapist_id = t.id;
```

|therapist_first_name|language    |
|--------------------|------------|
|Dr. Sarah           |English     |
|Dr. Sarah           |Spanish     |
|Dr. Michael         |English     |
|Dr. Jennifer        |English     |
|Dr. Jennifer        |French      |
|Dr. David           |English     |
|Dr. David           |Portuguese  |
|Dr. Emily           |English     |
|Dr. William         |English     |
|Dr. William         |Spanish     |
|Dr. Olivia          |English     |
|Dr. Ethan           |English     |
|Dr. Ethan           |Spanish     |
|Dr. Amelia          |English     |
|Dr. Amelia          |German      |
|Dr. James           |English     |


**Client Activity**

This code block displays the first name of clients and the total number of sessions they have participated in.

```sql
SELECT c.first_name AS client_first_name, COUNT(*) AS total_sessions
FROM Sessions s
JOIN Clients c ON s.client_id = c.id
GROUP BY c.id;
```

|client_first_name|total_sessions|
|-----------------|--------------|
|John             |8             |
|Jane             |5             |
|Michael          |4             |
|Emily            |4             |
|Sophia           |5             |
|William          |5             |
|Olivia           |4             |
|James            |3             |
|Sophie           |3             |
|Alexander        |3             |
|Ava              |5             |
|Liam             |3             |
|Isabella         |3             |
|Benjamin         |2             |
|Mia              |3             |
|Ethan            |3             |
|Amelia           |2             |
|Oliver           |3             |
|Harper           |3             |
|Evelyn           |3             |


**Client Reviews**

The following query fetches client reviews, including the client's first name, therapist's first name, rating, review text, and date.

```sql
SELECT c.first_name AS client_first_name, t.first_name AS therapist_first_name, rating, review_text, date
FROM ClientReviews cr
JOIN Clients c ON cr.client_id = c.id
JOIN Therapists t ON cr.therapist_id = t.id;
```

|client_first_name|therapist_first_name|rating|review_text                                                                                                                 |date      |
|-----------------|--------------------|------|----------------------------------------------------------------------------------------------------------------------------|----------|
|John             |Dr. Sarah           |4.8   |Dr. Sarah is an excellent therapist. She helped me manage my stress effectively.                                            |2023-07-10|
|Jane             |Dr. Michael         |4.5   |Dr. Michael is very understanding and supportive. His advice on improving communication in relationships has been valuable. |2023-07-12|
|Michael          |Dr. Jennifer        |4.9   |I'm grateful to Dr. Jennifer for helping me cope with the loss of a loved one. Her compassionate approach made a difference.|2023-07-14|
|Emily            |Dr. David           |4.6   |Dr. David has been a great support in my journey to improve self-esteem. I'm seeing positive changes.                       |2023-07-16|
|Sophia           |Dr. Emily           |4.7   |I highly recommend Dr. Emily to the LGBTQ+ community. Her understanding and guidance have been invaluable to me.            |2023-07-18|
|William          |Dr. William         |4.4   |Dr. William's mindfulness-based therapy has helped me gain clarity and focus in my life.                                    |2023-07-20|
|Olivia           |Dr. Olivia          |4.3   |Dr. Olivia is a caring psychologist. Her sessions have been helpful in managing my depression.                              |2023-07-22|
|James            |Dr. Ethan           |4.7   |Dr. Ethan is an experienced couples counselor. My partner and I have seen improvements in our relationship.                 |2023-07-24|
|Sophie           |Dr. Amelia          |4.9   |I'm glad I found Dr. Amelia. Her cognitive therapy approach has been beneficial in addressing my challenges.                |2023-07-26|
|Alexander        |Dr. James           |4.5   |Dr. James has been compassionate during my journey through grief. His support has been invaluable.                          |2023-07-28|


**Session Notes Overview**

This code block provides an overview of session notes, displaying the session ID, therapist's first name, client's first name, session notes, and therapist's private notes.

```sql
SELECT s.id AS session_id,
       t.first_name AS therapist_first_name,
       c.first_name AS client_first_name,
       sn.notes_text,
       sn.therapist_private_notes
FROM SessionNotes sn
JOIN Sessions s ON sn.session_id = s.id
JOIN Therapists t ON s.therapist_id = t.id
JOIN Clients c ON s.client_id = c.id;
```

|session_id|therapist_first_name|client_first_name|notes_text                                                                                                                  |therapist_private_notes|
|----------|--------------------|-----------------|----------------------------------------------------------------------------------------------------------------------------|-----------------------|
|1         |Dr. Sarah           |John             |John had a productive session today. We discussed strategies for coping with stress and anxiety.                            |The client seemed engaged and committed to the treatment plan.|
|2         |Dr. Sarah           |John             |John made progress in recognizing thought patterns. We explored deeper into the roots of his anxiety.                       |The client expressed relief after identifying some underlying causes of anxiety.|
|3         |Dr. Sarah           |John             |John showed improvement in implementing relaxation techniques. We discussed setting achievable goals.                       |The client is motivated to continue practicing relaxation techniques and goal setting.|
|4         |Dr. Sarah           |John             |John discussed recent experiences with stressors and applied coping strategies effectively.                                 |The client demonstrated resilience in handling stressors and utilizing coping techniques.|
|5         |Dr. Sarah           |John             |John shared his experiences with Dr. Sarah Johnson and how therapy has been helpful for him.                                |The client expressed gratitude for the progress made in therapy.|
|6         |Dr. Sarah           |John             |John discussed setting new personal goals and strategies to maintain progress outside of therapy.                           |The client showed enthusiasm in applying therapy insights to daily life.|
|7         |Dr. Sarah           |John             |John worked on managing interpersonal conflicts and exploring assertiveness skills.                                         |The client is interested in practicing assertiveness in various situations.|
|8         |Dr. Sarah           |John             |John discussed his journey through therapy and expressed confidence in his coping abilities.                                |The client demonstrated growth and positive self-reflection throughout the therapy process.|
|45        |Dr. Sarah           |Ava              |Ava discussed her experiences with stress and explored coping strategies.                                                   |The client expressed an interest in mindfulness practices for stress reduction.|
|46        |Dr. Sarah           |Ava              |Ava made progress in implementing mindfulness techniques and discussed emotional regulation.                                |The client felt more equipped to manage overwhelming emotions through mindfulness.|
|47        |Dr. Sarah           |Ava              |Ava shared her experiences with time management and explored techniques for prioritization.                                 |The client is motivated to explore techniques for better time management.|
|48        |Dr. Sarah           |Ava              |Ava discussed her future goals and explored potential career paths.                                                         |The client is enthusiastic about exploring career options aligned with her interests.|


**Therapist Credentials Overview**

The following query presents an overview of therapist credentials, showing the therapist's first name, qualification, issuing institution, and completion year.

```sql
SELECT t.first_name AS therapist_first_name,
       c.qualification,
       c.issuing_institution,
       c.completion_year
FROM Therapists t
JOIN Credentials c ON t.id = c.therapist_id;
```

|therapist_first_name|qualification|issuing_institution|completion_year                                                                                                             |
|--------------------|-------------|-------------------|----------------------------------------------------------------------------------------------------------------------------|
|Dr. Sarah           |Ph.D. in Psychology|University of XYZ  |2012                                                                                                                        |
|Dr. Michael         |M.A. in Counseling Psychology|ABC Institute      |2010                                                                                                                        |
|Dr. Jennifer        |Psy.D. in Clinical Psychology|University of LMN  |2008                                                                                                                        |
|Dr. David           |M.S. in Mental Health Counseling|PQR University     |2015                                                                                                                        |
|Dr. Emily           |Ph.D. in Counseling Psychology|DEF Institute      |2017                                                                                                                        |
|Dr. William         |M.A. in Counseling|GHI University     |2014                                                                                                                        |
|Dr. Olivia          |Psy.D. in Clinical Psychology|MNO Institute      |2016                                                                                                                        |
|Dr. Ethan           |M.S. in Marriage and Family Therapy|STU University     |2013                                                                                                                        |
|Dr. Amelia          |Ph.D. in Psychology|VWX Institute      |2011                                                                                                                        |
|Dr. James           |M.A. in Counseling Psychology|YZA University     |2009                                                                                                                        |

**Therapist Session Summary**

This query summarizes the therapist's session data, including their first name, total sessions, completed sessions, and canceled sessions.

```sql
SELECT t.first_name AS therapist_first_name,
       COUNT(s.id) AS total_sessions,
       SUM(IF(s.if_happened, 1, 0)) AS completed_sessions,
       SUM(IF(s.if_happened = 0, 1, 0)) AS canceled_sessions
FROM Therapists t
LEFT JOIN Sessions s ON t.id = s.therapist_id
GROUP BY t.id;
```

|therapist_first_name|total_sessions|completed_sessions|canceled_sessions|
|--------------------|--------------|------------------|-----------------|
|Dr. Sarah           |13            |13                |0                |
|Dr. Michael         |8             |8                 |0                |
|Dr. Jennifer        |7             |7                 |0                |
|Dr. David           |6             |6                 |0                |
|Dr. Emily           |8             |8                 |0                |
|Dr. William         |8             |8                 |0                |
|Dr. Olivia          |6             |6                 |0                |
|Dr. Ethan           |6             |6                 |0                |
|Dr. Amelia          |6             |6                 |0                |
|Dr. James           |6             |6                 |0                |


**Payment Transactions**

This block of code retrieves payment transaction details, including the transaction ID, session date, client's first name, therapist's first name, and transaction amount.

```sql
SELECT pt.id, s.date AS session_date, c.first_name AS client_first_name, t.first_name AS therapist_first_name, pt.amount
FROM PaymentTransactions pt
JOIN Sessions s ON pt.session_id = s.id
JOIN Clients c ON pt.client_id = c.id
JOIN Therapists t ON pt.therapist_id = t.id;
```

|id |session_date|client_first_name|therapist_first_name|amount|
|---|------------|-----------------|--------------------|------|
|1  |2023-07-10  |John             |Dr. Sarah           |100.00|
|2  |2023-07-13  |John             |Dr. Sarah           |100.00|
|3  |2023-07-18  |John             |Dr. Sarah           |100.00|
|4  |2023-07-21  |John             |Dr. Sarah           |100.00|
|5  |2023-07-25  |John             |Dr. Sarah           |100.00|
|6  |2023-07-27  |John             |Dr. Sarah           |100.00|
|7  |2023-07-30  |John             |Dr. Sarah           |100.00|
|8  |2023-08-01  |John             |Dr. Sarah           |100.00|
|45 |2023-07-23  |Ava              |Dr. Sarah           |80.00 |
|46 |2023-07-27  |Ava              |Dr. Sarah           |80.00 |


**Payment Overview**

This query presents an overview of payment information, including the total revenue, the number of unique clients, unique therapists, and the total number of sessions.

```sql
SELECT
    SUM(amount) AS total_revenue,
    COUNT(DISTINCT client_id) AS unique_clients,
    COUNT(DISTINCT therapist_id) AS unique_therapists,
    COUNT(DISTINCT session_id) AS total_sessions
FROM PaymentTransactions;
```

|total_revenue|unique_clients|unique_therapists|total_sessions|
|-------------|--------------|-----------------|--------------|
|6010.00      |20            |10               |74            |



# Conclusion

The Online Therapist Database project showcases my expertise in SQL and database management. Through the creation of this comprehensive 
and sophisticated platform, I aim to illustrate the capabilities of SQL in designing efficient, scalable, and user-friendly databases for 
contemporary online services. This project stands as a testament to my commitment to leveraging technology to promote a healthier and happier 
world, where therapy is easily accessible to those in need.
