# CPAN-228 Assignment 0: Project Planning

## Project Name
**Learn-Quest**

## Category
Online Learning Platform (Education & Course Management)

## Team Members
- Kuldip Sahota  
- Edgar Cobb  
- Maheen Khan  

## Date
February 15, 2026

---

# 1. Project Overview & Motivation

## What We're Building

**LearnQuest** is an educational platform where instructors can create courses, students can enroll, and both can track progress through assignments and grades. The platform focuses on structured learning and simplified performance monitoring.

## Key Innovations

- Role-based access control (Student, Instructor, Admin)
- Instructors can create courses, assignments, and track grades
- Students can enroll, submit assignments, and monitor progress
- Real-time dashboard updates for grades and completion status

---

# 2. Core Concept: How the Platform Works

## Platform Workflow

1. Instructors log in and create courses
2. Students log in and browse available courses
3. Students enroll and access course materials
4. Instructors create assignments with deadlines and grading rubrics
5. Students submit assignments online
6. Instructors grade submissions
7. The system updates grades automatically
8. Dashboards display completion status and performance
9. Admins manage users, courses, and assignments system-wide

## Course Flow

### Student Perspective
Enrollment → Access Materials → Submission → Grade

### Instructor Perspective
Course Creation → Assignment Management → Grading → Progress Tracking

---

# 3. Domain Model Definition

Below are the core entities powering **LearnQuest**:

---

## Entity 1: User

Represents all platform accounts (Students, Instructors, Admins).

| Field Name   | Type                                      | Description |
|--------------|-------------------------------------------|------------|
| userId       | Long (Primary Key)                        | Unique account ID |
| username     | String (Unique)                           | Login username |
| email        | String (Unique)                           | User email address |
| passwordHash | String                                    | Encrypted password (BCrypt) |
| role         | Enum (STUDENT, INSTRUCTOR, ADMIN)         | Determines permissions |
| isActive     | Boolean                                   | Account enabled/disabled |
| createdAt    | LocalDateTime                             | Registration date |

---

## Entity 2: Course

A course created and managed by an instructor.

| Field Name    | Type                                        | Description |
|---------------|---------------------------------------------|------------|
| courseId      | Long (Primary Key)                          | Unique course ID |
| instructorId  | Long (FK → User.userId)                     | Instructor who owns course |
| title         | String                                      | Course name |
| description   | String                                      | Course details |
| startDate     | LocalDateTime                               | Course start date |
| endDate       | LocalDateTime                               | Course end date |
| createdAt     | LocalDateTime                               | Record creation date |

---

## Entity 3: Enrollment

Links students to courses.

| Field Name    | Type                                        | Description |
|---------------|---------------------------------------------|------------|
| enrollmentId  | Long (Primary Key)                          | Unique enrollment record |
| studentId     | Long (FK → User.userId)                     | Enrolled student |
| courseId      | Long (FK → Course.courseId)                 | Course enrolled in |
| enrolledOn    | LocalDateTime                               | Date student joined |

---

## Entity 4: Assignment

Represents assignments within a course.

| Field Name    | Type                                        | Description |
|---------------|---------------------------------------------|------------|
| assignmentId  | Long (Primary Key)                          | Unique assignment ID |
| courseId      | Long (FK → Course.courseId)                 | Related course |
| title         | String                                      | Assignment title |
| description   | String                                      | Instructions/details |
| dueDate       | LocalDateTime                               | Submission deadline |
| maxScore      | Double                                      | Maximum points |
| createdAt     | LocalDateTime                               | Record creation date |

---

## Entity 5: Submission

Represents a student's submission for an assignment.

| Field Name     | Type                                           | Description |
|----------------|------------------------------------------------|------------|
| submissionId   | Long (Primary Key)                             | Unique submission ID |
| assignmentId   | Long (FK → Assignment.assignmentId)            | Related assignment |
| studentId      | Long (FK → User.userId)                        | Submitting student |
| submittedAt    | LocalDateTime                                  | Submission date |
| fileUrl        | String                                         | Uploaded file or text |
| status         | Enum (SUBMITTED, LATE, GRADED)                 | Submission state |

---

## Entity 6: Grade

Stores grading information.

| Field Name     | Type                                           | Description |
|----------------|------------------------------------------------|------------|
| gradeId        | Long (Primary Key)                             | Unique grade record |
| submissionId   | Long (FK → Submission.submissionId)            | Graded submission |
| score          | Double                                         | Numeric grade |
| feedback       | String                                         | Instructor comments |
| gradedAt       | LocalDateTime                                  | Date graded |

---

# ER Diagram Relationships

- **User (STUDENT)** ⟷ Enrollment ⟷ Course  
  - Many-to-many relationship via Enrollment  

- **User (INSTRUCTOR)** ⟷ Course  
  - One-to-many relationship  

- **Course** ⟷ Assignment  
  - One-to-many relationship  

- **Assignment** ⟷ Submission ⟷ User (STUDENT)  
  - Many-to-many relationship via Submission  

- **Submission** ⟷ Grade  
  - One-to-one relationship  

---

<img width="975" height="1323" alt="image" src="https://github.com/user-attachments/assets/7d5ad468-eec3-4e82-b8e6-3c2936a6e2b8" />

# 4. User Roles & Access Control

| Role       | View Courses | Create Courses | Manage Assignments | Manage Users |
|------------|-------------|---------------|--------------------|--------------|
| STUDENT    | ✓ Browse & enroll | ✗ No | ✗ No | ✗ No |
| INSTRUCTOR | ✓ Their courses | ✓ Own courses | ✓ Own assignments | ✗ No |
| ADMIN      | ✓ All courses | ✓ All courses | ✓ All assignments | ✓ All users |

---


# 5. UI Wireframes / Layout Design

## Page 1: Home / Dashboard (Public Page)
- Lists available courses  
- Login / Register buttons  
- Featured instructors  

<img width="758" height="1264" alt="image" src="https://github.com/user-attachments/assets/6f169d79-c274-4741-910b-01817abd63a8" />

## Page 2: Course Details (Student Page)
- Displays enrolled students  
- Syllabus and assignments  
- Enroll button (if not enrolled)

<img width="677" height="1128" alt="image" src="https://github.com/user-attachments/assets/be1a3bf5-56ba-45f4-9558-c8b37f6799dc" />

## Page 3: Instructor Dashboard
- List of created courses  
- Create / Edit / Delete courses  
- View student submissions

<img width="728" height="1119" alt="image" src="https://github.com/user-attachments/assets/39e05307-0ed3-4ba5-bd78-e406b57d790c" />

## Page 4: Assignment Submission (Student Page)
- Upload files or submit text  
- View grades and feedback

<img width="711" height="1187" alt="image" src="https://github.com/user-attachments/assets/9b159279-9f5c-450d-8095-567792341350" />

## Page 5: Login Page
- Username & password fields  
- Registration link  
- Error messages for failed login

<img width="666" height="1078" alt="image" src="https://github.com/user-attachments/assets/d85898dd-c6ed-48a6-a3d5-660081582820" />

## Page 6: Admin Panel
- Manage users, courses, assignments  
- View platform statistics

<img width="664" height="1109" alt="image" src="https://github.com/user-attachments/assets/26326eee-eb95-4dc4-8885-126c27a7fc81" />
