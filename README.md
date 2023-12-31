# Multi-Process Drone System

## Contributors
- Seyed Emad Razavi
- Tanvir Rahman Sajal

## Project Overview
Our goal for this project was to implement a multi-process drone system in C which is managed by a master process. This master process oversees the creation and termination of all other processes, which include:

1. **keyboardManager:** Takes user input, setting direction and force for the drone.
2. **droneDynamics:** Calculates the updated position of the drone based on the input.
3. **Window:** Shows the movement of the drone on the user's screen.
4. **watchdog:** Monitors all processes to ensure correct operation.
5. **server:** Handles access to shared memory and communicates with the watchdog.

The processes communicate using shared memory and semaphores, pipes, and the system is designed to run in a Linux environment. The user can control the drone by pressing defined keys on the keyboard, and the drone moves accordingly on the screen. Two Konsoles are displayed; one shows the drone movement and the other for the watchdog, displaying the sent and received signals to ensure everything is working properly.

This is the first part of the complete project. Future enhancements will include targets and obstacles, which the user will need to navigate, grabbing targets and avoiding obstacles.

## Tools Required
- GCC compiler
- A Linux environment (tested only on Linux)
- Ncurses library
- Konsole terminal

## How to Run
The code can be run in the terminal just by typing`make`. The user can command the drone using the keyboard with the following directions:

- `w`: Move up-left
- `e`: Move up
- `r`: Move up-right
- `s`: Move left
- `d`: Stop
- `f`: Move right
- `x`: Move down-left
- `c`: Move down
- `v`: Move down-right

Note: Pressing the same key increases the speed of the drone.

## Components System and Architecture
![System Architecture](https://github.com/Emaaaad/ARP_1ST_TE/blob/main/ARP1%20(1).png)

### Master Process
The `master.c` file acts as the main controller, creating and managing child processes for the drone, server, keyboard, and watchdog. It uses basic fork mechanisms to spawn these processes and tracks their IDs. Through simple inter-process communication using pipes, the master process initiates and terminates child processes.

### Window Process
The `window.c` initializes the ncurses library, sets up color pairs, and installs signal handlers for `SIGINT` and `SIGUSR1`. It reads the drone's position from shared memory and updates the ncurses window accordingly.

### Keyboard Manager Process
The `keyboardManager.c` reads user input from the window process and sends commands to control the drone's direction and force through a pipe connected to `droneDynamics.c`.

### Drone Process
The `droneDynamics.c` sets up shared memory and semaphores for communication with the server and window processes. It reads commands from the keyboard, updates its position, and writes the updated position to shared memory.

### Server Process
The `server.c` sends its PID to the watchdog and reads the drone's position from shared memory, using a semaphore to synchronize access to the shared memory.

### Watchdog Process
The `watchdog.c` reads PIDs of all other processes and sets up signal handlers. It sends `SIGUSR1` signals to all processes and terminates them if they fail to respond within a threshold.

## Conclusion
We have created a multi-process drone system with shared memory communication and a text-based UI. The system utilizes processes, shared memory, and semaphores for inter-process communication, serving as a foundation for a drone simulation system that can be expanded in future assignments.
