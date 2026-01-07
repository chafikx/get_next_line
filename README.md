*This project has been created as part of the 42 curriculum by chbenhiz.*

# Get Next Line

## Description
The aim of this project is to create a function that returns a line read from a file descriptor.
It allows reading a text file (or standard input) line by line, regardless of the buffer size. This project is an introduction to **static variables** in C and helps understand memory management and file manipulation (`read`, `malloc`, `free`).

## Instructions

### How to use
To use the function in your code, simply include the header:

    #include "get_next_line.h"

### Compilation
You need to compile the source files (`get_next_line.c`, `get_next_line_utils.c`) with your main program. The buffer size can be defined at compilation time.

Example:

    cc -Wall -Wextra -Werror -D BUFFER_SIZE=42 get_next_line.c get_next_line_utils.c main.c -o gnl

### Execution
Run the executable with a file:

    ./gnl file.txt

## Algorithm & Justification

The main challenge is that `read()` grabs a fixed amount of bytes (`BUFFER_SIZE`), which rarely aligns with line breaks. To solve this, I used a **static variable** (`stash`) to store data between function calls.

The logic follows three steps:

1.  **Read & Stash**: The function reads the file in chunks and appends them to the static stash. It continues loop-reading until a newline (`\n`) is found or the end of the file is reached.
    * *Justification*: Storing data in a static variable prevents data loss between calls, as local variables are destroyed when the function returns.

2.  **Extract Line**: Once a newline is present, the function extracts the substring from the start of the stash up to the newline character. This is the string returned to the user.

3.  **Clean Up**: The remaining part of the buffer (everything after the newline) is saved back into the static variable. The old buffer is freed.
    * *Justification*: This step is crucial to ensure the next call to `get_next_line` starts exactly where the previous one stopped.

**Memory Safety**: The function checks for allocation failures at every step. If `malloc` returns `NULL`, the function frees any allocated memory (including the stash) to prevent leaks and returns `NULL`.

## Resources

### Documentation
* `man 2 read`
* `man 3 malloc` / `free`

### AI Usage
As required by the subject, AI (Gemini) was used during this project to:
* Clarify the concept and behavior of static variables.
* Debug segmentation faults caused by specific edge cases (e.g., handling `malloc` failures simulated by testers).
* Optimize the memory cleaning process in the `ft_strjoin` function.