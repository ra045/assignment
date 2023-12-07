#include <stdio.h>
#include <stdlib.h>

#define MAX_SIZE 5

// Circular Queue Structure
typedef struct {
    int front, rear;
    int size;
    int *array;
} CircularQueue;
//reading number to queue
int read_num(int num)
{
	printf("enter number to be added to queue: ");
	scanf("%d",&num);
	return num;
}
// Initialize a Circular Queue
CircularQueue* initializeQueue() {
    CircularQueue* queue = (CircularQueue*)malloc(sizeof(CircularQueue));
    queue->front = queue->rear = -1;
    queue->size = MAX_SIZE;
    queue->array = (int*)malloc(MAX_SIZE * sizeof(int));
    return queue;
}

// Check if the queue is empty
int isEmpty(CircularQueue* queue) {
    return (queue->front == -1);
}

// Check if the queue is full
int isFull(CircularQueue* queue) {
    return ((queue->rear + 1) % queue->size == queue->front);
}

// Function to write data to the queue
void writeQueue(CircularQueue* queue, int data) {
    if (isFull(queue)) {
        // Queue is full, overwrite oldest data
        queue->front = (queue->front + 1) % queue->size;
    }

    // Move the rear to the next position
    queue->rear = (queue->rear + 1) % queue->size;

    // Add data to the rear position
    queue->array[queue->rear] = data;

    // If the queue was empty, set front to the new rear
    if (isEmpty(queue)) {
        queue->front = queue->rear;
    }
}

// Function to read data from the queue
int readQueue(CircularQueue* queue) {
    int data = -1;

    // Check if the queue is empty
    if (!isEmpty(queue)) {
        // Get data from the front
        data = queue->array[queue->front];

        // If there is only one element in the queue, reset front and rear
        if (queue->front == queue->rear) {
            queue->front = queue->rear = -1;
        } else {
            // Move the front to the next position
            queue->front = (queue->front + 1) % queue->size;
        }
    }

    return data;
}

// Function to clear the queue
void clearQueue(CircularQueue* queue) {
    // Reset front and rear to -1
    queue->front = queue->rear = -1;
}

// Function to print the elements of the queue
void printQueue(CircularQueue* queue) {
    if (isEmpty(queue)) {
        printf("Queue is empty.\n");
        return;
    }

    printf("Queue elements: ");
    int i = queue->front;
    do {
        printf("%d ", queue->array[i]);
        i = (i + 1) % queue->size;
    } while (i != (queue->rear + 1) % queue->size);
    printf("\n");
}

// Driver program for testing
int main() {
    CircularQueue* queue = initializeQueue();
    int num;
    // Writing to the queue
    writeQueue(queue, read_num(num));
    writeQueue(queue, read_num(num));
    writeQueue(queue, read_num(num));
    writeQueue(queue, read_num(num));
    writeQueue(queue, read_num(num));
    printQueue(queue);

    // Reading from the queue
    int data = readQueue(queue);
    printf("Read from queue: %d\n", data);
    printQueue(queue);

    // Writing to the queue again
    writeQueue(queue, read_num(num));
    writeQueue(queue, read_num(num));
    writeQueue(queue, read_num(num));
    printQueue(queue);

    // Clearing the queue
    clearQueue(queue); 
    printQueue(queue);

    free(queue->array);
    free(queue);

    return 0;
}
