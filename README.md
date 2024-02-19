
# Data-management-
GitHub repo: Basic C program for a database management system. Features include record insertion, display, search, and deletion. Clear menu-driven interface. Suitable for educational use or as a foundation for more complex projects.


#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_DATABASE_SIZE 100

struct Record {
    char key[20];
    char value[50];
};

struct Database {
    struct Record data[MAX_DATABASE_SIZE];
    int size;
};

void initializeDatabase(struct Database *db) {
    db->size = 0;
}

void insertRecord(struct Database *db, const char *key, const char *value) {
    if (db->size < MAX_DATABASE_SIZE) {
        strcpy(db->data[db->size].key, key);
        strcpy(db->data[db->size].value, value);
        db->size++;
        printf("Record inserted successfully.\n");
    } else {
        printf("Database is full. Cannot insert more records.\n");
    }
}

void displayDatabase(struct Database *db) {
    printf("\nDatabase Records:\n");
    for (int i = 0; i < db->size; i++) {
        printf("Key: %s, Value: %s\n", db->data[i].key, db->data[i].value);
    }
}

int searchRecord(struct Database *db, const char *key) {
    for (int i = 0; i < db->size; i++) {
        if (strcmp(db->data[i].key, key) == 0) {
            return i; // Record found, return index
        }
    }
    return -1; // Record not found
}

void deleteRecord(struct Database *db, const char *key) {
    int index = searchRecord(db, key);
    if (index != -1) {
        for (int i = index; i < db->size - 1; i++) {
            db->data[i] = db->data[i + 1];
        }
        db->size--;
        printf("Record deleted successfully.\n");
    } else {
        printf("Record not found.\n");
    }
}

int main() {
    struct Database myDatabase;
    initializeDatabase(&myDatabase);

    int choice;
    char key[20], value[50];

    do {
        printf("\nDatabase Management System\n");
        printf("1. Insert Record\n");
        printf("2. Display Database\n");
        printf("3. Search Record\n");
        printf("4. Delete Record\n");
        printf("5. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter key: ");
                scanf("%s", key);
                printf("Enter value: ");
                scanf(" %[^\n]s", value);
                insertRecord(&myDatabase, key, value);
                break;
            case 2:
                displayDatabase(&myDatabase);
                break;
            case 3:
                printf("Enter key to search: ");
                scanf("%s", key);
                int index = searchRecord(&myDatabase, key);
                if (index != -1) {
                    printf("Record found at index %d.\n", index);
                } else {
                    printf("Record not found.\n");
                }
                break;
            case 4:
                printf("Enter key to delete: ");
                scanf("%s", key);
                deleteRecord(&myDatabase, key);
                break;
            case 5:
                printf("Exiting Database Management System. Goodbye!\n");
                break;
            default:
                printf("Invalid choice. Please enter a valid option.\n");
        }
    } while (choice != 5);

    return 0;
}
