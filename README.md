# STM32-HCSR04-Ultrasonic-Distance-Measurement
STM32F446RE embedded systems project demonstrating HC-SR04 ultrasonic sensor interfacing, distance measurement using TIM4 microsecond timer, UART data transmission, and LED-based obstacle indication using STM32 HAL libraries.

---

## Overview

This project demonstrates ultrasonic distance measurement using the HC-SR04 sensor with the STM32F446RE microcontroller.

The system:

- Generates a trigger pulse for the ultrasonic sensor
- Measures echo pulse duration using TIM4
- Calculates object distance using the speed of sound
- Displays distance data on UART terminal
- Controls onboard LED based on distance threshold

The project is developed using:

- STM32CubeMX
- STM32CubeIDE
- STM32 HAL Library

---

## Features

- HC-SR04 ultrasonic sensor interfacing
- Distance measurement using time-of-flight method
- Microsecond delay generation using TIM4
- UART serial communication
- LED-based obstacle indication
- STM32 HAL-based implementation

---

## Hardware Requirements

- STM32 Nucleo-F446RE Development Board
- HC-SR04 Ultrasonic Sensor
- Jumper Wires
- USB Type-A to Mini-B Cable

---

## Software Requirements

- STM32CubeIDE
- STM32CubeMX

---

## Peripheral Configuration

| Peripheral | Configuration |
|------------|----------------|
| PA5 | GPIO Output (LED) |
| PA9 | GPIO Output (TRIG) |
| PA8 | GPIO Input (ECHO) |
| USART2 | UART Communication |
| TIM4 | Microsecond Delay Timer |

---

## Working Principle

The HC-SR04 sensor measures distance using ultrasonic waves.

### Measurement Process

1. STM32 sends a 10 µs trigger pulse
2. Sensor transmits ultrasonic wave
3. Echo signal returns after reflection
4. STM32 measures echo pulse duration
5. Distance is calculated using time-of-flight equation

### Distance Formula

```text
Distance = (Speed of Sound × Time) / 2
```

Where:

- Speed of sound ≈ 343 m/s
- Division by 2 accounts for round-trip travel

---

## Timer Configuration

TIM4 is configured with:

| Parameter | Value |
|------------|--------|
| System Clock | 84 MHz |
| Prescaler | 84 - 1 |
| Timer Resolution | 1 µs |

This enables precise microsecond-level timing for ultrasonic pulse measurements.

---

## Source Code

### Trigger Pulse Generation

```c
HAL_GPIO_WritePin(TRIG_GPIO_Port,
                  TRIG_Pin,
                  GPIO_PIN_SET);

usDelay(10);

HAL_GPIO_WritePin(TRIG_GPIO_Port,
                  TRIG_Pin,
                  GPIO_PIN_RESET);
```

---

### Distance Measurement

```c
while(HAL_GPIO_ReadPin(ECHO_GPIO_Port,
                       ECHO_Pin) == GPIO_PIN_SET)
{
    numTicks++;

    usDelay(2);
}

distance =
(numTicks + 0.0f) * 2.8 * speedOfSound;
```

---

### UART Transmission

```c
sprintf(uartBuf,
        "Distance (cm) = %.1f\r\n",
        distance);

HAL_UART_Transmit(&huart2,
                  (uint8_t *)uartBuf,
                  strlen(uartBuf),
                  100);
```

---

## LED Indication Logic

| Distance | LED Status |
|------------|-------------|
| ≤ 20 cm | ON |
| > 20 cm | OFF |

The onboard LED acts as a simple obstacle indicator.

---

## Build and Run

1. Open the project in STM32CubeIDE
2. Configure peripherals using STM32CubeMX
3. Connect HC-SR04 sensor
4. Build the project
5. Flash firmware to STM32 board
6. Open UART serial terminal
7. Observe distance measurements and LED indication

---

## Expected Output

UART Terminal Output:

```text
Distance (cm) = 12.4
Distance (cm) = 18.7
Distance (cm) = 25.1
```

- LED turns ON when object is within 20 cm
- LED turns OFF when object moves farther away

---

## Learning Outcomes

- Ultrasonic sensor interfacing
- GPIO input/output configuration
- Timer-based microsecond delays
- UART communication
- Distance measurement techniques
- Embedded systems debugging
- STM32 HAL programming

---

## Future Improvements

- Add LCD/OLED distance display
- Implement buzzer-based obstacle alert
- Use timer input capture mode
- Add moving average filtering
- Develop obstacle avoidance robot

---

## Author

**Ayush Jangra**  
ECE Student | Chitkara University

---

## License

This project is created for educational and academic purposes.
