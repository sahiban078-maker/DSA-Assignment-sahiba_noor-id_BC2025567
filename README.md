#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct Student {
    int roll;
    char name[30];
    float marks;
    struct Student *next;
} Student;

Student *head = NULL;

// Create a new node
Student* create(int roll, char name[], float marks) {
    Student *p = malloc(sizeof(Student));
    p->roll = roll;
    strcpy(p->name, name);
    p->marks = marks;
    p->next = NULL;
    return p;
}

// Check duplicate roll number
int exists(int roll) {
    Student *p = head;
    while (p) {
        if (p->roll == roll)
            return 1;
        p = p->next;
    }
    return 0;
}

// Add student
void addStudent() {
    int roll;
    char name[30];
    float marks;

    printf("\nEnter Roll No: ");
    scanf("%d", &roll);

    if (exists(roll)) {
        printf("Roll number already exists!\n");
        return;
    }

    printf("Enter Name: ");
    scanf(" %[^\n]", name);

    printf("Enter Marks: ");
    scanf("%f", &marks);

    Student *p = create(roll, name, marks);

    if (head == NULL)
        head = p;
    else {
        Student *temp = head;
        while (temp->next)
            temp = temp->next;
        temp->next = p;
    }

    printf("Student added successfully!\n");
}

// Display students
void display() {
    Student *p = head;

    if (!p) {
        printf("\nNo records found!\n");
        return;
    }

    printf("\n-----------------------------------------\n");
    printf("Roll\tName\t\tMarks\tGrade\n");
    printf("-----------------------------------------\n");

    while (p) {
        char grade;

        if (p->marks >= 90)
            grade = 'A';
        else if (p->marks >= 75)
            grade = 'B';
        else if (p->marks >= 60)
            grade = 'C';
        else if (p->marks >= 40)
            grade = 'D';
        else
            grade = 'F';

        printf("%d\t%-15s %.2f\t%c\n",
               p->roll, p->name, p->marks, grade);

        p = p->next;
    }
}

// Search student
void search() {
    int roll;
    printf("\nEnter Roll No to search: ");
    scanf("%d", &roll);

    Student *p = head;

    while (p) {
        if (p->roll == roll) {
            printf("\nStudent Found!\n");
            printf("Roll  : %d\n", p->roll);
            printf("Name  : %s\n", p->name);
            printf("Marks : %.2f\n", p->marks);
            return;
        }
        p = p->next;
    }

    printf("Student not found!\n");
}

// Update student
void update() {
    int roll;
    printf("\nEnter Roll No: ");
    scanf("%d", &roll);

    Student *p = head;

    while (p) {
        if (p->roll == roll) {
            printf("Enter New Name: ");
            scanf(" %[^\n]", p->name);

            printf("Enter New Marks: ");
            scanf("%f", &p->marks);

            printf("Record updated!\n");
            return;
        }
        p = p->next;
    }

    printf("Student not found!\n");
}

// Delete student
void deleteStudent() {
    int roll;
    printf("\nEnter Roll No to delete: ");
    scanf("%d", &roll);

    Student *p = head, *prev = NULL;

    while (p) {
        if (p->roll == roll) {
            if (prev == NULL)
                head = p->next;
            else
                prev->next = p->next;

            free(p);
            printf("Student deleted successfully!\n");
            return;
        }

        prev = p;
        p = p->next;
    }

    printf("Student not found!\n");
}

// Sort students by marks
void sortMarks() {
    Student *i, *j;
    int roll;
    float marks;
    char name[30];

    for (i = head; i; i = i->next) {
        for (j = i->next; j; j = j->next) {

            if (i->marks < j->marks) {
                roll = i->roll;
                i->roll = j->roll;
                j->roll = roll;

                strcpy(name, i->name);
                strcpy(i->name, j->name);
                strcpy(j->name, name);

                marks = i->marks;
                i->marks = j->marks;
                j->marks = marks;
            }
        }
    }

    printf("Students sorted by marks!\n");
}

// Save records
void save() {
    FILE *fp = fopen("students.txt", "w");
    Student *p = head;

    if (!fp) {
        printf("File error!\n");
        return;
    }

    while (p) {
        fprintf(fp, "%d %s %.2f\n",
                p->roll, p->name, p->marks);
        p = p->next;
    }

    fclose(fp);
    printf("Records saved!\n");
}

// Main function
int main() {
    int choice;

    do {
        printf("\n\n===== STUDENT MANAGEMENT SYSTEM =====\n");
        printf("1. Add Student\n");
        printf("2. Display Students\n");
        printf("3. Search Student\n");
        printf("4. Update Student\n");
        printf("5. Delete Student\n");
        printf("6. Sort By Marks\n");
        printf("7. Save Records\n");
        printf("8. Exit\n");
        printf("Enter Choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: addStudent(); break;
            case 2: display(); break;
            case 3: search(); break;
            case 4: update(); break;
            case 5: deleteStudent(); break;
            case 6: sortMarks(); break;
            case 7: save(); break;
            case 8: printf("Thank You!\n"); break;
            default: printf("Invalid choice!\n");
        }

    } while (choice != 8);

    return 0;
}
