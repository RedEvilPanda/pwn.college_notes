## Creating a child process and passing arguments with environment variables

```C
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <sys/prctl.h>
#include <fcntl.h>

void pwncollege(){}
int main()
{
	printf("[~] Parent process [pid: %d]\n", getpid());
	pid_t pid; int rv;
	switch(pid = fork()) // FORK - creates a child process
	{
	case -1: // Error creating process
		fprintf(stderr, "[!] Process error!");
		exit(1);
	
	case 0: // Process created successfully
		prctl(PR_SET_NAME, (unsigned long) "pwncollege");
		printf("[+] Child process started [pid: %d]\n", getpid()); 
		
		char *args[2]; // Process name and arguments
		args[0] = "/challenge/embryoio_level34";
		args[1] = NULL;
		
		char *envp[2]; // Environment Variables
		envp[0] = "dhaboa=xzwzjzgvob";
		envp[1] = NULL;
		
		execve(args[0], args, envp); // Initialize the program in a subprocess
		exit(rv);
		
	default: // Return to parent process
		printf("[+] Wait when child proces stop...\n");
		waitpid(pid, NULL, 0);
		exit(0);
	}
	
	return 0;
}

```


## Creating a child process and passing STDIN data to it as a stream from a file
```C
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <sys/prctl.h>
#include <fcntl.h>

void pwncollege(){}
int main()
{
	printf("[~] Parent process [pid: %d]\n", getpid());
	pid_t pid; int rv;
	int fileds = open("/tmp/ivyyzs", O_RDONLY); // Open the file for reading
	
	switch(pid = fork())
	{
	case -1:
		fprintf(stderr, "[!] Process error!");
		exit(1);
	
	case 0:
		prctl(PR_SET_NAME, (unsigned long) "pwncollege");
		printf("[+] Child process started [pid: %d]\n", getpid());
		
		char *args[2];
		args[0] = "/challenge/embryoio_level35";
		args[1] = NULL;
		
		dup2(fileds, STDIN_FILENO);// Change the file descriptor to STDIN
		close(fileds);
		execve(args[0], args, NULL);
		exit(rv);
		
	default:
		printf("[+] Wait when child proces stop...\n");
		waitpid(pid, NULL, 0);
		exit(0);
	}
	
	return 0;
}

// => ./embryoio_level < /tmp/ivzyyzs
```


## Passing data via PIPE between two processes

```C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

void pwncollege(){}
int main()
{
	int filesDesctriptor[2];
	pid_t pid_embryo, pid_cat;
	pipe(filesDesctriptor);
	
	pid_cat = fork();
	if (pid_cat == 0)
	{
		char *args[2];
		args[0] = "/challenge/embryoio_level60";
		args[1] = NULL;
		
		dup2(filesDesctriptor[1], STDOUT_FILENO);
		close(filesDesctriptor[0]);
		close(filesDesctriptor[1]);
		execve(args[0], args, NULL);
	}
	
	pid_embryo = fork();
	if (pid_embryo == 0)
	{
		char *args[2];
		args[0] = "/usr/bin/cat";
		args[1] = NULL;
		
		dup2(filesDesctriptor[0], STDIN_FILENO);
		close(filesDesctriptor[0]);
		close(filesDesctriptor[1]);
		execve(args[0], args, NULL);
	}
	
	waitpid(pid_cat, NULL, 0);
	waitpid(pid_embryo,NULL, 0);
	
	return 0;
}

// => ./embryoio_level60 | cat 
```

