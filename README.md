# Introduction to the Online Therapist Database Project

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
This schema serves as a visual representation of the SQL tables and the relationships between 
them, aiding in understanding the database structure and data flow.

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

**Client Overview**
This code block fetches a concise overview of clients, displaying their ID, first name, last name, and email.

```sql
SELECT id, first_name, last_name, email
FROM Clients;
```

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

**Therapist Languages**
This block of code retrieves the languages spoken by therapists, along with their respective first names.

```sql
SELECT t.first_name AS therapist_first_name, l.language
FROM Languages l
JOIN Therapists t ON l.therapist_id = t.id;
```

**Client Activity**
This code block displays the first name of clients and the total number of sessions they have participated in.

```sql
SELECT c.first_name AS client_first_name, COUNT(*) AS total_sessions
FROM Sessions s
JOIN Clients c ON s.client_id = c.id
GROUP BY c.id;
```

**Client Reviews**
The following query fetches client reviews, including the client's first name, therapist's first name, rating, review text, and date.

```sql
SELECT c.first_name AS client_first_name, t.first_name AS therapist_first_name, rating, review_text, date
FROM ClientReviews cr
JOIN Clients c ON cr.client_id = c.id
JOIN Therapists t ON cr.therapist_id = t.id;
```

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

**Therapist Activity**
This code block provides an overview of therapist activity, including the therapist's first name and the total number of sessions they have conducted.

```sql
SELECT t.first_name AS therapist_first_name, COUNT(*) AS total_sessions
FROM Sessions s
JOIN Therapists t ON s.therapist_id = t.id
GROUP BY t.id;
```

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

**Payment Transactions**
This block of code retrieves payment transaction details, including the transaction ID, session date, client's first name, therapist's first name, and transaction amount.

```sql
SELECT pt.id, s.date AS session_date, c.first_name AS client_first_name, t.first_name AS therapist_first_name, pt.amount
FROM PaymentTransactions pt
JOIN Sessions s ON pt.session_id = s.id
JOIN Clients c ON pt.client_id = c.id
JOIN Therapists t ON pt.therapist_id = t.id;
```

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



# Conclusion

The Online Therapist Database project showcases my expertise in SQL and database management. Through the creation of this comprehensive 
and sophisticated platform, I aim to illustrate the capabilities of SQL in designing efficient, scalable, and user-friendly databases for 
contemporary online services. This project stands as a testament to my commitment to leveraging technology to promote a healthier and happier 
world, where therapy is easily accessible to those in need.
