#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <semaphore.h>
#include <unistd.h>
#include <time.h>

sem_t *forks;               // Array of fork semaphores
pthread_mutex_t mutex;      // Mutex to ensure mutual exclusion while eating
int num_philosophers;

void* philosopher(void* arg) {
    int id = *(int*)arg;
    free(arg);  // Free the dynamically allocated memory for the ID

    int left = id;
    int right = (id + 1) % num_philosophers;

    while (1) {
        printf("Philosopher %d is thinking...\n", id);
        sleep(rand() % 3 + 1); // Simulate thinking

        pthread_mutex_lock(&mutex);  // Ensure only one eats at a time

        // Pick up forks in a specific order to avoid deadlock
        if (left < right) {
            sem_wait(&forks[left]);
            sem_wait(&forks[right]);
        } else {
            sem_wait(&forks[right]);
            sem_wait(&forks[left]);
        }

        printf("Philosopher %d is eating...\n", id);
        sleep(rand() % 2 + 1); // Simulate eating

        // Release forks
        sem_post(&forks[left]);
        sem_post(&forks[right]);

        pthread_mutex_unlock(&mutex); // Allow others to eat

        printf("Philosopher %d finished eating and starts thinking again.\n", id);
    }

    return NULL;
}

int main() {
    srand(time(NULL));

    printf("Enter number of philosophers: ");
    scanf("%d", &num_philosophers);

    if (num_philosophers < 2) {
        printf("Need at least 2 philosophers to simulate.\n");
        return 1;
    }

    pthread_t *threads = malloc(sizeof(pthread_t) * num_philosophers);
    forks = malloc(sizeof(sem_t) * num_philosophers);

    // Initialize semaphores for forks
    for (int i = 0; i < num_philosophers; i++) {
        sem_init(&forks[i], 0, 1);
    }

    pthread_mutex_init(&mutex, NULL); // Initialize the mutex

    // Create philosopher threads
    for (int i = 0; i < num_philosophers; i++) {
        int* id = malloc(sizeof(int)); // Allocate memory for each philosopher's ID
        *id = i;
        pthread_create(&threads[i], NULL, philosopher, id);
    }

    // Join threads (will not return unless threads are terminated)
    for (int i = 0; i < num_philosophers; i++) {
        pthread_join(threads[i], NULL);
    }

    // Cleanup (won't be reached due to infinite loop in threads)
    for (int i = 0; i < num_philosophers; i++) {
        sem_destroy(&forks[i]);
    }
    pthread_mutex_destroy(&mutex);
    free(forks);
    free(threads);

    return 0;
}
