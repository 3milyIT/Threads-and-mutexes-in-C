# Threads-and-mutexes-in-C

Creating Threads and Synchronization in C

In C programming, threads can be created using the `pthread_create()` function from the `pthread.h` library. Threads allow for concurrent execution of different parts of a program. Additionally, synchronization mechanisms such as mutexes (`pthread_mutex_t`) can be used to coordinate access to shared resources and avoid data races.

1. Write a program that creates a thread using `pthread_create()`. The thread should perform a specific operation, such as displaying the contents of a 10-element array. Afterwards, the previously created thread should be terminated using `pthread_join()` or `pthread_cancel()`.

```c
#include <stdio.h>
#include <pthread.h>

void* countToTen(void* arg) {
    for (int i = 1; i <= 10; i++) {
        // sleep(1);
        printf("%d\n", i);
    }
    pthread_exit(NULL);
}

int main() {
    pthread_t thread;
    pthread_create(&thread, NULL, &countToTen, NULL);
    pthread_join(thread, NULL);
    return 0;
}
```

2. Extend the previous program by introducing a global variable. This global variable should be incremented 10 times simultaneously by both the main thread and the created thread.

```c
#include <stdio.h>
#include <pthread.h>

int globalVariable = 0;
pthread_mutex_t mutex;

void* incrementGlobalVariable(void* arg) {
    for (int i = 0; i < 10; i++) {
        pthread_mutex_lock(&mutex);
        globalVariable++;
        printf("Thread: %d\n", globalVariable);
        pthread_mutex_unlock(&mutex);
    }
    pthread_exit(NULL);
}

int main() {
    pthread_t thread;
    pthread_mutex_init(&mutex, NULL);

    pthread_create(&thread, NULL, &incrementGlobalVariable, NULL);

    for (int i = 0; i < 10; i++) {
        pthread_mutex_lock(&mutex);
        globalVariable++;
        printf("Main Thread: %d\n", globalVariable);
        pthread_mutex_unlock(&mutex);
    }

    pthread_join(thread, NULL);
    pthread_mutex_destroy(&mutex);

    return 0;
}
```

3. Use mutexes (`pthread_mutex_t`) to synchronize and protect the shared global variable. By using `pthread_mutex_lock()` and `pthread_mutex_unlock()`, you can prevent simultaneous access to the variable and ensure proper synchronization.

```c
#include <stdio.h>
#include <pthread.h>

int globalVariable = 0;
pthread_mutex_t mutex;

void* incrementGlobalVariable(void* arg) {
    for (int i = 0; i < 10; i++) {
        pthread_mutex_lock(&mutex);
        globalVariable++;
        printf("Thread: %d\n", globalVariable);
        pthread_mutex_unlock(&mutex);
    }
    pthread_exit(NULL);
}

int main() {
    pthread_t thread;
    pthread_mutex_init(&mutex, NULL);

    pthread_create(&thread, NULL, &incrementGlobalVariable, NULL);

    for (int i = 0; i < 10; i++) {
        pthread_mutex_lock(&mutex);
        globalVariable++;
        printf("Main Thread: %d\n", globalVariable);
        pthread_mutex_unlock(&mutex);
    }

    pthread_join(thread, NULL);
    pthread_mutex_destroy(&mutex);

    return 0;
}
```

These examples demonstrate the creation of threads using `pthread_create()` and the usage of mutexes for synchronization in C programs. By employing threads and synchronization mechanisms, concurrent execution and proper coordination of shared resources can be achieved.

Let's dive into more details about the functions used in the provided examples.

1. **pthread_create**: The `pthread_create` function is used to create a new thread. It takes four parameters: the thread identifier, thread attributes, start routine, and arguments to be passed to the start routine. The start routine is the function that will be executed by the new thread. In the example, `pthread_create(&thread, NULL, &countToTen, NULL);` creates a new thread with the `countToTen` function as the start routine.

2. **pthread_join**: The `pthread_join` function is used to wait for a thread to terminate. It takes two parameters: the thread identifier and the address of a pointer where the exit status of the thread will be stored. Calling `pthread_join(thread, NULL);` in the main thread causes the program to wait until the `countToTen` thread completes its execution.

3. **pthread_cancel**: The `pthread_cancel` function is used to request the cancellation of a thread. When called, it sends a cancellation request to the specified thread, but it's up to the thread to actually terminate. In the given examples, `pthread_cancel` is not used, but you can use it to request the cancellation of the `countToTen` thread.

4. **pthread_mutex_t**: `pthread_mutex_t` is a data type representing a mutex (short for mutual exclusion). A mutex is used to provide mutual exclusion, ensuring that only one thread can access a shared resource at a time. It helps prevent data races and ensures thread-safe access to shared variables. In the examples, `pthread_mutex_t mutex;` declares a mutex variable.

5. **pthread_mutex_init**: The `pthread_mutex_init` function is used to initialize a mutex variable. It takes two parameters: the address of the mutex variable and a pointer to the mutex attributes. In the example, `pthread_mutex_init(&mutex, NULL);` initializes the mutex.

6. **pthread_mutex_lock**: The `pthread_mutex_lock` function is used to lock a mutex. When a thread calls this function, it acquires the mutex and proceeds to access the shared resource. If the mutex is already locked by another thread, the calling thread will be blocked until the mutex is unlocked. In the examples, `pthread_mutex_lock(&mutex);` is used before accessing the shared variable.

7. **pthread_mutex_unlock**: The `pthread_mutex_unlock` function is used to unlock a mutex. It releases the mutex, allowing other threads to acquire it. In the examples, `pthread_mutex_unlock(&mutex);` is called after accessing the shared variable.

8. **pthread_mutex_destroy**: The `pthread_mutex_destroy` function is used to destroy a mutex variable. It releases any resources associated with the mutex. In the examples, `pthread_mutex_destroy(&mutex);` is used to destroy the mutex variable after it is no longer needed.

These functions provide the necessary tools to create threads, synchronize their execution, and protect shared resources from concurrent access. By using functions like `pthread_create`, `pthread_join`, `pthread_mutex_lock`, and `pthread_mutex_unlock`, you can effectively utilize multithreading in your C programs and ensure proper synchronization.

---

Tworzenie wątków i synchronizacja w języku C

W programowaniu w języku C,

 wątki mogą być tworzone za pomocą funkcji `pthread_create()` z biblioteki `pthread.h`. Wątki umożliwiają jednoczesne wykonywanie różnych części programu. Dodatkowo, mechanizmy synchronizacji, takie jak muteksy (`pthread_mutex_t`), mogą być wykorzystywane do koordynacji dostępu do zasobów współdzielonych i unikania wyścigów danych.

1. Napisz program, który tworzy wątek za pomocą funkcji `pthread_create()`. Wątek powinien wykonywać określone operacje, na przykład wyświetlanie zawartości 10-elementowej tablicy. Następnie wcześniej utworzony wątek powinien zostać zakończony za pomocą `pthread_join()` lub `pthread_cancel()`.

```c
#include <stdio.h>
#include <pthread.h>

void* countToTen(void* arg) {
    for (int i = 1; i <= 10; i++) {
        // sleep(1);
        printf("%d\n", i);
    }
    pthread_exit(NULL);
}

int main() {
    pthread_t thread;
    pthread_create(&thread, NULL, &countToTen, NULL);
    pthread_join(thread, NULL);
    return 0;
}
```

2. Rozszerz poprzedni program poprzez wprowadzenie zmiennej globalnej. Ta zmienna globalna powinna być inkrementowana 10 razy jednocześnie przez wątek główny i utworzony wątek.

```c
#include <stdio.h>
#include <pthread.h>

int globalVariable = 0;
pthread_mutex_t mutex;

void* incrementGlobalVariable(void* arg) {
    for (int i = 0; i < 10; i++) {
        pthread_mutex_lock(&mutex);
        globalVariable++;
        printf("Wątek: %d\n", globalVariable);
        pthread_mutex_unlock(&mutex);
    }
    pthread_exit(NULL);
}

int main() {
    pthread_t thread;
    pthread_mutex_init(&mutex, NULL);

    pthread_create(&thread, NULL, &incrementGlobalVariable, NULL);

    for (int i = 0; i < 10; i++) {
        pthread_mutex_lock(&mutex);
        globalVariable++;
        printf("Proces główny: %d\n", globalVariable);
        pthread_mutex_unlock(&mutex);
    }

    pthread_join(thread, NULL);
    pthread_mutex_destroy(&mutex);

    return 0;
}
```

3. Wykorzystaj muteksy (`pthread_mutex_t`) do synchronizacji i ochrony współdzielonej zmiennej globalnej. Poprzez użycie `pthread_mutex_lock()` i `pthread_mutex_unlock()`, można zapobiec jednoczesnemu dostępowi do zmiennej i zapewnić właściwą synchronizację.

```c
#include <stdio.h>
#include <pthread.h>

int globalVariable = 0;
pthread_mutex_t mutex;

void* incrementGlobalVariable(void* arg) {
    for (int i = 0; i < 10; i++) {
        pthread_mutex_lock(&mutex);
        globalVariable++;
        printf("Wątek: %d\n", globalVariable);
        pthread_mutex_unlock(&mutex);
    }
    pthread_exit(NULL);
}

int main() {
    pthread_t thread;
    pthread_mutex_init

(&mutex, NULL);

    pthread_create(&thread, NULL, &incrementGlobalVariable, NULL);

    for (int i = 0; i < 10; i++) {
        pthread_mutex_lock(&mutex);
        globalVariable++;
        printf("Proces główny: %d\n", globalVariable);
        pthread_mutex_unlock(&mutex);
    }

    pthread_join(thread, NULL);
    pthread_mutex_destroy(&mutex);

    return 0;
}
```

Powyższe przykłady demonstrują tworzenie wątków za pomocą `pthread_create()` oraz wykorzystanie muteksów do synchronizacji w programach w języku C. Poprzez zastosowanie wątków i mechanizmów synchronizacji, można osiągnąć równoczesne wykonanie i odpowiednią koordynację zasobów współdzielonych.
