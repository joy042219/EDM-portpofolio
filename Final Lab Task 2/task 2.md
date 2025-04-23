# Final Lab Task 2: Transforming ER Model to Relational Tables
 # Task Description
 The task is to convert an ER diagram of student assignment submissions into MySQL tables by defining entities and their attributes, establishing relationships, and identifying primary and foreign keys. It also involves accurately representing any dependent or weak entities to create a normalized database schema for effective data management.

# Step by Step
 these are the Steps in order to complete the tasks usigng My SQL Workbench

# Step 1 Create the Student Table
## Student tabale
- username: String (VARCHAR) up to 55 characters. 

<img src="image/2 stu_tbl.PNG" alt="Alt Text" width="400" height="300">

 # Step 2  Create the Assignment Table
 ## Assignment Table
 - shortname: String (VARCHAR) up to 50 characters.
 - due_date: cannot be NULL
 - url: String (VARCHAR), up to 255 characters, can be NULL.
 <img src="image/2 assignment_tbl.PNG" alt="Alt Text" width="400" height="300">

# Step 3  Create the Submission Table
## Submission Table
- username: String (VARCHAR), up to 50 characters.
- shortname: String (VARCHAR), up to 50 characters.
- version: Integer, represent the version of the submision.
- submit_date: Date, cannot be null.
- data: Text. 
 <img src="image/2 SUBMISSION_TBL.PNG" alt="Alt Text" width="400" height="300">

 # ER Diagram or Relational Schema
  <img src="image/ERD task 2.PNG" alt="Alt Text" width="400" height="400">
 
  # SQL Copy of the Database  ->> [Student Assignment Submission](https://github.com/joy042219/EDM-portpofolio/blob/main/Final%20Lab%20Task%202/image/Sql%20copy)


