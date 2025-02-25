```C
#include <stdio.h>
#include <stdlib.h>

/* 
 * Struct Definition: Student
 * Represents a student with:
 * - `number` (Student ID)
 * - `grade` (Student Grade)
 */
typedef struct Student {
    int number;
    int grade;
} Student;

/* Function Prototypes */
Student* CreateStudent();        // Function to allocate memory for a new student
void DestroyStudent(Student* student);  // Function to free memory
void SetNumber(Student* student, int number);  // Function to set student ID
void SetGrade(Student* student, int grade);  // Function to set student grade
void Print(Student* student);    // Function to print student details

/* Function Implementations */

/* 
 * CreateStudent: Allocates memory for a new Student and returns a pointer.
 * If memory allocation fails, it prints an error and exits the program.
 */
Student* CreateStudent() {
    Student* self = (Student*)malloc(sizeof(Student)); // Allocate memory for a Student
    if (!self) {  // Check if allocation failed
        printf("Memory allocation failed!\n");
        exit(1);  // Exit program if allocation fails
    }
    return self;
}

/* 
 * DestroyStudent: Frees the memory allocated for a Student object.
 * Prevents memory leaks by checking if the pointer is NULL before freeing.
 */
void DestroyStudent(Student* student) {
    if (!student) return;  // If student is NULL, do nothing
    free(student);  // Free allocated memory
}

/* 
 * SetNumber: Assigns a student number to a Student object.
 * Checks if the student pointer is NULL before accessing memory.
 */
void SetNumber(Student* student, int number) {
    if (!student) return;  // Ensure student is valid before accessing memory
    student->number = number;
}

/* 
 * SetGrade: Assigns a grade to a Student object.
 * Ensures the student pointer is not NULL before modifying data.
 */
void SetGrade(Student* student, int grade) {
    if (!student) return;  // Prevents segmentation faults on NULL pointers
    student->grade = grade;
}

/* 
 * Print: Displays the student's information (ID and grade).
 * Ensures the student pointer is valid before accessing data.
 */
void Print(Student* student) {
    if (!student) return;  // Check if the pointer is valid
    printf("Student number: %d, Grade: %d\n", student->number, student->grade);
}

/* 
 * Main function: Demonstrates usage of Student struct and functions.
 */
int main() {
    /* Create two student instances */
    Student* stu1 = CreateStudent();
    Student* stu2 = CreateStudent();

    /* Set values for the students */
    SetNumber(stu1, 11);
    SetNumber(stu2, 22);
    SetGrade(stu1, 111);
    SetGrade(stu2, 222);

    /* Print student details */
    Print(stu1);
    Print(stu2);

    /* Free allocated memory to prevent memory leaks */
    DestroyStudent(stu1);
    DestroyStudent(stu2);

    return 0;  // Indicate successful execution
}

```

