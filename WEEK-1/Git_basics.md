> **Git and SQL Notes**

**[Git Basics]{.underline}**

**What is Git?**

-   Git is a **distributed version control system (VCS)** used for
    tracking code changes, collaboration, and source control.

-   It helps developers **manage versions, prevent data loss, and
    recover previous versions** of code.

**Why Use Git?**

-   Works across **Windows, Mac, and Linux**.

-   **Open-source** with no licensing cost.

-   Supports **branches, parallel workflows, and large projects**.

**[Setting Up Git]{.underline}**

**Installing Git**

-   Download and install from [Git-SCM](https://git-scm.com/).

-   Mac users use **Terminal**, Windows users use **Git Bash**.

**Configure Git**

git config \--global user.name \"Your Name\"

git config \--global user.email \"youremail@example.com\"

-   Ensures your commits are associated with your identity.

**[Basic Git Commands]{.underline}**

**Adding, Committing, Pulling, and Pushing**

**Add Files**

git add \--all

-   Stages all changes (new, modified, deleted files).

**Commit Changes**

git commit -m \"Commit message\"

-   Saves the current state of files with a meaningful message.

**Push to Remote Repository**

git push origin main

-   Sends local commits to the remote repository.

**Pull Updates from Remote Repository**

git pull origin main

-   Fetches and merges changes from the remote repository.

**[Branching in Git]{.underline}**

**Creating and Switching Branches**

**Create a Branch Without Switching**

> git branch feature-branch

**Create and Switch to a Branch**

> git checkout -b feature-branch
>
> Equivalent to:
>
> git branch feature-branch
>
> git checkout feature-branch

**View All Branches**

> git branch

**Delete a Local Branch**

> git branch -d branch-name

-   Use -D to force delete an unmerged branch:

> git branch -D branch-name

**Delete a Remote Branch**

> git push origin \--delete branch-name

**[Upstream Tracking in Git]{.underline}**

-   Links a **local branch** to a **remote branch**.

-   Allows simple git pull and git push without specifying branches.

**Set Upstream Tracking**

git push -u origin branch-name

**Check Upstream Branch**

git branch -vv

**Change Upstream Branch**

git branch \--set-upstream-to=origin/new-branch

**Additional Git Commands**

**Check Status**

git status

-   Shows uncommitted changes.

**View Commit History**

> git log
>
> git log \--oneline -n2 \# Shows last 2 commits in one line

**View Differences**

> git diff
>
> git diff \--staged \# Shows staged differences
>
> git diff filename \# Shows differences in a specific file

**Reset Local Changes**

> git reset \--hard origin/branch-name

-   Resets local branch to match the remote branch.

**Fetch Updates Without Merging**

> git fetch \--all

-   Downloads changes but does not apply them.

**SQL - Databases and Constraints**

**Creating a Database**

CREATE DATABASE database_name;

USE database_name;

**Creating Tables**

CREATE TABLE employees (

emp_id INT PRIMARY KEY,

name VARCHAR(50),

salary DECIMAL(10,2),

department VARCHAR(50)

);

**Inserting Data**

INSERT INTO employees VALUES (1, \'John Doe\', 50000.00, \'IT\');

**Retrieving Data**

SELECT \* FROM employees;

**SQL Joins**

**LEFT JOIN Example**

SELECT e.emp_id, e.name, e.salary,

COALESCE(d.department_name, \'No Department\') AS department

FROM employees e

LEFT JOIN departments d ON e.department_id = d.department_id

WHERE d.department_id IS NULL;

-   Retrieves employees who do not belong to any department.

**[SQL ON DELETE & ON UPDATE Constraints]{.underline}**

**Creating a Table with Constraints**

CREATE TABLE enrollments (

enrollment_id INT PRIMARY KEY,

student_id INT,

course_id INT DEFAULT 999,

enrollment_date DATE,

FOREIGN KEY (student_id) REFERENCES students(student_id)

ON DELETE CASCADE ON UPDATE CASCADE,

FOREIGN KEY (course_id) REFERENCES courses(course_id)

ON DELETE SET NULL ON UPDATE SET DEFAULT

);

**Constraints Explanation:**

-   **ON DELETE CASCADE**: Deleting a student deletes related
    enrollments.

-   **ON DELETE SET NULL**: Deleting a course sets course_id to NULL in
    enrollments.

-   **ON UPDATE CASCADE**: Updating student_id updates references in
    enrollments.

-   **ON UPDATE SET DEFAULT**: Updating course_id changes it to a
    default value.

**[SQL Triggers]{.underline}**

**Creating a Trigger Before Deleting a Course**

CREATE TABLE enrollment_logging (

enrollment_id INT PRIMARY KEY,

action VARCHAR(200),

change_date DATE

);

CREATE TRIGGER before_course_deletion

BEFORE DELETE ON courses

FOR EACH ROW

INSERT INTO enrollment_logging (enrollment_id, action, change_date)

VALUES (OLD.course_id, \'DELETE BASED ON DELETION FROM COURSE\', NOW());

-   **Triggers execute automatically** when a specified action occurs
    (e.g., DELETE).

**[SQL DELETE Operations]{.underline}**

**Deleting a Course (Cascade Deletion)**

> DELETE FROM courses WHERE course_id = 1;

-   Deletes the course and its associated enrollments.

**Verifying Deletions**

> SELECT \* FROM enrollments;
>
> SELECT \* FROM courses;
