# Simple Car Velocity Simulation

- [Problem Statement](#problem-statement)
  - [Requirements](#requirements)
  - [Part A](#part-a)
  - [Part B](#part-b)
- [Solutions](#solutions)

## Problem Statement

### Requirements

For this assignment you must implement a simple simulation of the velocity (speed) of a car. The simulation has two inputs. Input 1 (V) represents the Current velocity (0-7). Input 2 (A) represents the acceleration (or deceleration) and the resulting velocity change in a single time unit of the simulation.

An A value of 1, 2, or 3 means the car decelerates (reduces its velocity) by 1, 2, or 3 velocity units. The acceleration is successful if the final Velocity is in the range 0-7, where 0 is stopped, and 7 is maximum velocity.

**Examples:**

1. Velocity = 5, A = 2 : 5 – 2 = 3. 3 is between 0 and 7. This acceleration was *successful*.
2. Velocity = 2, A = 3 : 2 - 3 = -1. -1 is not between 0 and 7. This acceleration was *unsuccessful*.
3. Velocity = 6, A = 1 : 6 – 1 = 5. 5 is between 0 and 7. This acceleration was *successful*.

An A value of 4, 5, or 6 means the car accelerates (increases its velocity) by 1, 2, or 3 Velocity units. The acceleration is successful if the final Velocity is in the range 0-7 where 0 is stopped, and 7 is maximum velocity.

**Examples:**

1. Velocity = 5, A = 6, speed increase is 3 : 5 + 3 = 8. 8 is not between 0 and 7. This acceleration was *unsuccessful*.
2. Velocity = 2, A = 5, speed increase is 2 : 2 + 2 = 4. 4 is between 0 and 7. This acceleration was *successful*.
3. Velocity = 6, A = 4, speed increase is 1 : 6 + 1 = 7. 7 is between 0 and 7. This acceleration was *successful*.

An A value of 0 means there is no change to the Velocity. This acceleration will always be successful.

An A value of 7 means that the turbo button has been pressed and the Velocity moves to its maximum value (7). This acceleration will be successful if the current velocity is not 0.

> **Note:** The examples on this page represent only some selected cases. There are (many) more cases that result in successful or unsuccessful accelerations. Your circuit must correctly show a successful or unsuccessful acceleration for each possible case.

The Velocity (V) will be represented by three inputs (3 bits). The three V inputs are named as V1, V2, and V3. The table below shows the assignment of bits to each V value for V1, V2, and V3. Note the order of these bits, V1 is on the left.

The acceleration (A) that we want to apply will be represented by three inputs (3 bits). The three A inputs are named as A1, A2, and A3. The table below shows the assignment of bits to each A value for A1, A2, and A3. Note the order of these bits, A1 is on the left.

### Part A

The implementation for this part must use only the three basic logic gates (AND, OR, NOT) with maximum 2 inputs. No other logic gates or circuits are permitted.

You are required to implement a circuit where the user (you) can input a value for the Velocity (V) using value (V1, V2, and V3) and an acceleration to apply (A) using value (A1, A2, and A3) and the circuit decodes the V1, V2, V3 and A1, A2, A3 values using a decoder made up of only the permitted logic gates to determine if the acceleration is successful or unsuccessful based on the [requirements section](#requirements).

The output is via a single LED labelled Successful which is lit if V = 0, 1, 2, 3, 4, 5, 6, or 7 after a successful acceleration is applied (A = 0, 1, 2, 3, 4, 5, 6, or 7). The LED is not lit for any values of V or A or where the applied acceleration is unsuccessful.

### Part B

For this part, the simulation will count how many successful and unsuccessful accelerations have been made, If the number of unsuccessful accelerations becomes greater than the number of successful accelerations the simulation must stop and no changes to the circuit will be permitted after this happens.

| V Value | V1 | V2 | V3 | A Value | A1 | A2 | A3 |
|:-------:|:--:|:--:|:--:|:-------:|:--:|:--:|:--:|
|    0    |  0 |  0 |  0 |    0    |  0 |  0 |  0 |
|    1    |  0 |  0 |  1 |    1    |  0 |  0 |  1 |
|    2    |  0 |  1 |  0 |    2    |  0 |  1 |  0 |
|    3    |  0 |  1 |  1 |    3    |  0 |  1 |  1 |
|    4    |  1 |  0 |  0 |    4    |  1 |  0 |  0 |
|    5    |  1 |  0 |  1 |    5    |  1 |  0 |  1 |
|    6    |  1 |  1 |  0 |    6    |  1 |  1 |  0 |
|    7    |  1 |  1 |  1 |    7    |  1 |  1 |  1 |

Using the same circuit as Part A, add additional circuitry to count how many successful and unsuccessful accelerations have been tried. Each time an unsuccessful acceleration is tried, add 1 to the number of unsuccessful accelerations. Each time a successful acceleration is tried, add 1 to the number of successful accelerations.

**Simulation stopping condition (simulation output can not change):**

If the number of unsuccessful accelerations is greater than the number of successful accelerations then an LED labelled `Circuit locked` is lit, and the circuit is permanently locked. No matter the changes to the inputs after this happens, the `Simulation locked` LED will remain lit and cannot be turned off.

> **Note:** If the successful count reaches the maximum value of 7 (for a 3 bit counter) then the simulation will also stop but the `Simulation Locked` LED will remain off. In this case you can assume that the user will no longer try to interact with the simulation.

**For Part B only**, you may **use only the three basic logic gates** (AND, OR, NOT) with **maximum 2 inputs**, as well as the more advanced counter (3 bit), comparator (3 bit unsigned), and DFLIP-FLOP circuits (only those three) from the Logisim circuit library. You may also use constants. The prebuilt DFLIP-FLOP circuit can be used to ‘remember’ some information.

> **Note:** For Part B you will need to add a button that is pressed by you after the Velocity (V) and acceleration (A) have been entered. This is to avoid counting while you are adjusting the input pins for the V and A input pins (V1, V2, V3, A1, A2, and A3).

## Solutions

- [Easy Solution](./Solution/Easy)
- [Complex Solution](./Solution/Complex) *(Custom circuits for [Adder](./Solution/Complex/README.md#adder), [Subtractor](./Solution/Complex/README.md#subtractor), [Comparator](./Solution/Complex/README.md#comparator), [Check Between](./Solution/Complex/README.md#between), [A and V Generator](./Solution/Complex/README.md#a-and-v-generator), [Acceleration](./Solution/Complex/README.md#accerleration), [Deceleration](./Solution/Complex/README.md#deceleration))*
