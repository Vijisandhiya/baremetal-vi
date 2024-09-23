#include <stdio.h>

const int HEAP_SIZE = 128 * 1024;  // 128 KB
const int BLOCK_SIZE = 1024;       // 1 KB
const int NUM_BLOCKS = HEAP_SIZE / BLOCK_SIZE;  

char heap[HEAP_SIZE];
int block_map[NUM_BLOCKS];

void initialize_heap() {
    int i;
    for (i = 0; i < NUM_BLOCKS; i++) {
        block_map[i] = 0; 
    }
}

int align_size(int size) {
    int remainder = size % BLOCK_SIZE;
    if (remainder == 0) {
        return size;
    } else {
        return size + BLOCK_SIZE - remainder;
    }
}

void* my_malloc(int size) {
    int aligned_size = align_size(size);  
    int num_blocks_needed = aligned_size / BLOCK_SIZE;

    int i, j;
    int best_fit_index = -1;
    int best_fit_size = NUM_BLOCKS + 1;

    for (i = 0; i < NUM_BLOCKS; i++) {
        if (block_map[i] == 1) {
            continue;
        }

        int free_block_count = 0;
        for (j = i; j < NUM_BLOCKS && block_map[j] == 0; j++) {
            free_block_count++;
            if (free_block_count == num_blocks_needed) {
                break;
            }
        }

        if (free_block_count >= num_blocks_needed && free_block_count < best_fit_size) {
            best_fit_index = i;
            best_fit_size = free_block_count;
        }

        i = j;  
    }

    if (best_fit_index == -1) {
        return 0;
    }

    for (i = best_fit_index; i < best_fit_index + num_blocks_needed; i++) {
        block_map[i] = 1;
    }

    return (void*)&heap[best_fit_index * BLOCK_SIZE];
}

void my_free(void* ptr) {
    int block_index = ((char*)ptr - heap) / BLOCK_SIZE;

    if (block_map[block_index] == 0) {
        printf("Error: Double free detected!\n");
        return;
    }

    while (block_index < NUM_BLOCKS && block_map[block_index] == 1) {
        block_map[block_index] = 0;
        block_index++;
    }
}

int main() {
    initialize_heap();  

    void* ptr1 = my_malloc(128);    
    void* ptr2 = my_malloc(1000);     
    void* ptr3 = my_malloc(128 * 8 * 1024);  

    printf("Allocated memory:\nPtr1: %p\nPtr2: %p\nPtr3: %p\n", ptr1, ptr2, ptr3);

    my_free(ptr1);
    my_free(ptr2);  
    my_free(ptr3);  

    return 0;
}
