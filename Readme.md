# STM32 Non-Blocking Industrial Fault Simulator

![STM32](https://img.shields.io/badge/MCU-STM32F446RE-blue)
![C](https://img.shields.io/badge/Language-C-green)
![Architecture](https://img.shields.io/badge/Architecture-State_Machine-orange)

## 📌 Overview
This project demonstrates a real-time, non-blocking fault detection system designed for industrial automation environments. Built on an **STM32 ARM Cortex-M4** microcontroller, it eliminates the use of processor-halting functions (like `HAL_Delay()`) by utilizing hardware interrupts (EXTI) and hardware timers (`HAL_GetTick()`). 

The architecture ensures that the processor remains fully available for high-speed tasks (such as reading encoders or PID calculations) while instantly responding to critical hardware alarms (e.g., E-Stop).

## ⚙️ Hardware Requirements
*   **MCU:** STM32 NUCLEO-F446RE Development Board
*   **Actuators:** Green LED (System Normal) & Red LED (Fault Alarm)
*   **Sensors/Inputs:** Industrial Push Button (Simulating a Fault/E-Stop switch)
*   **Miscellaneous:** 330Ω Resistors, Breadboard, Jumper Wires

## 🔌 Pinout & Wiring Configuration

| Component | STM32 Pin | Configuration | Notes |
| :--- | :--- | :--- | :--- |
| **Green LED** | `PA0` | GPIO Output | Toggles every 500ms (Heartbeat) |
| **Red LED** | `PA1` | GPIO Output | Solid ON during FAULT state |
| **Fault Button**| `PA4` | GPIO EXTI4 | Falling Edge Trigger, Internal Pull-up |

## 🧠 System Architecture & How It Works

1. **State Machine Concept:** The system operates strictly within defined states (`STATE_NORMAL` and `STATE_FAULT`), managed by a `volatile` variable to prevent compiler optimization issues during interrupts.
2. **Non-Blocking Delays:** Instead of locking the CPU, the main loop continuously polls the hardware timer (`HAL_GetTick()`) to toggle the Green LED exactly every 500ms.
3. **Hardware Debouncing:** Mechanical button bounces are aggressively filtered inside the `HAL_GPIO_EXTI_Callback` function using a 50ms time window, ensuring that electrical noise does not trigger false system states.

## 🚀 How to Run
1. Clone this repository to your local machine.
2. Open the project directory using **STM32CubeIDE**.
3. Compile the project (`Build`) and flash it to the NUCLEO-F446RE board using the onboard ST-LINK (`Run`).
4. Ground the `PA4` pin to simulate a hardware fault and observe the interrupt response.