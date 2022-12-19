## Initialization

In FreeRTOS, Semaphores are implemented based on queue mechanism. There are 4 types of semaphores in FreeRTOS:
- Binary Semaphore
- Counting Semaphore
- Mutex
- Recursive

The working of Binary Semaphore is pretty straightforward. A binary semaphore is called binary because either it is 1 or 0. So a Task either have the semaphore or it simple doesn't.

Semaphores are often better used as a signaling mechanism to other threads that some common resource is ready to be used. you'll normally see semaphores used in a producer consumer design where one or more tasks add data to a shared resource and one or more tasks remove data from that resource in order to use it. The shared resource is often something like a buffer or a linked list that can grow unbounded. Note that if access to the resource is not atomic then you'll likely still need to protect it with a mutex in addition to using semaphores to notify the consumers. Whenever a producer task adds something to the buffer it calls semaphore give which increases the value of the semaphore by one you can limit the size of the shared resource by limiting the maximum value of the semaphore. The tasks can check the value of the semaphore and if it's at its max it cannot add anything to the buffer.
Other tasks may also try to read from the shared resource these tasks are known as consumers.

> Semaphores including binary semaphores are useful in interrupt service routines where you want to signal to other tasks that some data is ready. You do not want to use a mutex in an interrupt service routine as it's bad form to have such a routine be blocked for any amount of time waiting for a resource to free up. Note that giving and taking is not necessarily common terminology in the world of semaphores.


```C
//If it returns a null value, this means there isn't enough heap memory available for freeRTOS to allocate the semaphore data structures.
SemaphoreHandle_t xSemaphoreCreatebinary();

//This macro must have previously been created with a call to xSemaphoreCreatebinary()
//This must not be used from an ISR. See xSemaphoreGiveFromISR() for an alternative which can be used from an ISR.
BaseType_t xSemaphoreGive( SemaphoreHandle_t xSemaphore ); 
if ( xSemaphore != NULL ) {
    if ( xSemaphoreGive(xSemaphore) == pdTRUE ) {
        // We would expect this call to fail because we cannot give
        // a semaphore without first "taking" it!
    }
}
```


```C
//This macro must have previously been created with a call to xSemaphoreCreatebinary()
//This macro musnt be called from an ISR. xQueueReceiveFromISR() can be used to take a semaphore from within an interrupt if required.
//pdTRUE if the semaphore was obtained. pdFALSE if xTicksToWait expired without the semaphore becoming available.
xSemaphoreTake(SemaphoreHandle_t xSemaphore, TickType_t xTicksToWait);
if( xSemaphore != NULL ) {
    /* See if we can obtain the semaphore.  If the semaphore is not
    available wait 10 ticks to see if it becomes free. */
    if (xSemaphoreTake(xSemaphore, (TickType_t)10) == pdTRUE ) {
        /* We were able to obtain the semaphore and can now access the
        shared resource. */
        /* We have finished accessing the shared resource.  Release the
        semaphore. */
        xSemaphoreGive(xSemaphore);
    } else {
        /* We could not obtain the semaphore and can therefore not access
        the shared resource safely. */
    }
}
```

```C
void IRAM_ATTR(void) {
	static BaseType_t xHigherPriorityTaskWoken = pdFALSE;
	xSemaphoreGiveFromISR(imuSemaphore, &xHigherPriorityTaskWoken);
    if( xHigherPriorityTaskWoken == pdTRUE ) {
        portYIELD_FROM_ISR(); // this wakes up imu_task immediately instead of on next FreeRTOS tick
    }
}
```


## 1.Example

```C
#include "semphr.h"
static SemaphoreHandle_t xBinarySemaphore;

void task1(void *pvParam) {
    for ( ;; ) {
        xSemaphoreTake(xBinarySemaphore, portMAX_DELAY);
        printf("Task1 took semaphore \n");
        xSemaphoreGive(xBinarySemaphore);
        printf("Task1 gave semaphore \n");
    }
}

void task2(void *pvParam) {
    for ( ;; ) {
        xSemaphoreTake(xBinarySemaphore, portMAX_DELAY);
        printf("Task2 took semaphore \n");
        xSemaphoreGive(xBinarySemaphore);
        printf("Task2 gave semaphore \n");
    }
}

void app_main() {
    xBinarySemaphore = xSemaphoreCreateBinary();
    xSemaphoreGive(xBinarySemaphore);
}
```
> In the above example there is two tasks. Both tasks can not execute at the same time. Both taks have the same priority level. Therefore, the FreeRTOS scheduler will schedule both tasks in a time-sharing or round-robin fashion.


```C
#include "semphr.h"
static SemaphoreHandle_t xInterruptSemaphore;
static SemaphoreHandle_t xBinarySemaphore;

void interruptHandler()  { 
    BaseType_t xHigherPriorityTaskWoken = pdFALSE;
    /*
    * if xHigherPriorityTaskWoken equals pdTRUE, then a context switch should be performed
    */
    xSemaphoreGiveFromISR(interruptSemaphore, &xHigherPriorityTaskWoken);
}


void task1(void *pvParam) {
    for ( ;; ) {
        xSemaphoreTake(interruptSemaphore, portMAX_DELAY);
        printf("Task1 took semaphore \n");
        xSemaphoreGive(xBinarySemaphore);
        printf("Task1 gave semaphore \n");
    }
}

void task2(void *pvParam) {
    for ( ;; ) {
        xSemaphoreTake(xBinarySemaphore, portMAX_DELAY);
        printf("Task2 took semaphore \n");
    }
}

void app_main() {
    xBinarySemaphore = xSemaphoreCreateBinary();
    interruptSemaphore = xSemaphoreCreateBinary();
    xSemaphoreGive(xBinarySemaphore);
}
```
