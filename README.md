# file-system-organizer_c
A program that organizes files into directories based on type, date, or size, improving file management and reducing clutter. Project Goal of Implement a file system organizer using C, enabling users to efficiently manage and organize their files.
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <dirent.h>

int main() {
    DIR *dir;
    struct dirent *ent;
    char *file_types[] = {"txt", "jpg", "mp4", NULL};

    // Open the directory
    dir = opendir(".");
    if (dir == NULL) {
        perror("Error opening directory");
        return 1;
    }

    // Iterate through the files in the directory
    while ((ent = readdir(dir)) != NULL) {
        // Get the file extension
        char *ext = strrchr(ent->d_name, '.');
        if (ext != NULL) {
            ext++; // Move past the dot

            // Check if the file type is in the list
            for (int i = 0; file_types[i] != NULL; i++) {
                if (strcmp(ext, file_types[i]) == 0) {
                    // Move the file to the corresponding directory
                    char cmd[256];
                    sprintf(cmd, "mv %s %s/", ent->d_name, file_types[i]);
                    system(cmd);
                    break;
                }
            }
        }
    }

    // Close the directory
    closedir(dir);
    return 0;
}
