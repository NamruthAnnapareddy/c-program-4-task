# c-program-4-task
of codetech it solutions
#include <stdio.h>
#include <stdlib.h>

/* Function declarations */
void compressFile();
void decompressFile();

int main() {
    int choice;

    printf("---- RLE Data Compression Tool ----\n");
    printf("1. Compress File\n");
    printf("2. Decompress File\n");
    printf("Enter your choice: ");
    scanf("%d", &choice);

    switch (choice) {
        case 1:
            compressFile();
            break;
        case 2:
            decompressFile();
            break;
        default:
            printf("Invalid choice.\n");
    }

    return 0;
}

/* Compress file using Run-Length Encoding */
void compressFile() {
    FILE *input, *output;
    char current, previous;
    int count = 1;

    input = fopen("input.txt", "r");
    output = fopen("compressed.txt", "w");

    if (input == NULL || output == NULL) {
        printf("Error opening file.\n");
        exit(1);
    }

    previous = fgetc(input);

    while ((current = fgetc(input)) != EOF) {
        if (current == previous) {
            count++;
        } else {
            fprintf(output, "%c%d", previous, count);
            previous = current;
            count = 1;
        }
    }

    fprintf(output, "%c%d", previous, count);

    fclose(input);
    fclose(output);

    printf("File compressed successfully.\n");
}

/* Decompress file */
void decompressFile() {
    FILE *input, *output;
    char ch;
    int count;

    input = fopen("compressed.txt", "r");
    output = fopen("decompressed.txt", "w");

    if (input == NULL || output == NULL) {
        printf("Error opening file.\n");
        exit(1);
    }

    while ((ch = fgetc(input)) != EOF) {
        fscanf(input, "%d", &count);
        for (int i = 0; i < count; i++) {
            fputc(ch, output);
        }
    }

    fclose(input);
    fclose(output);

    printf("File decompressed successfully.\n");
}
input:-
AAAABBBCCDAA
output:-
AAAABBBCCDAA

