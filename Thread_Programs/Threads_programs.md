# 🧵 Thread Programs Collection



## `thread_programs/multiple_threads.c`

```c
#include <stdio.h>
#include <pthread.h>

#define NUM_THREADS 3

void* print_message(void* arg) 
{
    int thread_num = *(int*)arg;
    printf("Hello from Thread %d\n", thread_num);
    return NULL;
}

int main()
 {
    pthread_t threads[NUM_THREADS];
    int thread_ids[NUM_THREADS];

    for (int i = 0; i < NUM_THREADS; i++) 
    {
        thread_ids[i] = i + 1;
        pthread_create(&threads[i], NULL, print_message, &thread_ids[i]);
    }

    for (int i = 0; i < NUM_THREADS; i++) 
    {
        pthread_join(threads[i], NULL);
    }

    return 0;
}


```

## `thread_programs/race_condition.c`

```c
// Race condition (without using mutex)
/*
#include <stdio.h>
#include <pthread.h>

int counter = 0;  // Shared global variable

void* increment(void* arg) 
{
    for (int i = 0; i < 1000000; i++)
    {
        counter++;  // Not protected!
    }
    return NULL;
}

int main()
{
    pthread_t t1, t2;

    pthread_create(&t1, NULL, increment, NULL);
    pthread_create(&t2, NULL, increment, NULL);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    printf("Final counter value: %d\n", counter);
    return 0;
}
*/
// to reslove the race condition
#include <stdio.h>
#include <pthread.h>

int counter = 0;  // Shared global variable

pthread_mutex_t lock = PTHREAD_MUTEX_INITIALIZER;

void* increment(void* arg) 
{
    for (int i = 0; i < 1000000; i++)
    {
        pthread_mutex_lock(&lock);
        counter++;
        pthread_mutex_unlock(&lock);
    }
    return NULL;
}


int main() 
{
    pthread_t t1, t2;

    pthread_create(&t1, NULL, increment, NULL);
    pthread_create(&t2, NULL, increment, NULL);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    printf("Final counter value: %d\n", counter);
    return 0;
}

```

## `thread_programs/server.log`

```c
FILE *log_file = fopen("server.log", "a");
fprintf(log_file, "[%s] Client says: %s\n", time_str, buffer);
fclose(log_file);


```

## `thread_programs/t1.c`

```c
//Write a C program to create a thread that prints "Hello, World!
// File: t1.c
#include <stdio.h>
#include <pthread.h>
void* print_hello(void* arg) 
{
    printf("Hello, World!\n");
    return NULL;
}

int main()
{
    pthread_t thread_id;
    if (pthread_create(&thread_id, NULL, print_hello, NULL) != 0)
    {
        perror("Failed to create thread");
        return 1;
    }

    pthread_join(thread_id, NULL);

    return 0;
}


```

## `thread_programs/t10.c`

```c
//10.Implement a C program to create a thread that checks if a given string is a palindrome?
#include <stdio.h>
#include <string.h>
#include <pthread.h>

// Thread function to check palindrome
void* check_palindrome(void* arg) 
{
    char str[100];
    int len, is_palindrome = 1;
    printf("Enter a string: ");
    fgets(str,sizeof(str),stdin);
    len = strlen(str);
    for (int i = 0; i < len / 2; i++) 
    {
        if (str[i] != str[len - 1 - i]) 
        {
            is_palindrome = 0;
            break;
        }
    }

    if (is_palindrome)
        printf("\"%s\" is a palindrome.\n", str);
    else
            printf("\"%s\" is not a palindrome.\n", str);
   return NULL;
}

int main() 
{
    pthread_t thread;
    pthread_create(&thread, NULL, check_palindrome, NULL);
    pthread_join(thread, NULL);
}


```

## `thread_programs/t11.c`

```c
//Write a C program to create a thread that prints "Hello, World!" with thread synchronization?
#include <stdio.h>
#include <pthread.h>

pthread_mutex_t lock;


void* print_message(void* arg)
{
    pthread_mutex_lock(&lock);
    printf("Hello, World!\n");
    pthread_mutex_unlock(&lock);
    return NULL;
}

int main()
{
    pthread_t thread;

    pthread_mutex_init(&lock, NULL);
    pthread_create(&thread, NULL, print_message, NULL);
    pthread_join(thread, NULL);
    pthread_mutex_destroy(&lock);
}


```

## `thread_programs/t12.c`

```c
//12.Develop a C program to create two threads that print their thread IDs and synchronize their output
#include <stdio.h>
#include <pthread.h>

// Declare a mutex
pthread_mutex_t lock;

// Thread function
void* print_thread_id(void* arg)
{
    pthread_mutex_lock(&lock);
    printf("Hello from thread! My thread ID is: %lu\n", (unsigned long)pthread_self());
    pthread_mutex_unlock(&lock);
    return NULL;
}

int main()
{
    pthread_t thread1, thread2;
    pthread_mutex_init(&lock, NULL);
    pthread_create(&thread1, NULL, print_thread_id, NULL);
    pthread_create(&thread2, NULL, print_thread_id, NULL);
    pthread_join(thread1, NULL);
    pthread_join(thread2, NULL);
    pthread_mutex_destroy(&lock);
}



```

## `thread_programs/t13.c`

```c
//13.Implement a C program to create a thread that generates random numbers and synchronizes access to a shared buffer?
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <unistd.h> // for sleep()

#define SIZE 5 // size of shared buffer

int buffer[SIZE];
int count = 0; // how many numbers added

pthread_mutex_t lock; 


void* generate_random(void* arg)
{
    while (count < SIZE) 
    {
        int num = rand() % 100; // generate random number (0–99)

        pthread_mutex_lock(&lock);
        buffer[count] = num;
        printf("Generated number: %d stored at index %d\n", num, count);
        count++;

        pthread_mutex_unlock(&lock); 

        sleep(1); // delay for visibility
    }
    return NULL;
}

int main() 
{
    pthread_t thread;

    srand(time(NULL)); // seed random number generator

    pthread_mutex_init(&lock, NULL); 

    pthread_create(&thread, NULL, generate_random, NULL);
    pthread_join(thread, NULL);

    pthread_mutex_destroy(&lock); 

    // Print final buffer content
    printf("\nFinal buffer: ");
    for (int i = 0; i < SIZE; i++)
    {
        printf("%d ", buffer[i]);
    }
    printf("\n");

    return 0;
}


```

## `thread_programs/t14.c`

```c
//14.Write a C program to create a thread that performs addition of two numbers with mutex locks.
#include <stdio.h>
#include <pthread.h>

// Global variables
int a = 0, b = 0, sum = 0;
pthread_mutex_t lock; 
void* add_numbers(void* arg) 
{
    pthread_mutex_lock(&lock); 
    sum = a + b;
    printf("Sum of %d and %d is: %d\n", a, b, sum);
    pthread_mutex_unlock(&lock); 
    return NULL;
}

int main() 
{
    pthread_t thread;
    printf("Enter first number: ");
    scanf("%d", &a);
    printf("Enter second number: ");
    scanf("%d", &b);
    pthread_mutex_init(&lock, NULL);
    pthread_create(&thread, NULL, add_numbers, NULL);
    pthread_join(thread, NULL);
    pthread_mutex_destroy(&lock);
}


```

## `thread_programs/t15.c`

```c
//15.Implement a C program to create two threads that increment and decrement a shared variable, respectively, using mutex locks


#include <stdio.h>
#include <pthread.h>
#include <unistd.h>

int shared_var = 0;            // Shared variable
pthread_mutex_t lock;          
void* increment(void* arg) 
{
    for (int i = 0; i < 5; i++)
    {
        pthread_mutex_lock(&lock);
        shared_var++;
        printf("Increment thread: shared_var = %d\n", shared_var);
        pthread_mutex_unlock(&lock);
        sleep(1);
    }
    return NULL;
}


void* decrement(void* arg) 
{
    for (int i = 0; i < 5; i++) 
    {
        pthread_mutex_lock(&lock);
        shared_var--;
        printf("Decrement thread: shared_var = %d\n", shared_var);
        pthread_mutex_unlock(&lock);
        sleep(1);
    }
    return NULL;
}

int main() {
    pthread_t inc_thread, dec_thread;

  
    pthread_mutex_init(&lock, NULL);

    
    pthread_create(&inc_thread, NULL, increment, NULL);
    pthread_create(&dec_thread, NULL, decrement, NULL);

    
    pthread_join(inc_thread, NULL);
    pthread_join(dec_thread, NULL);

    
    pthread_mutex_destroy(&lock);

    printf("Final value of shared_var = %d\n", shared_var);
    return 0;
}


```

## `thread_programs/t16.c`

```c
//16.Develop a C program to create a thread that reads input from the user and synchronizes access to shared resources?
#include <stdio.h>
#include <pthread.h>
#include <string.h>

char shared_name[100];           // Shared resource
pthread_mutex_t lock;            


void* read_input(void* arg) 
{
    pthread_mutex_lock(&lock);   

    printf("Enter your name: ");
  //scanf("%s", shared_name);    // Reads a single word (no spaces)
    fgets(shared_name,sizeof(shared_name),stdin);

    pthread_mutex_unlock(&lock); 
    return NULL;
}

int main() 
{
    pthread_t thread;

    
    pthread_mutex_init(&lock, NULL);

 
    pthread_create(&thread, NULL, read_input, NULL);

  
    pthread_join(thread, NULL);

    printf("Hello, %s! Welcome!\n", shared_name);

    pthread_mutex_destroy(&lock);

    return 0;
}


```

## `thread_programs/t17.c`

```c
//17.Implement a C program to create a thread that prints prime numbers up to a given limit with mutex locks?
#include <stdio.h>
#include <pthread.h>

int limit;                      // Shared variable for the upper limit
pthread_mutex_t lock;           


int is_prime(int n) 
{
    if (n <= 1) return 0;
    for (int i = 2; i * i <= n; i++) 
    {
        if (n % i == 0)
            return 0;
    }
    return 1;
}

void* print_primes(void* arg) 
{
    pthread_mutex_lock(&lock); 

    printf("Prime numbers up to %d:\n", limit);
    for (int i = 2; i <= limit; i++) 
    {
        if (is_prime(i)) 
        {
            printf("%d ", i);
        }
    }
    printf("\n");

    pthread_mutex_unlock(&lock);  
    return NULL;
}

int main() 
{
    pthread_t thread;

  
    printf("Enter the limit: ");
    scanf("%d", &limit);

    
    pthread_mutex_init(&lock, NULL);

 
    pthread_create(&thread, NULL, print_primes, NULL);

    pthread_join(thread, NULL);

    pthread_mutex_destroy(&lock);

    return 0;
}


```

## `thread_programs/t18.c`

```c
//18.Implement a C program to create a thread that calculates the sum of Fibonacci numbers up to a given limit using dynamic programming with mutex locks
#include <stdio.h>
#include <pthread.h>

int limit;              // Number of terms (e.g., 10 terms of Fibonacci)
int fib[100];           // Store Fibonacci numbers
int sum = 0;            // Shared sum
pthread_mutex_t lock;   

void* calculate_fibonacci(void* arg) 
{
    pthread_mutex_lock(&lock);

    fib[0] = 0;
    fib[1] = 1;
    sum = fib[0] + fib[1];

    for (int i = 2; i < limit; i++) 
    {
        fib[i] = fib[i - 1] + fib[i - 2];
        sum += fib[i];
    }

    printf("Fibonacci Series up to %d terms:\n", limit);
    for (int i = 0; i < limit; i++) 
    {
        printf("%d ", fib[i]);
    }
    printf("\nSum of Fibonacci numbers: %d\n", sum);

    pthread_mutex_unlock(&lock);
    return NULL;
}

int main() 
{
    pthread_t thread;

    printf("Enter the number of Fibonacci terms: ");
    scanf("%d", &limit);

    
    pthread_mutex_init(&lock, NULL);

 
    pthread_create(&thread, NULL, calculate_fibonacci, NULL);

    pthread_join(thread, NULL);

    pthread_mutex_destroy(&lock);

    return 0;
}


```

## `thread_programs/t19.c`

```c
//19.Write a C program to create a thread that checks if a given year is a leap year using dynamic programming with mutex locks?
#include <stdio.h>
#include <pthread.h>

int year;                          // Shared input year
int result_cache[10000] = {-1};    
pthread_mutex_t lock;              


void* check_leap_year(void* arg) 
{
    pthread_mutex_lock(&lock);

    if (year < 0 || year >= 10000) 
    {
        printf("Invalid year. Please enter between 0 and 9999.\n");
        pthread_mutex_unlock(&lock);
        return NULL;
    }

    if (result_cache[year] != -1) 
    {
        if (result_cache[year])
            printf("%d is a leap year (cached).\n", year);
        else
            printf("%d is not a leap year (cached).\n", year);
    } 
    else 
    {
        // Check leap year condition
        int is_leap = 0;
        if ((year % 4 == 0 && year % 100 != 0) || (year % 400 == 0))
            is_leap = 1;

        // Store in cache
        result_cache[year] = is_leap;

        if (is_leap)
            printf("%d is a leap year.\n", year);
        else
            printf("%d is not a leap year.\n", year);
    }

    pthread_mutex_unlock(&lock);
    return NULL;
}

int main() 
{
    pthread_t thread;
    for (int i = 0; i < 10000; i++)
        result_cache[i] = -1;

    printf("Enter a year to check: ");
    scanf("%d", &year);
    pthread_mutex_init(&lock, NULL);

    
    pthread_create(&thread, NULL, check_leap_year, NULL);

    
    pthread_join(thread, NULL);

    
    pthread_mutex_destroy(&lock);

    return 0;
}


```

## `thread_programs/t2.c`

```c
//Modify the above program to create multiple threads, each printing its own message
// File: multi_hello_thread.c
#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>

#define NUM_THREADS 5
void* print_hello(void* arg) 
{
    int thread_num = *(int*)arg;
    printf("Hello from thread %d\n", thread_num);
    free(arg); // Free allocated memory
    return NULL;
}

int main() 
{
    pthread_t threads[NUM_THREADS];

    for (int i = 0; i < NUM_THREADS; i++) 
    {
        int* thread_num = malloc(sizeof(int));
        if (thread_num == NULL)
       	{
            perror("Failed to allocate memory");
            return 1;
        }
        *thread_num = i + 1;

        if (pthread_create(&threads[i], NULL, print_hello, thread_num) != 0) 
	    {
            perror("Failed to create thread");
            return 1;
        }
    }
    for (int i = 0; i < NUM_THREADS; i++) 
    {
        pthread_join(threads[i], NULL);
    }

    return 0;
}


```

## `thread_programs/t20.c`

```c
//20.Write a C program to create a thread that checks if a given string is a palindrome using dynamic programming with mutex locks
#include <stdio.h>
#include <string.h>
#include <pthread.h>

char input[100];               // Shared input string
int dp[100][100];              
pthread_mutex_t lock;          

void* check_palindrome(void* arg) 
{
    pthread_mutex_lock(&lock);

    int len = strlen(input);

    // Initialize DP table: all single characters are palindromes
    for (int i = 0; i < len; i++)
        dp[i][i] = 1;

    // Build DP table for substrings
    for (int l = 2; l <= len; l++)// l = substring length 
    { 
        for (int i = 0; i <= len - l; i++) 
        {
            int j = i + l - 1;

            if (input[i] == input[j]) 
            {
                if (l == 2)
                    dp[i][j] = 1;
                else
                    dp[i][j] = dp[i + 1][j - 1];
            } 
            else 
            {
                dp[i][j] = 0;
            }
        }
    }

    if (dp[0][len - 1])
        printf("\"%s\" is a palindrome.\n", input);
    else
        printf("\"%s\" is not a palindrome.\n", input);

    pthread_mutex_unlock(&lock);
    return NULL;
}

int main() 
{
    pthread_t thread;

  
    printf("Enter a string: ");
    // scanf("%s", input); 
    fgets(input,sizeof(input),stdin);
  
    pthread_mutex_init(&lock, NULL);

    
    pthread_create(&thread, NULL, check_palindrome, NULL);

    
    pthread_join(thread, NULL);

    
    pthread_mutex_destroy(&lock);

    return 0;
}


```

## `thread_programs/t21.c`

```c
//21.Implement a C program to create a thread that performs selection sort on an array of integers?
#include <stdio.h>
#include <pthread.h>

#define SIZE 5

int arr[SIZE];  
void* selection_sort(void* arg) 
{
    for (int i = 0; i < SIZE - 1; i++) 
    {
        int min_idx = i;
        for (int j = i + 1; j < SIZE; j++) 
        {
            if (arr[j] < arr[min_idx]) 
            {
                min_idx = j;
            }
        }

        // Swap elements
        if (min_idx != i) 
        {
            int temp = arr[i];
            arr[i] = arr[min_idx];
            arr[min_idx] = temp;
        }
    }

    printf("Sorted array using Selection Sort:\n");
    for (int i = 0; i < SIZE; i++) 
    {
        printf("%d ", arr[i]);
    }
    printf("\n");

    return NULL;
}

int main() 
{
    pthread_t thread;

   
    printf("Enter %d integers:\n", SIZE);
    for (int i = 0; i < SIZE; i++) 
    {
        scanf("%d", &arr[i]);
    }

    
    pthread_create(&thread, NULL, selection_sort, NULL);

  
    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t22.c`

```c
//22.Develop a C program to create a thread that calculates the area of a triangle?

#include <stdio.h>
#include <pthread.h>

// Structure to pass multiple values to thread
struct Triangle 
{
    float base;
    float height;
};

void* calculate_area(void* arg) 
{
    struct Triangle* t = (struct Triangle*)arg;

    float area = 0.5 * t->base * t->height;
    printf("Area of the triangle: %.2f\n", area);

    return NULL;
}

int main() 
{
    pthread_t thread;
    struct Triangle t;

  
    printf("Enter the base of the triangle: ");
    scanf("%f", &t.base);
    printf("Enter the height of the triangle: ");
    scanf("%f", &t.height);
    pthread_create(&thread, NULL, calculate_area, (void*)&t);
    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t23.c`

```c
//23.Write a C program to create a thread that calculates the sum of squares of numbers from 1 to 100?
#include <stdio.h>
#include <pthread.h>

int sum = 0;  // Shared variable of type int

// Thread function to calculate sum of squares
void* calculate_sum_of_squares(void* arg) 
{
    for (int i = 1; i <= 100; i++) 
    {
        sum += i * i;
    }

    printf("Sum of squares from 1 to 100 is: %d\n", sum);
    return NULL;
}

int main() 
{
    pthread_t thread;
    pthread_create(&thread, NULL, calculate_sum_of_squares, NULL);

    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t24.c`

```c
//24.Write a C program to create a thread that generates a random array of integers
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <time.h>

#define SIZE 10          // Size of the array
int arr[SIZE];          // Shared array to store random numbers
void* generate_random_array(void* arg) 
{
    srand(time(NULL));  // Seed the random number generator

    printf("Random array of %d integers:\n", SIZE);
    for (int i = 0; i < SIZE; i++) 
    {
        arr[i] = rand() % 100;  // Random number between 0 and 99
        printf("%d ", arr[i]);
    }
    printf("\n");

    return NULL;
}

int main() 
{
    pthread_t thread;
    pthread_create(&thread, NULL, generate_random_array, NULL);

    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t25.c`

```c
//25.Implement a C program to create a thread that performs bubble sort on an array of integers?
#include <stdio.h>
#include <pthread.h>

#define SIZE 5

int arr[SIZE];  // Shared array to be sorted

void* bubble_sort(void* arg) 
{
    for (int i = 0; i < SIZE - 1; i++) 
    {
        for (int j = 0; j < SIZE - 1 - i; j++) 
        {
            if (arr[j] > arr[j + 1])
            {    // Swap
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }

    printf("Sorted array using Bubble Sort:\n");
    for (int i = 0; i < SIZE; i++) 
    {
        printf("%d ", arr[i]);
    }
    printf("\n");

    return NULL;
}

int main() 
{
    pthread_t thread;
    printf("Enter %d integers:\n", SIZE);
    for (int i = 0; i < SIZE; i++) 
    {
        scanf("%d", &arr[i]);
    }

    pthread_create(&thread, NULL, bubble_sort, NULL);

    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t26.c`

```c
//26.Develop a C program to create a thread that calculates the greatest common divisor (GCD) of two numbers?
#include <stdio.h>
#include <pthread.h>

int a, b, gcd;  // Shared variables

// Function to compute GCD using Euclidean algorithm
int compute_gcd(int x, int y) 
{
    while (y != 0) 
    {
        int temp = y;
        y = x % y;
        x = temp;
    }
    return x;
}

// Thread function to calculate GCD
void* find_gcd(void* arg) 
{
    gcd = compute_gcd(a, b);
    printf("GCD of %d and %d is: %d\n", a, b, gcd);
    return NULL;
}

int main() 
{
    pthread_t thread;
    printf("Enter first number: ");
    scanf("%d", &a);
    printf("Enter second number: ");
    scanf("%d", &b);
    pthread_create(&thread, NULL, find_gcd, NULL);
    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t27.c`

```c
//27.Write a C program to create a thread that calculates the sum of Fibonacci numbers up to a given limit?
#include <stdio.h>
#include <pthread.h>

int limit;     // Number of Fibonacci terms
int sum = 0;   // Sum of Fibonacci numbers
void* calculate_fibonacci_sum(void* arg) 
{
    int a = 0, b = 1, next;

    sum = a + b;

    for (int i = 2; i < limit; i++) 
    {
        next = a + b;
        sum += next;
        a = b;
        b = next;
    }

    if (limit == 1) sum = 0;
    if (limit == 0) sum = 0;

    printf("Sum of first %d Fibonacci numbers is: %d\n", limit, sum);
    return NULL;
}

int main() 
{
    pthread_t thread;
    printf("Enter the number of Fibonacci terms: ");
    scanf("%d", &limit);
    pthread_create(&thread, NULL, calculate_fibonacci_sum, NULL);

    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t28.c`

```c
//8.Implement a C program to create a thread that calculates the sum of even numbers from 1 to 100
#include <stdio.h>
#include <pthread.h>

int sum = 0;  // Shared variable to store the result

void* calculate_even_sum(void* arg) 
{
    for (int i = 2; i <= 100; i += 2) 
    {
        sum += i;
    }

    printf("Sum of even numbers from 1 to 100 is: %d\n", sum);
    return NULL;
}

int main() 
{
    pthread_t thread;

 
    pthread_create(&thread, NULL, calculate_even_sum, NULL);

    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t29.c`

```c
//29.Develop a C program to create a thread that calculates the factorial of a given number using iteration?
#include <stdio.h>
#include <pthread.h>

int number;     // Shared variable for input
int result = 1; // To store factorial
void* calculate_factorial(void* arg) 
{
    for (int i = 1; i <= number; i++) 
    {
        result *= i;
    }

    printf("Factorial of %d is: %d\n", number, result);
    return NULL;
}

int main() 
{
    pthread_t thread;
    printf("Enter a number to calculate its factorial: ");
    scanf("%d", &number);
    pthread_create(&thread, NULL, calculate_factorial, NULL);

    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t2_1.c`

```c
//Modify the above program to create multiple threads, each printing its own message
#include <stdio.h>
#include <pthread.h>

// Separate thread functions
void* thread1_func(void* arg) 
{
    printf("Hello from thread 1\n");
    return NULL;
}

void* thread2_func(void* arg) 
{
    printf("Hello from thread 2\n");
    return NULL;
}

void* thread3_func(void* arg)
{
    printf("Hello from thread 3\n");
    return NULL;
}

int main() 
{
    pthread_t t1, t2, t3;
    pthread_create(&t1, NULL, thread1_func, NULL);
    pthread_create(&t2, NULL, thread2_func, NULL);
    pthread_create(&t3, NULL, thread3_func, NULL);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);
    pthread_join(t3, NULL);

    return 0;
}


```

## `thread_programs/t3.c`

```c
//Develop a C program to create two threads that print numbers from 1 to 10 concurrently?
#include <stdio.h>
#include <pthread.h>
#include <unistd.h> // For sleep()

void* print_numbers(void* arg)
{
    char* thread_name = (char*)arg;
    for (int i = 1; i <= 10; i++) 
    {
        printf("%s: %d\n", thread_name, i);
        sleep(1); 
    }

    return NULL;
}

int main() 
{
    pthread_t t1, t2;
    pthread_create(&t1, NULL, print_numbers, "Thread 1");
    pthread_create(&t2, NULL, print_numbers, "Thread 2");
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    return 0;
}


```

## `thread_programs/t30.c`

```c
//30.Write a C program to create a thread that checks if a given year is a leap year?
#include <stdio.h>
#include <pthread.h>

int year;  // Shared variable to store the input year

// Thread function to check leap year
void* check_leap_year(void* arg) 
{
    if ((year % 4 == 0 && year % 100 != 0) || (year % 400 == 0)) 
    {
        printf("%d is a leap year.\n", year);
    } 
    else 
    {
        printf("%d is not a leap year.\n", year);
    }
    return NULL;
}

int main() 
{
    pthread_t thread;

    // Get the year from the user
    printf("Enter a year: ");
    scanf("%d", &year);
    pthread_create(&thread, NULL, check_leap_year, NULL);
    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t31.c`

```c
//31.Implement a C program to create a thread that performs multiplication of two matrices?
#include <stdio.h>
#include <pthread.h>

#define ROW1 2
#define COL1 2
#define ROW2 2
#define COL2 2

int mat1[ROW1][COL1];
int mat2[ROW2][COL2];
int result[ROW1][COL2];

void* multiply_matrices(void* arg) 
{
    for (int i = 0; i < ROW1; i++) 
    {
        for (int j = 0; j < COL2; j++) 
        {
            result[i][j] = 0;
            for (int k = 0; k < COL1; k++) 
            {
                result[i][j] += mat1[i][k] * mat2[k][j];
            }
        }
    }

    printf("Result of matrix multiplication:\n");
    for (int i = 0; i < ROW1; i++) 
    {
        for (int j = 0; j < COL2; j++) 
        {
            printf("%d ", result[i][j]);
        }
        printf("\n");
    }

    return NULL;
}

int main() 
{
    pthread_t thread;
    printf("Enter elements of 1st  (%dx%d):\n", ROW1, COL1);
    for (int i = 0; i < ROW1; i++) 
    {
        for (int j = 0; j < COL1; j++) 
        {
            scanf("%d", &mat1[i][j]);
        }
    }

    printf("Enter elements of 2nd  (%dx%d):\n", ROW2, COL2);
    for (int i = 0; i < ROW2; i++) 
    {
        for (int j = 0; j < COL2; j++) 
        {
            scanf("%d", &mat2[i][j]);
        }
    }

    pthread_create(&thread, NULL, multiply_matrices, NULL);

    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t32.c`

```c
//32.Develop a C program to create a thread that calculates the average of numbers from 1 to
100

#include <stdio.h>
#include <pthread.h>

float average = 0;

void* calculate_average(void* arg) 
{
    int sum = 0;
    for (int i = 1; i <= 100; i++) 
    {
        sum += i;
    }

    average = sum / 100.0;
    printf("Average of numbers from 1 to 100 is: %.2f\n", average);

    return NULL;
}

int main() 
{
    pthread_t thread; 
    pthread_create(&thread, NULL, calculate_average, NULL);
    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t33.c`

```c
//33.Implement a C program to create a thread that generates a random string?
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <pthread.h>

#define LENGTH 10  // Length of random string

char random_str[LENGTH + 1];  // +1 for null terminator

void* generate_random_string(void* arg) 
{
    const char charset[] = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    int charset_size = sizeof(charset) - 1;

    srand(time(NULL));  // Seed for random number generation

    for (int i = 0; i < LENGTH; i++) 
    {
        int index = rand() % charset_size;
        random_str[i] = charset[index];
    }

    random_str[LENGTH] = '\0'; 

    printf("Generated random string: %s\n", random_str);

    return NULL;
}

int main() 
{
    pthread_t thread;
    pthread_create(&thread, NULL, generate_random_string, NULL);
    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t34.c`

```c
//34.Write a C program to create a thread that checks if a given number is a perfect square?
#include <stdio.h>
#include <pthread.h>
#include <math.h>

int number; 

void* check_perfect_square(void* arg) 
{
    int root = (int)sqrt(number);

    if (root * root == number) 
    {
        printf("%d is a perfect square.\n", number);
    } 
    else 
    {
        printf("%d is not a perfect square.\n", number);
    }

    return NULL;
}

int main() 
{
    pthread_t thread;
    printf("Enter a number: ");
    scanf("%d", &number);
    pthread_create(&thread, NULL, check_perfect_square, NULL);
    pthread_join(thread, NULL);
    return 0;
}


```

## `thread_programs/t35.c`

```c
//35.Write a C program to create a thread that calculates the sum of digits of a given number?
#include <stdio.h>
#include <pthread.h>

int number;  // Input number
int sum = 0; 
void* calculate_sum_of_digits(void* arg) 
{
    int n = number;
    while (n != 0) 
    {
        sum += n % 10;
        n /= 10;
    }

    printf("Sum of digits of %d is: %d\n", number, sum);
    return NULL;
}

int main() 
{
    pthread_t thread;
    printf("Enter a number: ");
    scanf("%d", &number);
    pthread_create(&thread, NULL, calculate_sum_of_digits, NULL);
    pthread_join(thread, NULL);
    return 0;
}


```

## `thread_programs/t36.c`

```c
//36.Implement a C program to create a thread that calculates the factorial of a given number using recursion
#include <stdio.h>
#include <pthread.h>

int number;   
unsigned int result = 1;

unsigned int factorial(int n) 
{
    if (n <= 1)
        return 1;
    else
        return n * factorial(n - 1);
}

void* calculate_factorial(void* arg) 
{
    result = factorial(number);
    printf("Factorial of %d is: %u\n", number, result);
    return NULL;
}

int main() 
{
    pthread_t thread;
    printf("Enter a number: ");
    scanf("%d", &number);
    pthread_create(&thread, NULL, calculate_factorial, NULL);
    pthread_join(thread, NULL);
    return 0;
}


```

## `thread_programs/t37.c`

```c
//37.Develop a C program to create a thread that finds the maximum element in an array?
#include <stdio.h>
#include <pthread.h>

#define SIZE 5
int arr[SIZE];      // Array of integers
int max_value;      // Maximum value to be found

void* find_max(void* arg) 
{
    max_value = arr[0];
    for (int i = 1; i < SIZE; i++) 
    {
        if (arr[i] > max_value) 
        {
            max_value = arr[i];
        }
    }
    printf("Maximum element in the array is: %d\n", max_value);
    return NULL;
}

int main() 
{
    pthread_t thread;
    printf("Enter %d integers:\n", SIZE);
    for (int i = 0; i < SIZE; i++) 
    {
        scanf("%d", &arr[i]);
    }
    pthread_create(&thread, NULL, find_max, NULL);
    pthread_join(thread, NULL);
    return 0;
}


```

## `thread_programs/t38.c`

```c
//38.Write a C program to create a thread that sorts an array of strings?
#include <stdio.h>
#include <pthread.h>
#include <string.h>

#define SIZE 5
#define MAX_LEN 100

char strings[SIZE][MAX_LEN];  // Array to hold strings

// Thread function to sort strings
void* sort_strings(void* arg) 
{
    char temp[MAX_LEN];
    for (int i = 0; i < SIZE - 1; i++) 
    {
        for (int j = i + 1; j < SIZE; j++) 
        {
            if (strcmp(strings[i], strings[j]) > 0) 
            {
                strcpy(temp, strings[i]);
                strcpy(strings[i], strings[j]);
                strcpy(strings[j], temp);
            }
        }
    }

    printf("\nSorted strings:\n");
    for (int i = 0; i < SIZE; i++) 
    {
        printf("%s\n", strings[i]);
    }

    return NULL;
}

int main() 
{
    pthread_t thread;
    printf("Enter %d strings:\n", SIZE);
    for (int i = 0; i < SIZE; i++) 
    {
        scanf("%s", strings[i]);
    }
    pthread_create(&thread, NULL, sort_strings, NULL);

    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t39.c`

```c
//39.Implement a C program to create a thread that calculates the square root of a number
/*
#include <stdio.h>
#include <pthread.h>
#include <math.h>

double number;       // User input
double square_root;  // Result

// Thread function to calculate square root
void* calculate_sqrt(void* arg) 
{
    if (number < 0) 
    {
        printf("Square root of a negative number is not real.\n");
    } 
    else 
    {
        square_root = sqrt(number);
        printf("Square root of %.2f is: %.2f\n", number, square_root);
    }
    return NULL;
}

int main() 
{
    pthread_t thread;
    printf("Enter a number: ");
    scanf("%lf", &number);
    pthread_create(&thread, NULL, calculate_sqrt, NULL);
    pthread_join(thread, NULL);

    return 0;
}
*/
#include <stdio.h>
#include <pthread.h>

int number;
int square_root;

void* integer_square_root(void* arg) 
{
    square_root = 0;

    if (number < 0) 
    {
        printf("Square root of a negative number is not real.\n");
        return NULL;
    }

    for (int i = 0; i * i <= number; i++) 
    {
        square_root = i;
    }

    printf("Integer square root of %d is: %d\n", number, square_root);
    return NULL;
}

int main()
{
    pthread_t thread;
    printf("Enter a number: ");
    scanf("%d", &number);
    pthread_create(&thread, NULL, integer_square_root, NULL);

    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t4.c`

```c
//Implement a C program to create a thread that calculates the factorial of a given number
#include <stdio.h>
#include <pthread.h>

void* factorial(void* arg)
{
    int num;
    printf("Enter a number to calculate factorial: ");
    scanf("%d", &num);

    long long fact = 1;
    for (int i = 1; i <= num; i++) 
    {
        fact *= i;
    }

    printf("Factorial of %d is %lld\n", num, fact);
    return NULL;
}

int main()
{
    pthread_t thread;

    pthread_create(&thread, NULL, factorial, NULL);

    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t40.c`

```c
//40.Develop a C program to create a thread that calculates the average of numbers in an array?
#include <stdio.h>
#include <pthread.h>

#define SIZE 5

int numbers[SIZE];     // Array of integers
float average = 0;     // To store the average

// Thread function to calculate average
void* calculate_average(void* arg) 
{
    int sum = 0;
    for (int i = 0; i < SIZE; i++) 
    {
        sum += numbers[i];
    }
    average = sum / (float)SIZE;
    printf("Average of array elements is: %.2f\n", average);
    return NULL;
}

int main() 
{
    pthread_t thread;

    // Input array from user
    printf("Enter %d numbers:\n", SIZE);
    for (int i = 0; i < SIZE; i++) 
    {
        scanf("%d", &numbers[i]);
    }

    pthread_create(&thread, NULL, calculate_average, NULL);

    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t41.c`

```c
//1.Write a C program to create a thread that generates a random password?
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <pthread.h>

#define PASSWORD_LENGTH 12  // You can change this value

char password[PASSWORD_LENGTH + 1];  // +1 for null terminator

// Thread function to generate random password
void* generate_password(void* arg) 
{
    const char charset[] = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*";
    int charset_size = sizeof(charset) - 1;

    srand(time(NULL));  // Seed random number generator

    for (int i = 0; i < PASSWORD_LENGTH; i++)
    {
        int index = rand() % charset_size;
        password[i] = charset[index];
    }

    password[PASSWORD_LENGTH] = '\0';  // Null-terminate the string

    printf("Generated password: %s\n", password);
    return NULL;
}

int main() 
{
    pthread_t thread;

     to generate password
    pthread_create(&thread, NULL, generate_password, NULL);

    
    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t42.c`

```c
//42.Implement a C program to create a thread that checks if a number is even or odd?
#include <stdio.h>
#include <pthread.h>

int number; 

void* check_even_or_odd(void* arg) 
{
    if (number % 2 == 0)
        printf("%d is Even\n", number);
    else
        printf("%d is Odd\n", number);
    return NULL;
}

int main() 
{
    pthread_t thread;

  
    printf("Enter a number: ");
    scanf("%d", &number);

    pthread_create(&thread, NULL, check_even_or_odd, NULL);

    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t43.c`

```c
//43.Develop a C program to create a thread that calculates the sum of elements in an array?
#include <stdio.h>
#include <pthread.h>

#define SIZE 5

int array[SIZE];   // Input array
int sum = 0;       // Sum of elements

// Thread function to calculate sum
void* calculate_sum(void* arg) 
{
    for (int i = 0; i < SIZE; i++) 
    {
        sum += array[i];
    }
    printf("Sum of array elements is: %d\n", sum);
    return NULL;
}

int main() 
{
    pthread_t thread;

    printf("Enter %d integers:\n", SIZE);
    for (int i = 0; i < SIZE; i++) 
    {
        scanf("%d", &array[i]);
    }

    pthread_create(&thread, NULL, calculate_sum, NULL);

    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t44.c`

```c
//44.Write a C program to create a thread that calculates the factorial of numbers from 1 to 10?
#include <stdio.h>
#include <pthread.h>

// Thread function to calculate and print factorials from 1 to 10
void* calculate_factorials(void* arg) 
{
    for (int i = 1; i <= 10; i++) 
    {
        int fact = 1;
        for (int j = 1; j <= i; j++) 
        {
            fact *= j;
        }
        printf("Factorial of %d is: %d\n", i, fact);
    }
    return NULL;
}

int main() 
{
    pthread_t thread;

    pthread_create(&thread, NULL, calculate_factorials, NULL);

    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t45.c`

```c
//5.Implement a C program to create a thread that calculates the area of a rectangle
#include <stdio.h>
#include <pthread.h>

int length, breadth;
int area;

void* calculate_area(void* arg) 
{
    area = length * breadth;
    printf("Area of the rectangle is: %d\n", area);
    return NULL;
}

int main() 
{
    pthread_t thread;

    printf("Enter the length of the rectangle: ");
    scanf("%d", &length);

    printf("Enter the breadth of the rectangle: ");
    scanf("%d", &breadth);

    pthread_create(&thread, NULL, calculate_area, NULL);

    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t46.c`

```c
//46.Develop a C program to create a thread that generates random numbers?
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <pthread.h>

#define COUNT 10 

void* generate_random_numbers(void* arg) 
{
    srand(time(NULL));  // Seed the random number generator

    printf("Random numbers:\n");
    for (int i = 0; i < COUNT; i++) 
    {
        int num = rand() % 100;  // Random number between 0 and 99
        printf("%d ", num);
    }
    printf("\n");
    return NULL;
}

int main() 
{
    pthread_t thread;

    pthread_create(&thread, NULL, generate_random_numbers, NULL);
  
    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t47.c`

```c
//47.Implement a C program to create a thread that sorts an array of integers?
#include <stdio.h>
#include <pthread.h>

#define SIZE 5

int array[SIZE];  // Array to be sorted

// function to perform bubble sort
void* sort_array(void* arg) 
{
    for (int i = 0; i < SIZE - 1; i++) 
    {
        for (int j = 0; j < SIZE - 1 - i; j++) 
        {
            if (array[j] > array[j + 1]) 
            {
                // Swap elements
                int temp = array[j];
                array[j] = array[j + 1];
                array[j + 1] = temp;
            }
        }
    }

    // Print sorted array
    printf("Sorted array: ");
    for (int i = 0; i < SIZE; i++) {
        printf("%d ", array[i]);
    }
    printf("\n");

    return NULL;
}

int main() 
{
    pthread_t thread;

    printf("Enter %d integers:\n", SIZE);
    for (int i = 0; i < SIZE; i++) 
    {
        scanf("%d", &array[i]);
    }
    
    pthread_create(&thread, NULL, sort_array, NULL);
    
    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t48.c`

```c
//48.Write a C program to create a thread that searches for a given number in an array?
#include <stdio.h>
#include <pthread.h>

#define SIZE 5

int array[SIZE];   // Array to search in
int target;        // Number to search

void* search_number(void* arg) 
{
    for (int i = 0; i < SIZE; i++) 
    {
        if (array[i] == target) 
        {
            printf("Number %d found at index %d.\n", target, i);
            return NULL;
        }
    }
    printf("Number %d not found in the array.\n", target);
    return NULL;
}

int main() 
{
    pthread_t thread;

    printf("Enter %d integers:\n", SIZE);
    for (int i = 0; i < SIZE; i++) 
    {
        scanf("%d", &array[i]);
    }

    printf("Enter the number to search: ");
    scanf("%d", &target);
   
    pthread_create(&thread, NULL, search_number, NULL);
    
    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t49.c`

```c
//49.Develop a C program to create a thread that reverses a given string?
#include <stdio.h>
#include <pthread.h>
#include <string.h>

#define SIZE 100

char str[SIZE];  // Input string

// Thread function to reverse the string
void* reverse_string(void* arg) 
{
    int len = strlen(str);
    for (int i = 0; i < len / 2; i++) 
    {
        char temp = str[i];
        str[i] = str[len - 1 - i];
        str[len - 1 - i] = temp;
    }

    printf("Reversed string: %s\n", str);
    return NULL;
}

int main() 
{
    pthread_t thread;

     printf("Enter a string: ");
    scanf("%s", str);  // Reads string without spaces
   
    pthread_create(&thread, NULL, reverse_string, NULL);
    
    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t5.c`

```c
//5.Write a C program to create two threads that print their thread IDs?
#include <stdio.h>
#include <pthread.h>

// Thread function
void* print_thread_id(void* arg) 
{
    pthread_t tid = pthread_self();  // Get thread ID
    printf("Hello from thread! Thread ID: %lu\n", (unsigned long)tid);
    return NULL;
}

int main()
{
    pthread_t t1, t2;
   
    pthread_create(&t1, NULL, print_thread_id, NULL);
    pthread_create(&t2, NULL, print_thread_id, NULL);

     to finish
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    return 0;
}


```

## `thread_programs/t50.c`

```c
//50.Implement a C program to create a thread that reads input from the user
#include <stdio.h>
#include <pthread.h>

#define SIZE 100

char input[SIZE];  // Buffer to store input


void* read_input(void* arg) 
{
    printf("Enter a string: ");
    scanf("%s", input);  // Reads a single word (no spaces)
    printf("You entered: %s\n", input);
    return NULL;
}

int main() 
{
    pthread_t thread;
    
    pthread_create(&thread, NULL, read_input, NULL);
    
    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t51.c`

```c
//51.Write a C program to create a thread that performs addition of two matrices
#include <stdio.h>
#include <pthread.h>

#define ROWS 2
#define COLS 2

int mat1[ROWS][COLS];     // First matrix
int mat2[ROWS][COLS];     // Second matrix
int result[ROWS][COLS];   // Result matrix

// Thread function to add two matrices
void* add_matrices(void* arg) 
{
    for (int i = 0; i < ROWS; i++) 
    {
        for (int j = 0; j < COLS; j++) 
        {
            result[i][j] = mat1[i][j] + mat2[i][j];
        }
    }

    // Print the result matrix
    printf("Resultant Matrix (Addition):\n");
    for (int i = 0; i < ROWS; i++) 
    {
        for (int j = 0; j < COLS; j++) 
        {
            printf("%d ", result[i][j]);
        }
        printf("\n");
    }

    return NULL;
}

int main() 
{
    pthread_t thread;

    // Input first matrix
    printf("Enter elements of Matrix 1 (%dx%d):\n", ROWS, COLS);
    for (int i = 0; i < ROWS; i++)
        for (int j = 0; j < COLS; j++)
            scanf("%d", &mat1[i][j]);

    // Input second matrix
    printf("Enter elements of Matrix 2 (%dx%d):\n", ROWS, COLS);
    for (int i = 0; i < ROWS; i++)
        for (int j = 0; j < COLS; j++)
            scanf("%d", &mat2[i][j]);

    
    pthread_create(&thread, NULL, add_matrices, NULL);

  
    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t52.c`

```c
//52.Develop a C program to create a thread that calculates the length of a given string?
#include <stdio.h>
#include <pthread.h>
#include <string.h>

#define SIZE 100

char str[SIZE];  // Input string

// Thread function to calculate length
void* calculate_length(void* arg) 
{
    int length = 0;

    // Manually calculating length (without using strlen)
    while (str[length] != '\0') 
    {
        length++;
    }

    printf("Length of the string is: %d\n", length);
    return NULL;
}

int main() 
{
    pthread_t thread;

    // Input string from user
    printf("Enter a string: ");
    scanf("%s", str);  // Reads single word (without spaces)
    
    pthread_create(&thread, NULL, calculate_length, NULL);
    
    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t53.c`

```c
//53.Write a C program to create two threads using pthreads library. Each thread should print "Hello, World!" along with its thread ID.
#include <stdio.h>
#include <pthread.h>

// Thread function
void* print_hello(void* arg) 
{
    pthread_t tid = pthread_self();  // Get thread ID
    printf("Hello, World! from thread ID: %lu\n", (unsigned long)tid);
    return NULL;
}

int main() 
{
    pthread_t t1, t2;

   
    pthread_create(&t1, NULL, print_hello, NULL);
    pthread_create(&t2, NULL, print_hello, NULL);

     to finish
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    return 0;
}


```

## `thread_programs/t54.c`

```c
//54.Modify the previous program to pass arguments to the threads and print those arguments along with the thread ID?
#include <stdio.h>
#include <pthread.h>

// Thread function with argument
void* print_message(void* arg) 
{
    char* msg = (char*)arg;
    pthread_t tid = pthread_self();
    printf("Thread ID: %lu | Message: %s\n", (unsigned long)tid, msg);
    return NULL;
}

int main() 
{
    pthread_t t1, t2;

    // Messages to pass to threads
    char* msg1 = "Hello from Thread 1";
    char* msg2 = "Hello from Thread 2";

    pthread_create(&t1, NULL, print_message, (void*)msg1);
    pthread_create(&t2, NULL, print_message, (void*)msg2);

     to finish
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    return 0;
}


```

## `thread_programs/t55.c`

```c
//55.Write a C program to demonstrate thread synchronization using mutex locks. Create two threads that increment a shared variable using mutex locks to ensure proper synchronization?
#include <stdio.h>
#include <pthread.h>

#define NUM_ITERATIONS 1000000

int shared_var = 0;               // Shared variable
pthread_mutex_t lock;             // Mutex lock

// Thread function to increment shared_var
void* increment(void* arg) 
{
    for (int i = 0; i < NUM_ITERATIONS; i++) 
    {
        pthread_mutex_lock(&lock);     // Lock
        shared_var++;                  // Critical section
        pthread_mutex_unlock(&lock);   // Unlock
    }
    return NULL;
}

int main() 
{
    pthread_t t1, t2;
  
    pthread_mutex_init(&lock, NULL);
   
    pthread_create(&t1, NULL, increment, NULL);
    pthread_create(&t2, NULL, increment, NULL);
    
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);
    
    pthread_mutex_destroy(&lock);

    printf("Final value of shared_var: %d\n", shared_var);

    return 0;
}


```

## `thread_programs/t56.c`

```c
//56.Extend the previous program to use semaphore instead of mutex locks for thread synchronization
#include <stdio.h>
#include <pthread.h>
#include <semaphore.h>

#define NUM_ITERATIONS 1000000

int shared_var = 0;       // Shared variable
sem_t sem;                // Semaphore

// Thread function to increment shared_var
void* increment(void* arg) 
{
    for (int i = 0; i < NUM_ITERATIONS; i++) 
    {
        sem_wait(&sem);     // Wait (decrease semaphore)
        shared_var++;       // Critical section
        sem_post(&sem);     // Signal (increase semaphore)
    }
    return NULL;
}

int main() 
{
    pthread_t t1, t2;

    // Initialize semaphore with value 1 (binary semaphore)
    sem_init(&sem, 0, 1);

    pthread_create(&t1, NULL, increment, NULL);
    pthread_create(&t2, NULL, increment, NULL);
    
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    // Destroy semaphore
    sem_destroy(&sem);

    printf("Final value of shared_var: %d\n", shared_var);

    return 0;
}


```

## `thread_programs/t57.c`

```c
//57.Write a C program to implement the producer-consumer problem using pthreads. Create two threads - one for producing items and another for consuming items from a shared buffer
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <semaphore.h>

#define SIZE 5  // Buffer size

int buffer[SIZE];  // Shared buffer
int in = 0, out = 0;

pthread_mutex_t mutex;
sem_t full, empty;

// Producer thread
void* producer(void* arg) 
{
    for (int i = 1; i <= 10; i++) 
    {
        sem_wait(&empty);  // Wait for empty space
        pthread_mutex_lock(&mutex);

        buffer[in] = i;
        printf("Produced: %d\n", i);
        in = (in + 1) % SIZE;

        pthread_mutex_unlock(&mutex);
        sem_post(&full);  // Signal that buffer has new item
    }
    return NULL;
}

// Consumer thread
void* consumer(void* arg) 
{
    for (int i = 1; i <= 10; i++) 
    {
        sem_wait(&full);  // Wait for available item
        pthread_mutex_lock(&mutex);

        int item = buffer[out];
        printf("Consumed: %d\n", item);
        out = (out + 1) % SIZE;

        pthread_mutex_unlock(&mutex);
        sem_post(&empty);  // Signal that buffer has space
    }
    return NULL;
}

int main() 
{
    pthread_t prod, cons;

   and semaphores
    pthread_mutex_init(&mutex, NULL);
    sem_init(&empty, 0, SIZE);  // All buffer slots are initially empty
    sem_init(&full, 0, 0);      // No items initially
    pthread_create(&prod, NULL, producer, NULL);
    pthread_create(&cons, NULL, consumer, NULL);

    
    pthread_join(prod, NULL);
    pthread_join(cons, NULL);

     and semaphores
    pthread_mutex_destroy(&mutex);
    sem_destroy(&empty);
    sem_destroy(&full);

    return 0;
}


```

## `thread_programs/t58.c`

```c
//58.Implement a multithreaded file copy program in C. Create multiple threads to read from one file and write to another file concurrently?
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <string.h>

#define BUFFER_SIZE 1024

char buffer[BUFFER_SIZE];
int bytes_read = 0;
int done = 0;

pthread_mutex_t mutex;
pthread_cond_t cond_read, cond_write;

FILE *src, *dest;

// Reader thread: reads from source file
void* reader_thread(void* arg) 
{
    while (1) {
        pthread_mutex_lock(&mutex);

        // Wait if previous data not written
        while (bytes_read != 0)
            pthread_cond_wait(&cond_read, &mutex);

        bytes_read = fread(buffer, 1, BUFFER_SIZE, src);

        if (bytes_read == 0) 
        {
            done = 1;
            pthread_cond_signal(&cond_write);  // Notify writer
            pthread_mutex_unlock(&mutex);
            break;
        }

        pthread_cond_signal(&cond_write);  // Notify writer
        pthread_mutex_unlock(&mutex);
    }
    return NULL;
}

// Writer thread: writes to destination file
void* writer_thread(void* arg)
{
    while (1) 
    {
        pthread_mutex_lock(&mutex);

        // Wait until there's data to write
        while (bytes_read == 0 && !done)
            pthread_cond_wait(&cond_write, &mutex);

        if (bytes_read > 0) 
        {
            fwrite(buffer, 1, bytes_read, dest);
            bytes_read = 0;
            pthread_cond_signal(&cond_read);  // Notify reader
        } 
        else if (done) 
        {
            pthread_mutex_unlock(&mutex);
            break;
        }

        pthread_mutex_unlock(&mutex);
    }
    return NULL;
}

int main() 
{
    pthread_t reader, writer;

    char src_name[100], dest_name[100];

    // Input source and destination filenames
    printf("Enter source filename: ");
    scanf("%s", src_name);
    printf("Enter destination filename: ");
    scanf("%s", dest_name);

    // Open files
    src = fopen(src_name, "rb");
    if (!src) {
        perror("Source file error");
        return 1;
    }

    dest = fopen(dest_name, "wb");
    if (!dest) {
        perror("Destination file error");
        fclose(src);
        return 1;
    }

 // condition variables
    pthread_mutex_init(&mutex, NULL);
    pthread_cond_init(&cond_read, NULL);
    pthread_cond_init(&cond_write, NULL);
   
    pthread_create(&reader, NULL, reader_thread, NULL);
    pthread_create(&writer, NULL, writer_thread, NULL);

    // Wait for threads to finish
    pthread_join(reader, NULL);
    pthread_join(writer, NULL);

    // Cleanup
    fclose(src);
    fclose(dest);
    pthread_mutex_destroy(&mutex);
    pthread_cond_destroy(&cond_read);
    pthread_cond_destroy(&cond_write);

    printf("File copy completed successfully.\n");
    return 0;
}


```

## `thread_programs/t59.c`

```c
//59.Write a C program to demonstrate thread cancellation. Create a thread that runs an infinite loop and cancels it after a certain condition is met from the main thread.
#include <stdio.h>
#include <pthread.h>
#include <unistd.h>  // For sleep()

// Thread function with infinite loop
void* infinite_loop(void* arg) 
{
    while (1) 
    {
        printf("Thread is running...\n");
        sleep(1);  // Simulate some work
    }
    return NULL;
}

int main() 
{
    pthread_t thread;

    pthread_create(&thread, NULL, infinite_loop, NULL);

    sleep(5);

    printf("Main thread: Cancelling the running thread...\n");
    pthread_cancel(thread);

    pthread_join(thread, NULL);

    printf("Thread has been cancelled. Exiting program.\n");

    return 0;
}


```

## `thread_programs/t6.c`

```c
//6.Develop a C program to create a thread that prints the sum of two numbers.
#include <stdio.h>
#include <pthread.h>

// Thread function
void* print_sum(void* arg) 
{
    int a, b;
    printf("Enter two numbers: ");
    scanf("%d %d", &a, &b);

    int sum = a + b;
    printf("Sum of %d and %d is: %d\n", a, b, sum);

    return NULL;
}

int main() 
{
    pthread_t thread;

    pthread_create(&thread, NULL, print_sum, NULL);

    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t60.c`

```c
//60.Write a C program to implement a reader-writer lock using pthreads. Create multiple reader threads and writer threads and ensure proper synchronization using the reader-writer lock?
/*
 *#include <stdio.h>
#include <pthread.h>
#include <unistd.h>  // For sleep()

#define NUM_READERS 3
#define NUM_WRITERS 2

int shared_data = 0;  // Shared resource
pthread_rwlock_t rwlock;

// Reader thread function
void* reader(void* arg) 
{
    int id = *(int*)arg;
    while (1) 
    {
        pthread_rwlock_rdlock(&rwlock);  // Acquire read lock

        printf("Reader %d read data = %d\n", id, shared_data);
        sleep(1);  // Simulate reading time

        pthread_rwlock_unlock(&rwlock);  // Release lock
        sleep(1);  // Wait before next read
    }
    return NULL;
}

// Writer thread function
void* writer(void* arg) 
{
    int id = *(int*)arg;
    while (1) 
    {
        pthread_rwlock_wrlock(&rwlock);  // Acquire write lock

        shared_data++;
        printf("Writer %d wrote data = %d\n", id, shared_data);
        sleep(2);  // Simulate writing time

        pthread_rwlock_unlock(&rwlock);  // Release lock
        sleep(3);  // Wait before next write
    }
    return NULL;
}

int main() 
{
    pthread_t r_threads[NUM_READERS], w_threads[NUM_WRITERS];
    int r_ids[NUM_READERS], w_ids[NUM_WRITERS];

    // Initialize reader-writer lock
    pthread_rwlock_init(&rwlock, NULL);

    // Create reader threads
    for (int i = 0; i < NUM_READERS; i++) 
    {
        r_ids[i] = i + 1;
        pthread_create(&r_threads[i], NULL, reader, &r_ids[i]);
    }

    // Create writer threads
    for (int i = 0; i < NUM_WRITERS; i++) 
    {
        w_ids[i] = i + 1;
        pthread_create(&w_threads[i], NULL, writer, &w_ids[i]);
    }

    // Join threads (infinite loop, so they run continuously)
    for (int i = 0; i < NUM_READERS; i++)
        pthread_join(r_threads[i], NULL);

    for (int i = 0; i < NUM_WRITERS; i++)
        pthread_join(w_threads[i], NULL);

    // Destroy lock (unreachable in this case)
    pthread_rwlock_destroy(&rwlock);

    return 0;
}
*/

/*
#include <stdio.h>
#include <pthread.h>
#include <unistd.h>

#define NUM_READERS 3
#define NUM_WRITERS 2

int shared_data = 0;
int read_count = 0;

pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
pthread_mutex_t write_lock = PTHREAD_MUTEX_INITIALIZER;

// Reader thread
void* reader(void* arg) 
{
    int id = *(int*)arg;
    while (1) 
    {
        // Entry section
        pthread_mutex_lock(&mutex);
        read_count++;
        if (read_count == 1) 
        {
            pthread_mutex_lock(&write_lock); // First reader locks writer
        }
        pthread_mutex_unlock(&mutex);

        // Critical section
        printf("Reader %d: read shared_data = %d\n", id, shared_data);
        sleep(1);

        // Exit section
        pthread_mutex_lock(&mutex);
        read_count--;
        if (read_count == 0) 
        {
            pthread_mutex_unlock(&write_lock); // Last reader unlocks writer
        }
        pthread_mutex_unlock(&mutex);

        sleep(1);
    }
    return NULL;
}

// Writer thread
void* writer(void* arg) 
{
    int id = *(int*)arg;
    while (1) 
    {
        pthread_mutex_lock(&write_lock); // Only one writer (and no readers)
        shared_data++;
        printf("Writer %d: wrote shared_data = %d\n", id, shared_data);
        pthread_mutex_unlock(&write_lock);
        sleep(3); // Simulate writing interval
    }
    return NULL;
}

int main() 
{
    pthread_t r_threads[NUM_READERS], w_threads[NUM_WRITERS];
    int r_ids[NUM_READERS], w_ids[NUM_WRITERS];

    // Create reader threads
    for (int i = 0; i < NUM_READERS; i++) 
    {
        r_ids[i] = i + 1;
        pthread_create(&r_threads[i], NULL, reader, &r_ids[i]);
    }

    // Create writer threads
    for (int i = 0; i < NUM_WRITERS; i++) 
    {
        w_ids[i] = i + 1;
        pthread_create(&w_threads[i], NULL, writer, &w_ids[i]);
    }

    // Join (not reachable due to infinite loop)
    for (int i = 0; i < NUM_READERS; i++)
        pthread_join(r_threads[i], NULL);
    for (int i = 0; i < NUM_WRITERS; i++)
        pthread_join(w_threads[i], NULL);

    return 0;
}
*/


```

## `thread_programs/t61.c`

```c
//61.Write a C program to create a thread that prints the even numbers between 1 and 20?
#include <stdio.h>
#include <pthread.h>

// Thread function to print even numbers
void* print_even_numbers(void* arg) 
{
    for (int i = 1; i <= 20; i++) 
    {
        if (i % 2 == 0) 
        {
            printf("Even: %d\n", i);
        }
    }
    return NULL;
}

int main() 
{
    pthread_t thread;
 
    pthread_create(&thread, NULL, print_even_numbers, NULL);

    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t62.c`

```c
//62.Develop a C program to create two threads that print odd and even numbers alternately?
/*
#include <stdio.h>
#include <pthread.h>
#include <unistd.h>

#define LIMIT 20

int counter = 1;
pthread_mutex_t lock = PTHREAD_MUTEX_INITIALIZER;

void* print_odd(void* arg) 
{
    while (counter <= LIMIT) 
    {
        pthread_mutex_lock(&lock);
        if (counter % 2 != 0) 
        {
            printf("Odd : %d\n", counter);
            counter++;
        }
        pthread_mutex_unlock(&lock);
        usleep(100); // small delay to allow context switch
    }
    return NULL;
}

void* print_even(void* arg) 
{
    while (counter <= LIMIT) 
    {
        pthread_mutex_lock(&lock);
        if (counter % 2 == 0) 
        {
            printf("Even: %d\n", counter);
            counter++;
        }
        pthread_mutex_unlock(&lock);
        usleep(100); // small delay to allow context switch
    }
    return NULL;
}

int main() 
{
    pthread_t odd_thread, even_thread;

    pthread_create(&odd_thread, NULL, print_odd, NULL);
    pthread_create(&even_thread, NULL, print_even, NULL);

    pthread_join(odd_thread, NULL);
    pthread_join(even_thread, NULL);

    pthread_mutex_destroy(&lock);
    return 0;
}
*/
#include <stdio.h>
#include <pthread.h>

int counter = 1;
#define LIMIT 20
pthread_mutex_t lock = PTHREAD_MUTEX_INITIALIZER;

void* print_odd(void* arg) 
{
    while (1) 
    {
        pthread_mutex_lock(&lock);
        if (counter > LIMIT) 
        {
            pthread_mutex_unlock(&lock);
            break;
        }
        if (counter % 2 != 0) 
        {
            printf("Odd : %d\n", counter);
            counter++;
        }
        pthread_mutex_unlock(&lock);
    }
    return NULL;
}

void* print_even(void* arg) 
{
    while (1) 
    {
        pthread_mutex_lock(&lock);
        if (counter > LIMIT) 
        {
            pthread_mutex_unlock(&lock);
            break;
        }
        if (counter % 2 == 0) 
        {
            printf("Even: %d\n", counter);
            counter++;
        }
        pthread_mutex_unlock(&lock);
    }
    return NULL;
}

int main() 
{
    pthread_t t1, t2;

    pthread_create(&t1, NULL, print_odd, NULL);
    pthread_create(&t2, NULL, print_even, NULL);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    pthread_mutex_destroy(&lock);
    return 0;
}


```

## `thread_programs/t63.c`

```c
//63.Implement a C program to create a thread that calculates the sum of squares of numbers from 1 to 10
/*
#include <stdio.h>
#include <pthread.h>

int sum = 0;  // Global variable shared between threads

// Thread function to calculate sum of squares
void* sum_of_squares(void* arg) 
{
    for (int i = 1; i <= 10; i++) 
    {
        sum += i * i;
    }
    return NULL;
}

int main() 
{
    pthread_t thread;

 
    pthread_create(&thread, NULL, sum_of_squares, NULL);

    
    pthread_join(thread, NULL);

    // Print the result
    printf("Sum of squares from 1 to 10 = %d\n", sum);

    return 0;
}
*/
#include <stdio.h>
#include <pthread.h>

// Thread function to calculate sum of squares
void* sum_of_squares(void* arg) 
{
    int* result = (int*)malloc(sizeof(int));
    *result = 0;
    for (int i = 1; i <= 10; i++) 
    {
        *result += i * i;
    }
    return result;  // Return result using pointer
}

int main() 
{
    pthread_t thread;
    int* result;
 
    pthread_create(&thread, NULL, sum_of_squares, NULL);

    // Wait for the thread and get result
    pthread_join(thread, (void**)&result);

    printf("Sum of squares from 1 to 10 = %d\n", *result);

    // Free the memory
    free(result);

    return 0;
}


```

## `thread_programs/t64.c`

```c
//64.Write a C program to create a thread that calculates the product of numbers from 1 to 5
#include <stdio.h>
#include <pthread.h>

int product = 1;  // Global variable

void* calculate_product(void* arg) 
{
    for (int i = 1; i <= 5; i++) 
    {
        product *= i;
    }
    return NULL;
}

int main() 
{
    pthread_t thread;
 
    pthread_create(&thread, NULL, calculate_product, NULL);

    pthread_join(thread, NULL);

    printf("Product of numbers from 1 to 5 = %d\n", product);

    return 0;
}


```

## `thread_programs/t65.c`

```c
//65.Develop a C program to create a thread that prints the first 10 terms of the Fibonacci sequence
#include <stdio.h>
#include <pthread.h>

#define LIMIT 10

void* print_fibonacci(void* arg) 
{
    int a = 0, b = 1, next;

    printf("Fibonacci Series (first %d terms):\n", LIMIT);
    for (int i = 1; i <= LIMIT; i++) 
    {
        printf("%d ", a);
        next = a + b;
        a = b;
        b = next;
    }
    printf("\n");
    return NULL;
}

int main() 
{
    pthread_t thread;

    pthread_create(&thread, NULL, print_fibonacci, NULL);

    pthread_join(thread, NULL);

    return 0;
}

/*
#include <stdio.h>
#include <pthread.h>

#define LIMIT 10

// Recursive function to calculate Fibonacci number at position n
int fibonacci(int n)
{
    if (n == 0)
        return 0;
    else if (n == 1)
        return 1;
    else
        return fibonacci(n - 1) + fibonacci(n - 2);
}

// Thread function
void* print_fibonacci_recursive(void* arg) 
{
    printf("Fibonacci Series (first %d terms, recursively):\n", LIMIT);
    for (int i = 0; i < LIMIT; i++) 
    {
        printf("%d ", fibonacci(i));
    }
    printf("\n");
    return NULL;
}

int main() 
{
    pthread_t thread;
    
    pthread_create(&thread, NULL, print_fibonacci_recursive, NULL);
  
    pthread_join(thread, NULL);

    return 0;
}
*/

```

## `thread_programs/t66.c`

```c
//66.Write a C program to create a thread that finds the maximum element in a given array
/*
#include <stdio.h>
#include <pthread.h>

#define SIZE 10

int arr[SIZE] = {12, 45, 2, 89, 33, 77, 10, 5, 99, 23};  // Sample array
int max_value;  // To store the maximum

// Thread function to find the maximum value
void* find_max(void* arg) {
    max_value = arr[0];
    for (int i = 1; i < SIZE; i++) 
    {
        if (arr[i] > max_value) 
        {
            max_value = arr[i];
        }
    }
    return NULL;
}

int main()
{
    pthread_t thread;

    pthread_create(&thread, NULL, find_max, NULL);

    pthread_join(thread, NULL);

    printf("Maximum element in the array: %d\n", max_value);

    return 0;
}
*/
#include <stdio.h>
#include <pthread.h>

#define MAX_SIZE 100

int arr[MAX_SIZE];
int n; 
int max_value; 

void* find_max(void* arg) 
{
    max_value = arr[0];
    for (int i = 1; i < n; i++) 
    {
        if (arr[i] > max_value) 
        {
            max_value = arr[i];
        }
    }
    return NULL;
}

int main() 
{
    pthread_t thread;

    printf("Enter the number of elements: ");
    scanf("%d", &n);

    if (n <= 0 || n > MAX_SIZE) 
    {
        printf("Invalid size.\n");
        return 1;
    }

    printf("Enter %d integers:\n", n);
    for (int i = 0; i < n; i++) 
    {
        scanf("%d", &arr[i]);
    }
    
    pthread_create(&thread, NULL, find_max, NULL);
  
    pthread_join(thread, NULL);

    printf("Maximum element in the array is: %d\n", max_value);

    return 0;
}


```

## `thread_programs/t67.c`

```c
//67.Implement a C program to create a thread that prints the ASCII values of characters in a given string?
#include <stdio.h>
#include <pthread.h>
#include <string.h>

char str[100]; // Global string to hold user input

void* print_ascii(void* arg) 
{
    printf("Character : ASCII Value\n");
    for (int i = 0; i < strlen(input); i++) 
    {
        printf("    %c     :     %d\n", str[i], str[i]);
    }
    return NULL;
}

int main() 
{
    pthread_t thread;

    printf("Enter a string: ");
    fgets(str, sizeof(str), stdin);

    // Remove trailing newline if any
    size_t len = strlen(input);
    if (len > 0 && str[len - 1] == '\n')
        str[len - 1] = '\0';
    
    pthread_create(&thread, NULL, print_ascii, NULL);
 
    pthread_join(thread, NULL);

    return 0;
}




```

## `thread_programs/t68.c`

```c
//68.Develop a C program to create a thread that calculates the sum of all prime numbers up to a given limit?
#include <stdio.h>
#include <pthread.h>

int limit;      
int prime_sum = 0; 

int is_prime(int n) 
{
    if (n < 2) return 0;
    for (int i = 2; i * i <= n; i++) 
    {
        if (n % i == 0)
        {
            return 0;
        }
    }
    return 1;
}

void* sum_of_primes(void* arg) 
{
    for (int i = 2; i <= limit; i++) 
    {
        if (is_prime(i)) 
        {
            prime_sum += i;
        }
    }
    return NULL;
}

int main() 
{
    pthread_t thread;

    printf("Enter the limit: ");
    scanf("%d", &limit);
   
    pthread_create(&thread, NULL, sum_of_primes, NULL);
    
    pthread_join(thread, NULL);

    printf("Sum of all prime numbers up to %d = %d\n", limit, prime_sum);

    return 0;
}



```

## `thread_programs/t69.c`

```c
//69.Write a C program to create a thread that calculates the area of a circle using a given radius
/*

#include <stdio.h>
#include <pthread.h>

float radius;       // User input
float area_result;  // To store result

void* calculate_area(void* arg) 
{
    area_result = 3.14159 * radius * radius;
    return NULL;
}

int main() 
{
    pthread_t thread;

    printf("Enter the radius of the circle: ");
    scanf("%f", &radius);
    
    pthread_create(&thread, NULL, calculate_area, NULL);
  
    pthread_join(thread, NULL);

    printf("Area of the circle with radius %.2f = %.2f\n", radius, area_result);

    return 0;
}
*/
#include <stdio.h>
#include <pthread.h>

void* calculate_area(void* arg) 
{
    float radius = *(float*)arg;
    float area = 3.14159 * radius * radius;
    printf("Area of the circle with radius %.2f = %.2f\n", radius, area);
    return NULL;
}

int main() 
{
    pthread_t thread;
    float radius;

    printf("Enter the radius of the circle: ");
    scanf("%f", &radius);

    pthread_create(&thread, NULL, calculate_area, &radius);
    
    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t7.c`

```c
//7.Implement a C program to create a thread that calculates the square of a number?
#include <stdio.h>
#include <pthread.h>

// Thread function
void* calculate_square(void* arg) 
{
    int num;
    printf("Enter a number: ");
    scanf("%d", &num);

    int square = num * num;
    printf("Square of %d is: %d\n", num, square);

    return NULL;
}

int main() 
{
    pthread_t thread;
 
    pthread_create(&thread, NULL, calculate_square, NULL);

    pthread_join(thread, NULL);

    return 0;
}

/*#include <stdio.h>
#include <pthread.h>

// Thread function
void* calculate_square(void* arg) 
{
    int num = *(int*)arg;
    int square = num * num;
    printf("Square of %d is: %d\n", num, square);
    return NULL;
}

int main() 
{
    pthread_t thread;
    int number;

    printf("Enter a number: ");
    scanf("%d", &number);

    pthread_create(&thread, NULL, calculate_square, &number);
  
    pthread_join(thread, NULL);

    return 0;
}
*/

```

## `thread_programs/t70.c`

```c
//70.Develop a C program to create a thread that calculates the average of a given array of floating-point numbers
#include <stdio.h>
#include <pthread.h>

#define MAX 100

float arr[MAX];
int n;               
float average = 0.0; 

void* calculate_average(void* arg) 
{
    float sum = 0.0;
    for (int i = 0; i < n; i++) 
    {
        sum += arr[i];
    }
    average = sum / n;
    return NULL;
}

int main() 
{
    pthread_t thread;

    printf("Enter the number of elements: ");
    scanf("%d", &n);

    if (n <= 0 || n > MAX) 
    {
        printf("Invalid size.\n");
        return 1;
    }

    printf("Enter %d floating-point numbers:\n", n);
    for (int i = 0; i < n; i++) 
    {
        scanf("%f", &arr[i]);
    }

    pthread_create(&thread, NULL, calculate_average, NULL);
    
    pthread_join(thread, NULL);

    printf("Average = %.2f\n", average);

    return 0;
}
/*
#include <stdio.h>
#include <pthread.h>

// Define fixed input
float arr[] = {2.5, 3.5, 4.0, 5.5, 6.5};
int n = sizeof(arr) / sizeof(arr[0]);
float average = 0.0;

// Thread function to calculate average
void* calculate_average(void* arg) 
{
    float sum = 0.0;
    for (int i = 0; i < n; i++) 
    {
        sum += arr[i];
    }
    average = sum / n;
    return NULL;
}

int main() 
{
    pthread_t thread;
    
    pthread_create(&thread, NULL, calculate_average, NULL);
    
    pthread_join(thread, NULL);

    printf("Average = %.2f\n", average);

    return 0;
}
*/

```

## `thread_programs/t71.c`

```c
//71.Implement a C program to create a thread that prints the factors of a given number?
#include <stdio.h>
#include <pthread.h>

int number = 36; // You can change this number as per user.

void* print_factors(void* arg) 
{
    printf("Factors of %d are:\n", number);
    for (int i = 1; i <= number; i++) 
    {
        if (number % i == 0) 
        {
            printf("%d ", i);
        }
    }
    printf("\n");
    return NULL;
}

int main() 
{
    pthread_t thread;
    
    pthread_create(&thread, NULL, print_factors, NULL);
    
    pthread_join(thread, NULL);

    return 0;
}



```

## `thread_programs/t72.c`

```c
//72.Develop a C program to create a thread that prints the English alphabet in uppercase?
#include <stdio.h>
#include <pthread.h>

// Thread function to print uppercase A-Z
void* print_uppercase_alphabet(void* arg)
{
    printf("Uppercase English Alphabet:\n");
    for (char ch = 'A'; ch <= 'Z'; ch++) 
    {
        printf("%c ", ch);
    }
    printf("\n");
    return NULL;
}

int main() 
{
    pthread_t thread;
    
    pthread_create(&thread, NULL, print_uppercase_alphabet, NULL);
  
    pthread_join(thread, NULL);

    return 0;
}
/*
 //Thread Prints Alphabet from Array
#include <stdio.h>
#include <pthread.h>

char uppercase[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

// Thread function to print letters from array
void* print_uppercase(void* arg) {
    printf("Uppercase English Alphabet:\n");
    for (int i = 0; uppercase[i] != '\0'; i++) {
        printf("%c ", uppercase[i]);
    }
    printf("\n");
    return NULL;
}

int main() {
    pthread_t thread;

 
    pthread_create(&thread, NULL, print_uppercase, NULL);

    pthread_join(thread, NULL);

    return 0;
}
*/

```

## `thread_programs/t73.c`

```c
//73.Implement a C program to create a thread that checks if a given number is a perfect square?
#include <stdio.h>
#include <pthread.h>

int number;  

void* check_perfect_square(void* arg) 
{
    int isPerfect = 0;
    for (int i = 1; i * i <= number; i++) 
    {
        if (i * i == number) 
        {
            isPerfect = 1;
            break;
        }
    }

    if (isPerfect) 
    {
        printf("%d is a perfect square.\n", number);
    } 
    else 
    {
        printf("%d is not a perfect square.\n", number);
    }

    return NULL;
}

int main() 
{
    pthread_t thread;
   
    printf("Enter a number: ");
    scanf("%d", &number);
    
    pthread_create(&thread, NULL, check_perfect_square, NULL);
  
    pthread_join(thread, NULL);

    return 0;
}



```

## `thread_programs/t74.c`

```c
//74.Implement a C program to create a thread that checks if a given number is divisible by another given number

#include <stdio.h>
#include <pthread.h>

int num1, num2;  // num1 is dividend, num2 is divisor

void* check_divisibility(void* arg) 
{
    if (num2 == 0) 
    {
        printf("Division by zero is not allowed.\n");
    } 
    else if (num1 % num2 == 0) 
    {
        printf("%d is divisible by %d.\n", num1, num2);
    } 
    else 
    {
        printf("%d is not divisible by %d.\n", num1, num2);
    }
    return NULL;
}

int main() 
{
    pthread_t thread;
   
    printf("Enter the first number (dividend): ");
    scanf("%d", &num1);
    printf("Enter the second number (divisor): ");
    scanf("%d", &num2);
    
    pthread_create(&thread, NULL, check_divisibility, NULL);
    
    pthread_join(thread, NULL);

    return 0;
}



```

## `thread_programs/t75.c`

```c
//75.Develop a C program to create a thread that prints the multiplication table of a given number up to a given limit? 
#include <stdio.h>
#include <pthread.h>

int number, limit;  // Inputs from user

void* print_table(void* arg) 
{
    printf("Multiplication table of %d up to %d:\n", number, limit);
    for (int i = 1; i <= limit; i++) 
    {
        printf("%d x %d = %d\n", number, i, number * i);
    }
    return NULL;
}

int main() 
{
    pthread_t thread;

    printf("Enter a number: ");
    scanf("%d", &number);
    printf("Enter the limit: ");
    scanf("%d", &limit);
 
    pthread_create(&thread, NULL, print_table, NULL);

    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t76.c`

```c
//76.Develop a C program to create a thread that prints the current system time?
#include <stdio.h>
#include <pthread.h>
#include <time.h>

void* print_system_time(void* arg) 
{
    time_t now;
    time(&now); // Get current time

    // Convert to local time string
    char* time_str = ctime(&now);

    printf("Current System Time: %s", time_str);
    return NULL;
}

int main() 
{
    pthread_t thread;
   
    pthread_create(&thread, NULL, print_system_time, NULL);
    
    pthread_join(thread, NULL);

    return 0;
}


```

## `thread_programs/t77.c`

```c
//77.Write a C program to create two threads that increment and decrement a shared variable,respectively, using mutex locks.
#include <stdio.h>
#include <pthread.h>

int shared_var = 0;             // Shared variable
pthread_mutex_t lock;           // Mutex lock

void* increment_thread(void* arg)
{
    for (int i = 0; i < 5; i++) 
    {
        pthread_mutex_lock(&lock);
        shared_var++;
        printf("Increment Thread: shared_var = %d\n", shared_var);
        pthread_mutex_unlock(&lock);
    }
    return NULL;
}

// Thread function to decrement
void* decrement_thread(void* arg) 
{
    for (int i = 0; i < 5; i++)
    {
        pthread_mutex_lock(&lock);
        shared_var--;
        printf("Decrement Thread: shared_var = %d\n", shared_var);
        pthread_mutex_unlock(&lock);
    }
    return NULL;
}

int main() 
{
    pthread_t inc_thread, dec_thread;

  
    pthread_mutex_init(&lock, NULL);

    
    pthread_create(&inc_thread, NULL, increment_thread, NULL);
    pthread_create(&dec_thread, NULL, decrement_thread, NULL);

    // Wait for threads to complete
    pthread_join(inc_thread, NULL);
    pthread_join(dec_thread, NULL);
   
    pthread_mutex_destroy(&lock);

    printf("Final value of shared_var = %d\n", shared_var);
    return 0;
}



```

## `thread_programs/t78.c`

```c
//78.Develop a C program to create a thread that prints numbers from 10 to 1 in descending order using mutex locks.
#include <stdio.h>
#include <pthread.h>

pthread_mutex_t lock;  // Declare mutex

void* print_descending(void* arg) 
{
    pthread_mutex_lock(&lock);

    printf("Descending numbers from 10 to 1:\n");
    for (int i = 10; i >= 1; i--) 
    {
        printf("%d ", i);
    }
    printf("\n");

    pthread_mutex_unlock(&lock);  
    return NULL;
}

int main() 
{
    pthread_t thread;
  
    pthread_mutex_init(&lock, NULL);
    
    pthread_create(&thread, NULL, print_descending, NULL);
    
    pthread_join(thread, NULL);
    
    pthread_mutex_destroy(&lock);

    return 0;
}



```

## `thread_programs/t79_1.c`

```c
//Using pthread_create()
#include <stdio.h>
#include <pthread.h>

void* thread_func(void* arg) 
{
    printf("Hello from pthread thread!\n");
    return NULL;
}

int main() 
{
    pthread_t tid;
    pthread_create(&tid, NULL, thread_func, NULL);
    pthread_join(tid, NULL);
    printf("Main thread finished.\n");
    return 0;
}



```

## `thread_programs/t79_2.c`

```c
// Using clone()
#define _GNU_SOURCE
#include <sched.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

#define STACK_SIZE 1024 * 1024

int thread_func(void* arg) 
{
    printf("Hello from clone thread!\n");
    return 0;
}

int main() 
{
    char* stack = malloc(STACK_SIZE);
    if (!stack) 
    {
        perror("malloc");
        exit(1);
    }

  // using clone with CLONE_VM to share memory
    pid_t pid = clone(thread_func, stack + STACK_SIZE, CLONE_VM | SIGCHLD, NULL);
    if (pid == -1) 
    {
        perror("clone");
        exit(1);
    }

    sleep(1); 
    printf("Main process finished.\n");

    free(stack);
    return 0;
}


```

## `thread_programs/t8.c`

```c
//8.Write a C program to create a thread that prints the current date and time
#include <stdio.h>
#include <pthread.h>
#include <time.h>

void* print_datetime(void* arg) 
{
    time_t now;
    struct tm *info;

    time(&now);
    info = localtime(&now);

    printf("Current date and time: %s", asctime(info));
    return NULL;
}

int main() 
{
    pthread_t thread;
    
    pthread_create(&thread, NULL, print_datetime, NULL);
  
    pthread_join(thread, NULL);

    return 0;
}
/*
 * #include <stdio.h>
#include <pthread.h>
#include <time.h>

// Thread function
void* thread_task(void* arg) 
{
    printf("Thread is running...\n");
    return NULL;
}

int main() 
{
    pthread_t thread;
    time_t now;
    struct tm *info;

    // Get current time in main
    time(&now);
    info = localtime(&now);

    printf("Main: Current date and time: %s", asctime(info));

    
    pthread_create(&thread, NULL, thread_task, NULL);

  
    pthread_join(thread, NULL);

    return 0;
}
*/

```

## `thread_programs/t80.c`

```c
//80.why a stack grows?
#include <stdio.h>

void functionC() 
{
    int c = 3;
    printf("Inside functionC: c = %d\n", c);
}

void functionB() 
{
    int b = 2;
    functionC();
}

void functionA() 
{
    int a = 1;
    functionB();
}

int main() 
{
    functionA();
    return 0;
}


```

## `thread_programs/t82.c`

```c
//82.can you fetch the thread entry point return value in your main thread?
#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>

void* add_numbers(void* arg) 
{
    int* result = malloc(sizeof(int)); // Create space to hold result
    *result = 5 + 10; // Do some work (5 + 10)
    return result; // Give result back
}

int main() 
{
    pthread_t thread;
    int* final_result;
 
    pthread_create(&thread, NULL, add_numbers, NULL);

    pthread_join(thread, (void**)&final_result);

    printf("The result from thread is: %d\n", *final_result);

    free(final_result);

    return 0;
}


```

## `thread_programs/t83.c`

```c
//



```


```c

## `thread_programs/t9.c`

```c
//9.Develop a C program to create a thread that checks if a number is prime or not.
#include <stdio.h>
#include <pthread.h>

// Thread function
void* check_prime(void* arg) 
{
    int num, is_prime = 1;

   
    printf("Enter a number: ");
    scanf("%d", &num);

    if (num <= 1) 
    {
        printf("%d is not a prime number (prime numbers are greater than 1).\n", num);
        return NULL;
    }

    for (int i = 2; i * i <= num; i++) 
    {
        if (num % i == 0) 
        {
            is_prime = 0;
            break;
        }
    }

    if (is_prime)
        printf("%d is a prime number.\n", num);
    else
        printf("%d is not a prime number.\n", num);

    return NULL;
}

int main() 
{
    pthread_t thread;
     
    pthread_create(&thread, NULL, check_prime, NULL);

    pthread_join(thread, NULL);

    return 0;
}


```

