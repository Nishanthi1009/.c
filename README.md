#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Node structure for doubly linked list
struct Node {
    char filename[50];
    struct Node* prev;
    struct Node* next;
};

// Function to create a new node
struct Node* createNode(char* filename) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    strcpy(newNode->filename, filename);
    newNode->prev = NULL;
    newNode->next = NULL;
    return newNode;
}

// Function to insert a node at the end of the list
void insertEnd(struct Node** head, char* filename) {
    struct Node* newNode = createNode(filename);
    if (*head == NULL) {
        *head = newNode;
        return;
    }
    struct Node* temp = *head;
    while (temp->next != NULL) {
        temp = temp->next;
    }
    temp->next = newNode;
    newNode->prev = temp;
}

// Function to perform alphabetical sorting of filenames
void sortFiles(struct Node** head) {
    struct Node *current, *index;
    char temp[50];
    if (*head == NULL) {
        return;
    }
    for (current = *head; current->next != NULL; current = current->next) {
        for (index = current->next; index != NULL; index = index->next) {
            if (strcmp(current->filename, index->filename) > 0) {
                strcpy(temp, current->filename);
                strcpy(current->filename, index->filename);
                strcpy(index->filename, temp);
            }
        }
    }
}

// Function to display the sorted list
void display(struct Node* head) {
    if (head == NULL) {
        printf("List is empty\n");
        return;
    }
    printf("Sorted filenames:\n");
    while (head != NULL) {
        printf("%s\n", head->filename);
        head = head->next;
    }
}

// Main function
int main() {
    struct Node* head = NULL;
    int n;
    
    printf("Document auto sorter\n");
    printf("Step1: Enter the number of files\n");
    printf("Step2: Enter the file names\n");
    printf("Step3: Sorting the files\n");
    printf("Step4: Exit\n");
    
    printf("Enter the number of files: ");
    scanf("%d", &n);
    getchar(); // Consume newline character
    
    for (int i = 0; i < n; i++) {
        char filename[50];
        printf("Enter filename %d: ", i + 1);
        fgets(filename, sizeof(filename), stdin);
        filename[strcspn(filename, "\n")] = '\0'; // Remove newline character
        insertEnd(&head, filename);
    }
    
    sortFiles(&head);
    display(head);
    
    printf("----Exit----\n");
    return 0;
}
