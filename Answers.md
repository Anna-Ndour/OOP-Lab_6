## Exercise 1
(a) true, 6 and dropped

(b) Course must have "removeStudent(Student)" and "getCredits() and UndergraduateStudent must have "removeCredits()"

(c) What will happen is an error. I would add the fragment alt and put the message [isEnrolled]

## Exercise 2

```mermaid
sequenceDiagram
    participant S as service: EnrollmentService
    participant P as printer: Printer
    create participant E as enrollment: Enrollment
    S->>E: new Enrollment(student, course)
    S-)P: printConfirmation(enrollment)
```

(a) because enrollement was created later with new participant so it appears at it's time of creation

(b) it is drawn with solid line and opened arrow -) because it shouldn.t wait.

## Exercise 3- opt

```mermaid
    sequenceDiagram
    participant S as service: EnrollmentService
    participant C as course: Course
    participant L as lab: LabSession
    S->>C: hasLab()
    C-->>S: hasLab?
        opt There is Lab
            S->>L: reserveBench(student)
        end  
```
## Exercise 4- alt

```mermaid
    sequenceDiagram
        actor User
        participant S as service: EnrollmentService
        participant C as course: Course
        participant W as waitlist: Waitlist
        User->>S: enroll(student, course)
        S->>C: hasSeatLeft()
        C-->>S: seatLeft?
        alt Seat Left
            S->>C: addStudent(student)
            S-->>User: "enrolled"
        else no seat left
            S->>W: add(student)
            S-->>User: "waitlisted"
        end
```
## Exercise 5 - loop

```mermaid
    sequenceDiagram
        participant T as transcript: Transcript
        participant E as enrollment: Enrollment
        loop for Each Enrollment
            T->>E: getGrade()
            E-->>T: grade
            T->>E: getCredits()
            E-->>T: credits
        end

```
# Exercise 6 - break

```mermaid
    sequenceDiagram
        actor User
        participant S as service: EnrollmentService
        participant C as course: Course
        participant St as student: UndergraduateStudent
        User->>S: enroll(student, course)
        S->>C: getPrerequisite()
        C-->>S: prereq
        S->>St: hasPassed(prereq)
        St-->>S: passed?
        break student hasn't pass prereq
        S-->>User: "missing prerequisite"
        end
        S->>C: isOpen()
        C-->>S: open?

```
# Exercise 7 - par

```mermaid
    sequenceDiagram
        participant G as grades: GradeService
        participant St as student: Student
        participant A as advisor: Advisor
        par notify student
            G-)St: notify(grade)
        and notify advisor
            G-)A: notify(grade)
        end
       

```
# Exercise 8

Sentence from the requirements|Fragment|
|---|---|
|"For each course in the student's list, …"|loop|
|"If the student asked for a printed timetable, also print it."|opt|
|"The student pays the fees either by card or by bank transfer."|alt|
|"If registration is closed, refuse and stop."|break|
|"Email the student and the advisor at the same time."|par|

# Exercise 9
 ```mermaid
        sequenceDiagram
            actor Registrar
            participant S as service: WaitlistService 
            participant C as course: Course
            participant W as waitlist: Waitlist
            Registrar->>S: promote(course, waitlist) 
            S->>C: hasSeatLeft()
            C-->S: seatLeft?       
            break no seat left
                 S-->>Registrar: undefined   
            end
            S->>W: next()
            W-->>S: next
            break next is undefined
                 S-->>Registrar: undefined
            end
            S->>C: addStudent(next)
            S-->>Registrar: next

``` 
# Exercise 10

```typescript ...
enrollAll(student: Student, courses: Course[]): number {
      let acceptedCount = 0;
       for (const course of courses) {
        if (course.isOpen()) {
            const credits = course.getCredits();
            if (student.addCredits(credits)) {
            acceptedCount++;
          }
            }
      }
      return acceptedCount;
    }
```
#   Exercise 11

|\#|Line of the diagram|What is wrong|Correction|
|---|---|---|---|
|1|6|line 6 should come after isOpen()|S->>C: isOpen()
S->>E: new Enrollment(student, course)|
|2|8|C->>S wrong arrow used|C-->>S|
|3|10|S->>C: seats = seats - 1 direct method assumption|S->>C: addStudent(Student)|


