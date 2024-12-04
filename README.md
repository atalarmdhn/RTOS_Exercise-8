# **STM32 RTOS LED Control Project**

## **Project Overview**
This project demonstrates the use of FreeRTOS on an STM32 microcontroller to control multiple LEDs (Green, Red, and Orange). The system is designed to showcase the following features:
- Task management in an RTOS environment.
- Use of semaphores for accessing shared resources safely.
- GPIO operations for LED control.

---

## **Features**
1. **LED Control**: 
   - Green, Red, and Orange LEDs are toggled using separate tasks.
   - Tasks operate at different priorities and intervals.

2. **Semaphore Implementation**:
   - A binary semaphore ensures synchronized access to a shared resource.

3. **Shared Resource Access**:
   - The `accessSharedData` function demonstrates safe access to shared data using semaphores.

4. **RTOS Task Priorities**:
   - Tasks are assigned different priorities to handle operations more effectively:
     - Orange LED Task: Highest priority.
     - Green and Red LED Tasks: Normal priority.

---

## **Hardware Requirements**
- STM32 Microcontroller (tested on STM32 family boards).
- LEDs connected to GPIO pins:
  - **Green LED**: GPIOA_PIN_0
  - **Red LED**: GPIOA_PIN_1
  - **Orange LED**: GPIOA_PIN_3

---

## **Software Requirements**
- STM32CubeMX for peripheral initialization.
- STM32 HAL Library.
- FreeRTOS for real-time operating system support.
- IDE: STM32CubeIDE or any ARM GCC-compatible IDE.

---

## **Project Structure**
### **Main Components:**
1. **Threads (Tasks):**
   - `GreenLedTask`: Controls the Green LED. Accesses the shared resource with a delay of 200ms.
   - `RedLedTask`: Controls the Red LED. Accesses the shared resource with a delay of 550ms.
   - `OrangeLedTask`: Toggles the Orange LED with a 50ms interval.

2. **Semaphore:**
   - `CriticalResourceSemaphore`: Ensures safe access to the `accessSharedData` function by the Green and Red LED tasks.

3. **Shared Resource:**
   - `accessSharedData`: Updates the `startFlag` and toggles an additional GPIO pin for demonstration purposes.

4. **System Initialization:**
   - `SystemClock_Config`: Configures the system clock.
   - `MX_GPIO_Init`: Sets up GPIO pins for LEDs.
   - `osKernelStart`: Starts the FreeRTOS kernel.

---

## **How to Run**
1. **Set Up the Project:**
   - Import the code into STM32CubeIDE or your preferred IDE.
   - Configure the FreeRTOS middleware in STM32CubeMX.

2. **Connect Hardware:**
   - Connect the LEDs to the specified GPIO pins on the STM32 board.

3. **Build and Flash:**
   - Compile the project and flash it to the STM32 microcontroller.

4. **Observe the Behavior:**
   - Green and Red LEDs toggle with delays while accessing the shared resource.
   - Orange LED toggles at a faster rate independently.

---

## **Code Details**
### **Semaphore Usage**
```c
osSemaphoreWait(CriticalResourceSemaphoreHandle, WaitTimeMilliseconds);
accessSharedData();
osSemaphoreRelease(CriticalResourceSemaphoreHandle);
```
- Ensures synchronized access to `accessSharedData` by the Green and Red LED tasks.

### **GPIO Operations**
```c
HAL_GPIO_WritePin(GPIOA, GPIO_PIN_0, GPIO_PIN_SET);  // Turn on Green LED
HAL_GPIO_WritePin(GPIOA, GPIO_PIN_0, GPIO_PIN_RESET);  // Turn off Green LED
HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_3);  // Toggle Orange LED
```
- Controls GPIO pins to toggle LEDs.

---

## **Customization**
- **Adjust LED Timing**:
  Modify the `osDelay` values in tasks to change the blinking speed.

- **Add More Tasks**:
  New tasks can be added using `osThreadDef` and `osThreadCreate`.

- **Change GPIO Pins**:
  Update the `MX_GPIO_Init` function to use different GPIO pins.

---

## **Potential Improvements**
- Add more complex shared resource management (e.g., queues).
- Implement a button-triggered interrupt to change LED patterns.
- Expand with additional peripherals like UART or timers.

---

## **License**
This project is licensed under the terms described in the LICENSE file in the root directory. If no LICENSE file is found, it is provided "AS-IS."
