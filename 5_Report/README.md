# INTRODUCTION:
The Atmel® ATmega328P is a low-power CMOS 8-bit microcontroller based on the AVR® enhanced
RISC architecture. By executing powerful instructions in a single clock cycle, the ATmega328P
achieves throughputs approaching 1MIPS per MHz allowing the system designer to optimize power
consumption versus processing speed. In order to maximize performance and parallelism, the AVR uses
a Harvard architecture – with separate memories and buses for program and data. Instructions in the
program memory are executed with a single level pipelining. While one instruction is being executed,
the next instruction is pre-fetched from the program memory. This concept enables instructions to be
executed in every clock cycle. The program memory is in-system reprogrammable flash memory.
# OBJECTIVE:
To control Servo Motor Using Timer1 PWM technique and UART interface.
# COMPONENTS:
• ATMega328P

• UART Module

• Servo Motor.
# DESCRIPTION:
Microchip's ATmega328 is an 8-bit, 28-pin AVR Microcontroller that uses RISC architecture and includes a 32KB flash-type programme memory.

The Atmega328 microcontroller is found in the Arduino UNO, Arduino Pro Mini, and Arduino Nano boards.

It can function at voltages ranging from 3.3 to 5.5 volts, but we usually choose 5 volts as a standard.

Cost-effectiveness, low power consumption, programming lock for security, and true timer counter with independent oscillator are only a few of its outstanding qualities.

It's most commonly found in Embedded Systems. Take a look at these Real-Life Embedded System Examples; we can design all of them with this Microcontroller.
# DESCRIPTION:
Timer-1 is configured as PWM to control the servo motor. Timer-1 is configured using TOV1,
OCF1A, OCF1B, and ICF1. UART is interfaced with ATmega 232P to control servo motor using
PWM.
# TIMER1:
The 16-bit Timer/Counter unit allows accurate program execution timing (event management), wave generation, and signal
timing measurement.
# Features:
• True 16-bit design (i.e., allows 16-bit PWM)

• Two independent output compare units

• Double buffered output compare registers
# TIMER0:
Timer/Counter0 is a general purpose 8-bit Timer/Counter module, with two independent output compare units, and with
PWM support. It allows accurate program execution timing (event management) and wave generation.
# UART:
• The universal synchronous and asynchronous serial receiver and transmitter (USART) is a highly
flexible serial communication device.

• The USART0 can also be used in master SPI mode

• The three main parts of the USART (listed from the top): Clock generator, transmitter and receiver.

Control registers are shared by all units. The clock generation logic consists of synchronization logic
for external clock input used by synchronous slave operation, and the baud rate generator. The XCKn
(transfer clock) pin is only used by synchronous transfer mode. The transmitter consists of a single
write buffer, a serial shift register, parity generator and control logic for handling different serial frame
formats. The write buffer allows a continuous transfer of data without any delay between frames. The
receiver is the most complex part of the USART module due to its clock and data recovery units. The
recovery units are used for asynchronous data reception. In addition to the recovery units, the receiver
includes a parity checker, control logic, a shift register and a two level receive buffer (UDRn). The
receiver supports the same frame formats as the transmitter, and can detect frame error, data overrun
and parity errors.
# SIMULATION DIAGRAM:
![image](https://user-images.githubusercontent.com/102902139/164994359-b76282b7-c2bf-4b4b-9966-25894d3d363b.png)


