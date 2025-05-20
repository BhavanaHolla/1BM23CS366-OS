#include <stdio.h>

int search(int frames[], int f, int key) {
    for (int i = 0; i < f; i++)
        if (frames[i] == key) return 1;
    return 0;
}

void fifo(int pages[], int n, int f) {
    int frames[10], faults = 0, index = 0;

    for (int i = 0; i < f; i++) frames[i] = -1;

    for (int i = 0; i < n; i++) {
        if (!search(frames, f, pages[i])) {
            frames[index] = pages[i];
            index = (index + 1) % f;
            faults++;
        }
    }

    printf("FIFO Faults: %d\n", faults);
}

void optimal(int pages[], int n, int f) {
    int frames[10], faults = 0;

    for (int i = 0; i < f; i++) frames[i] = -1;

    for (int i = 0; i < n; i++) {
        if (!search(frames, f, pages[i])) {
            int farthest = -1, index = -1;
            for (int j = 0; j < f; j++) {
                int k;
                for (k = i + 1; k < n; k++)
                    if (frames[j] == pages[k]) break;
                if (k > farthest) {
                    farthest = k;
                    index = j;
                }
                if (k == n) {
                    index = j;
                    break;
                }
            }
            if (index == -1) index = 0;
            frames[index] = pages[i];
            faults++;
        }
    }

    printf("Optimal Faults: %d\n", faults);
}


void lru(int pages[], int n, int f) {
    int frames[10], used[10], faults = 0;

    // Initialize frames and usage times
    for (int i = 0; i < f; i++) {
        frames[i] = -1;
        used[i] = -1;
    }

    for (int i = 0; i < n; i++) {
        int found = 0;

        // Check if page already exists
        for (int j = 0; j < f; j++) {
            if (frames[j] == pages[i]) {
                used[j] = i;  // update last used time
                found = 1;
                break;
            }
        }

        // Page fault occurs
        if (!found) {
            int replace = -1;

            // Look for an empty frame
            for (int j = 0; j < f; j++) {
                if (frames[j] == -1) {
                    replace = j;
                    break;
                }
            }

            // If no empty frame, find LRU page
            if (replace == -1) {
                replace = 0;
                for (int j = 1; j < f; j++) {
                    if (used[j] < used[replace])
                        replace = j;
                }
            }

            frames[replace] = pages[i];
            used[replace] = i;
            faults++;
        }
    }

    printf("LRU Faults: %d\n", faults);
}

int main() {
    int pages[100], n, f;

    printf("Enter number of page references: ");
    scanf("%d", &n);
    printf("Enter page reference string:\n");
    for (int i = 0; i < n; i++) scanf("%d", &pages[i]);
    printf("Enter number of frames: ");
    scanf("%d", &f);

    fifo(pages, n, f);
    optimal(pages, n, f);
    lru(pages, n, f);

    return 0;
}
