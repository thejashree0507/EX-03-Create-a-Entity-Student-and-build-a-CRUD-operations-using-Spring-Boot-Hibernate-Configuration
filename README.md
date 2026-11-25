# EXp_03_-Entity-Student-and-build-a-CRUD-operations-using-Spring-Boot-Hibernate-Configuration

## AIM:
To develop a Spring Boot application that performs CRUD (Create, Read, Update, Delete) operations on a Student entity using Spring Data JPA (Hibernate).

## ALGORITHM:

1\. Start the process.

2\. Create a new Spring Boot project using IntelliJ IDEA or Spring Initializr.

3\. Add the required dependencies: Spring Web, Spring Data JPA, H2 Database or MySQL Driver, and Spring Boot DevTools.

4\. Open the `application.properties` file and configure the database connection details.

5\. Enable Hibernate automatic table creation or updating using `spring.jpa.hibernate.ddl-auto=update`.

6\. Create a `Student` entity class and annotate it with `@Entity`.

7\. Define the student fields such as `id`, `name`, `department`, and `age`.

8\. Use `@Id` and `@GeneratedValue` annotations to define the primary key.

9\. Create a `StudentRepository` interface and extend `JpaRepository<Student, Long>` to perform CRUD operations.

10\. Create a `StudentController` class and annotate it with `@RestController`.

11\. Map the controller to the `/students` endpoint using `@RequestMapping`.

12\. Implement the following REST API operations:

- **POST&#x20;**`/students` – Add a new student.
- **GET&#x20;**`/students` – Retrieve all students.
- **GET&#x20;**`/students/{id}` – Retrieve a student by ID.
- **PUT&#x20;**`/students/{id}` – Update student details.
- **DELETE&#x20;**`/students/{id}` – Delete a student by ID.

13\. Run the Spring Boot application and verify that it starts successfully.

14\. Open Postman and test all the CRUD operations using the corresponding HTTP methods and URLs.

15\. Verify the inserted, retrieved, updated, and deleted student records in the database.

16\. Stop the application.

17\. End the process.

## Program:
### pom.xml
```
<dependencies>
    <!-- Spring Boot Web -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Spring Boot JPA -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- H2 Database (In-memory) -->
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```
 ### application.properties
```
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.hibernate.ddl-auto=update
spring.h2.console.enabled=true
```

### Student.java
```java
package com.example.studentcrud.model;

import jakarta.persistence.*;

@Entity
public class Student {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String department;
    private int age;
    public Long getId() {
        return id;
    }
    public void setId(Long id) {
        this.id = id;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public String getDepartment() {
        return department;
    }
    public void setDepartment(String department) {
        this.department = department;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
}
```
### StudentRepository.java
```java
package com.example.studentcrud.repository;

import com.example.studentcrud.model.Student;
import org.springframework.data.jpa.repository.JpaRepository;

public interface StudentRepository extends JpaRepository<Student, Long> {

}
```
### StudentController.java
```java
package com.example.studentcrud.controller;

import com.example.studentcrud.model.Student;
import com.example.studentcrud.repository.StudentRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/students")
public class StudentController {

        @Autowired
        private StudentRepository studentRepository;

        // CREATE
        @PostMapping
        public Student addStudent(@RequestBody Student student) {
            return studentRepository.save(student);
        }

        // READ ALL
        @GetMapping
        public List<Student> getAllStudents() {
            return studentRepository.findAll();
        }

        // READ BY ID
        @GetMapping("/{id}")
        public Student getStudent(@PathVariable Long id) {
            return studentRepository.findById(id)
                    .orElseThrow();
        }

        // UPDATE
        @PutMapping("/{id}")
        public Student updateStudent(
                @PathVariable Long id,
                @RequestBody Student studentDetails) {

            Student student = studentRepository.findById(id)
                    .orElseThrow();

            student.setName(studentDetails.getName());
            student.setDepartment(studentDetails.getDepartment());
            student.setAge(studentDetails.getAge());

            return studentRepository.save(student);
        }

        // DELETE
        @DeleteMapping("/{id}")
        public String deleteStudent(@PathVariable Long id) {

            studentRepository.deleteById(id);

            return "Student with ID " + id +
                    " deleted successfully!";
        }
    }

```
### StudentCRUDApplication.java
```java
package com.example.studentcrud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class StudentCrudApplication {

    public static void main(String[] args) {

        SpringApplication.run(StudentCrudApplication.class, args);
    }

}
```
## Output
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/7e8993f1-b6d7-4b75-bd2a-db5dd93e3e1b" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/dd0bd66a-11c6-4a6e-a6ce-8216ffa3b588" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/f7129406-e5fb-4a52-bf8c-b9d48272a4e1" />

## Result
Thus, the Student CRUD application was successfully developed using Spring Boot, Hibernate, Spring Data JPA, and H2 Database. The Create, Read, Update, and Delete operations were implemented and tested successfully using Postman.
