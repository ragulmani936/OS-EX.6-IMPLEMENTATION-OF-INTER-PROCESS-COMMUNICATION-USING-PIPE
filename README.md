# OS-EX.6-IMPLEMENTATION-OF-INTER-PROCESS-COMMUNICATION-USING-PIPE

## AIM:
To Write C program to illustrate IPC using pipes mechanisms.

## ALGORITHM:
### Step 1:
Create a pipe (pipe(fd)).
### Step 2:
Fork a child process (child_pid = fork()).
### Step 3:
In the child process:
    1. Close the read end of the pipe (close(fd[0])).
    2. Write a message to the pipe (write(fd[1], message, strlen(message) + 1)).
    3. Close the write end of the pipe (close(fd[1])).
    4. Exit the child process (exit(EXIT_SUCCESS)).
### Step 4:
In the parent process:
    1. Close the write end of the pipe (close(fd[1])).
    2. Read a message from the pipe (read(fd[0], received_message, sizeof(received_message))).
    3. Close the read end of the pipe (close(fd[0])).
    4. Print the received message (printf("Parent received: %s\n", received_message)).
    5. Parent process continues execution.
### Step 6:
End of the program (return 0 in the parent process).

## PROGRAM:
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>

int main() {
    int fd[2];
    pid_t child_pid;
    char message[100];

    printf("Enter a message: ");
    fgets(message, sizeof(message), stdin);

    if (pipe(fd) == -1) {
        perror("Pipe creation failed");
        exit(EXIT_FAILURE);
    }

    child_pid = fork();

    if (child_pid == -1) {
        perror("Fork failed");
        exit(EXIT_FAILURE);
    }

    if (child_pid == 0) {
        close(fd[0]);
        write(fd[1], message, strlen(message) + 1);
        close(fd[1]);
        exit(EXIT_SUCCESS);
    } else {
        char received_message[100];
        close(fd[1]);
        read(fd[0], received_message, sizeof(received_message));
        close(fd[0]);
        printf("Parent received: %s\n", received_message);
    }

    return 0;
}


```



## OUTPUT:
![](1.png)
## Result:
This output confirms that the parent process successfully received and printed the message sent by the child process through the pipe, demonstrating inter-process communication using pipes.


