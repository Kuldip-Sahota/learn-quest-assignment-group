CPAN-228 Assignment 0: Project Planning

Project Name: Learn-Quest

Category: Online Learning Platform (Education & Course Management)

Team Members:
•	Kuldip Sahota 
•	Edgar Cobb 
•	Maheen Khan 

Date: 2/15/26

1. Project Overview & Motivation
What We're Building:

LearnQuest is an educational platform where instructors create courses, students enroll, and both track progress through assignments and grades. The platform focuses on structured learning and easy monitoring of performance

Key Innovation:
•	Role-based access control ensures instructors and students only see what they’re allowed to
•	Instructors can create courses, assignments, and track grades
•	Students can enroll, submit assignments, and view their progress
•	Real-time dashboard updates show grades and completion status

**2**. Core Concept: How the Platform Works (High Level)
1.	Instructors log in and create courses
2.	Students log in and browse available courses
3.	Students enroll in courses and access learning material
4.	Instructors create assignments with deadlines and grading rubrics
5.	Students submit assignments online
6.	System automatically updates grades when instructors grade submissions
7.	Progress dashboards show each student’s completion and performance
8.	Admins manage users, courses, and assignments system-wide
Course Flow:
•	Students enroll → Access materials → Submit assignments → Receive grades – still valid; now uses Enrollment → Submission → Grade
•	Instructors create content → Manage assignments → Grade submissions → Track student progress – still valid; uses Course, Assignment, Submission, Grade

3. Domain Model Definition
Below are 7 core entities powering LearnQuest:
Entity 1: User
Represents all platform accounts (students, instructors, admins).
Field Name	Type	Description
userId	Long (Primary Key)	Unique account ID
username	String, Unique	Login username
email	String, Unique	User email address
passwordHash	String	Encrypted password (BCrypt)
role	Enum (STUDENT, INSTRUCTOR, ADMIN)	Determines system permissions
isActive	Boolean	Account enabled/disabled
createdAt	LocalDateTime	Registration date

Entity 2: Course
A course created and managed by an instructor.
Field Name	Type	Description
courseId	Long (Primary Key)	Unique course ID
instructorId	Long (Foreign Key → User.userId)	Instructor who owns course
title	String	Course name
description	String	Course details
startDate	LocalDateTime	Course start date
endDate	LocalDateTime	Course end date
createdAt	LocalDateTime	Record creation date

Entity 3: Enrollment
Links students to courses they join.
Field Name	Type	Description
enrollmentId	Long (Primary Key)	Unique enrollment record
studentId	Long (Foreign Key → User.userId)	Student enrolled
courseId	Long (Foreign Key → Course.courseId)	Course enrolled in
enrolledOn	LocalDateTime	Date student joined course


Entity 4: Assignment
A student’s submission for an assignment.
Field Name	Type	Description
assignmentId	Long (PK)	Unique assignment ID
courseId	Long (FK → Course.courseId)	Course it belongs to
title	String	Assignment title
description	String	Instructions/details
dueDate	LocalDateTime	Submission deadline
maxScore	Double	Maximum points possible
createdAt	LocalDateTime	Record creation date

Entity 5: Submission
A student’s submission for an assignment
Field Name	Type	Description
submissionId	Long (Primary Key)	Unique submission ID
assignmentId	Long (Foreign Key → Assignment.assignmentId)	Assignment submitted
studentId	Long (Foreign Key → User.userId)	Student who submitted
submittedAt	LocalDateTime	Submission date
fileUrl	String	Uploaded file location or text answer
status	Enum (SUBMITTED, LATE, GRADED)	Submission state

Entity 6: Grade
Stores grading information for a submission.
Field Name	Type	Description
gradeId	Long (Primary Key)	Unique grade record
submissionId	Long (Foreign Key → Submission.submissionId)	Submission graded
score	Double	Numeric grade
feedback	String	Instructor comments
gradedAt	LocalDateTime	Date graded


ER Diagram:
•	User (role = STUDENT) ⬌ Enrollment ⬌ Course
(Students enroll in courses; many-to-many relationship via Enrollment table)
•	User (role = INSTRUCTOR) ⬌ Course
(Each course is created and managed by an instructor; one-to-many)
•	Course ⬌ Assignment
(Each course has many assignments; one-to-many)
•	Assignment ⬌ Submission ⬌ User (role = STUDENT)
(Students submit assignments; many-to-many relationship via Submission table)
•	Submission ⬌ Grade
(Each submission can have one grade; one-to-one)


<img width="975" height="1323" alt="image" src="https://github.com/user-attachments/assets/7d5ad468-eec3-4e82-b8e6-3c2936a6e2b8" />

4. User Roles & Access Control
Role	View Courses	Create Courses	Manage Assignments	Manage Users
STUDENT	✓ Browse & enroll in courses they’re not enrolled in	X No	X No	X  No
INSTRUCTOR	✓ All courses they teach	✓ Create/Edit/Delete courses they own	✓ Create/Edit/Delete assignments in their courses	X  No
ADMIN	✓ All courses	✓ All courses	✓ All assignments	✓ All users


5. UI Wireframes / Layout Design
Page 1: Home / Dashboard (Public Page)
•	Lists available courses
•	Login / Register buttons
•	Highlights featured instructors

<img width="758" height="1264" alt="image" src="https://github.com/user-attachments/assets/6f169d79-c274-4741-910b-01817abd63a8" />

Page 2: Course Details (Student Page)
•	Shows enrolled students, syllabus, assignments
•	Enroll button (if not enrolled)

<img width="677" height="1128" alt="image" src="https://github.com/user-attachments/assets/be1a3bf5-56ba-45f4-9558-c8b37f6799dc" />

Page 3: Instructor Dashboard
•	List of created courses
•	Create / Edit / Delete course
•	View student submissions per assignment

<img width="728" height="1119" alt="image" src="https://github.com/user-attachments/assets/39e05307-0ed3-4ba5-bd78-e406b57d790c" />


Page 4: Assignment Submission (Student Page)
•	Upload files or submit text answers
•	View grade and feedback

<img width="711" height="1187" alt="image" src="https://github.com/user-attachments/assets/9b159279-9f5c-450d-8095-567792341350" />


Page 5: Login Page 
•	Username & password
•	Registration link
•	Error messages for failed login

<img width="666" height="1078" alt="image" src="https://github.com/user-attachments/assets/d85898dd-c6ed-48a6-a3d5-660081582820" />


Page 6: Admin Panel
•	Manage users, courses, assignments
•	View platform stats

<img width="664" height="1109" alt="image" src="https://github.com/user-attachments/assets/26326eee-eb95-4dc4-8885-126c27a7fc81" />





