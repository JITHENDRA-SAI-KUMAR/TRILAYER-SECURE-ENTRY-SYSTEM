🔐 TRI-LAYER SECURE ENTRY SYSTEM

## **RFID + Password + OTP Based Secure Access System using LPC2148 ARM7**

![LPC2148](https://img.shields.io/badge/Controller-LPC2148-blue?style=for-the-badge)
![Embedded C](https://img.shields.io/badge/Language-Embedded%20C-green?style=for-the-badge)
![Keil](https://img.shields.io/badge/IDE-Keil%20µVision-orange?style=for-the-badge)
![Proteus](https://img.shields.io/badge/Simulation-Proteus-red?style=for-the-badge)

> **A three-layer embedded security system that verifies RFID, Password and OTP before operating a motor-controlled door.**

---

# 📌 **PROJECT OVERVIEW**

The **Tri-Layer Secure Entry System** is an ARM7-based embedded access-control system developed using the **LPC2148 microcontroller**.

### **Three Security Layers**

| Layer | Authentication | Interface |
|---|---|---|
| **1** | RFID Card | UART1 |
| **2** | Password | 4×4 Keypad |
| **3** | OTP | GSM / UART0 |

### **Security Sequence**

```text
RFID CARD
    ↓
PASSWORD
    ↓
OTP
    ↓
ACCESS GRANTED
    ↓
DOOR CONTROL
________________________________________
📁 PROJECT STRUCTURE
TRI-LAYER-SECURE-ENTRY-SYSTEM/
│
├── README.md
│
├── src/
│   ├── main.c
│   ├── dc_motor.c
│   ├── delay.c
│   ├── gsm.c
│   ├── i2c.c
│   ├── i2c_eeprom.c
│   ├── interrupt.c
│   ├── kpm.c
│   ├── lcd.c
│   ├── uart0_int.c
│   └── uart1.c
│
├── include/
│   ├── dc_motor.h
│   ├── defines.h
│   ├── delay.h
│   ├── eint.h
│   ├── i2c.h
│   ├── i2c_defines.h
│   ├── i2c_eeprom.h
│   ├── kpm.h
│   ├── kpm_defines.h
│   ├── lcd.h
│   ├── lcd_defines.h
│   ├── pin_function_defines.h
│   ├── types.h
│   ├── uart.h
│   ├── uart0.h
│   └── uart1.h
│
├── keil/
│   └── Keil project files
│
├── proteus/
│   └── Proteus simulation files
│
├── outputs/
│   ├── hardware/
│   └── proteus/
│
└── docs/
Source Organization
Folder	Contains
src/	C source files
include/	Header files
keil/	Keil project files
proteus/	Proteus simulation
outputs/	Hardware and simulation results
docs/	Supporting documentation
________________________________________
🏗️ SYSTEM ARCHITECTURE
                         ┌───────────────────┐
                         │     LPC2148       │
                         │      ARM7         │
                         └─────────┬─────────┘
                                   │
          ┌────────────────────────┼────────────────────────┐
          │                        │                        │
          ▼                        ▼                        ▼
       RFID                     GSM                    EEPROM
      UART1                    UART0                     I2C
          │                        │                        │
          │                        │                        │
          ▼                        ▼                        ▼
   RFID Authentication       OTP SMS              User Data Storage
          
          │
          ▼
      4×4 KEYPAD
          │
          ▼
   Password / OTP Input
          │
          ▼
       16×2 LCD
          │
          ▼
     Status Display

          │
          ▼
      L293D DRIVER
          │
          ▼
       DC MOTOR
          │
          ▼
         DOOR
________________________________________
⚙️ SYSTEM INITIALIZATION
Before authentication starts, the LPC2148 initializes the required peripherals.
Initialization Sequence
                 START
                   │
                   ▼
          ┌─────────────────┐
          │ GPIO Initialize │
          └────────┬────────┘
                   ↓
          ┌─────────────────┐
          │ LCD Initialize  │
          └────────┬────────┘
                   ↓
          ┌─────────────────┐
          │ Keypad Init     │
          └────────┬────────┘
                   ↓
          ┌─────────────────┐
          │ UART0 Init      │
          │     GSM         │
          └────────┬────────┘
                   ↓
          ┌─────────────────┐
          │ UART1 Init      │
          │     RFID        │
          └────────┬────────┘
                   ↓
          ┌─────────────────┐
          │ I2C Initialize  │
          └────────┬────────┘
                   ↓
          ┌─────────────────┐
          │ EEPROM Access   │
          └────────┬────────┘
                   ↓
          ┌─────────────────┐
          │ Motor Initialize│
          └────────┬────────┘
                   ↓
          ┌─────────────────┐
          │ Interrupt Init  │
          └────────┬────────┘
                   ↓
             SYSTEM READY
                   │
                   ▼
             RFID WAITING
Initialization Includes
•	GPIO configuration 
•	LCD initialization 
•	Keypad initialization 
•	UART0 initialization 
•	UART1 initialization 
•	I2C initialization 
•	EEPROM access 
•	Motor-control initialization 
•	External interrupt initialization 
________________________________________
🧰 HARDWARE COMPONENTS
Component	Purpose
LPC2148 ARM7	Main controller
RFID Reader	User identification
RFID Card	Authentication
GSM Module	OTP transmission
4×4 Keypad	Password and OTP input
16×2 LCD	Status display
I2C EEPROM	Data storage
L293D	Motor driver
DC Motor	Door mechanism
Switch	External interrupt/control
LED	Status indication
________________________________________
📡 COMMUNICATION INTERFACES
All communication interfaces used by the system are grouped here.
Communication Summary
Interface	Device	Direction	Purpose
UART0	GSM	TX / RX	OTP communication
UART1	RFID	RX	RFID data reception
I2C	EEPROM	SDA / SCL	Data storage
GPIO	LCD	Output	Display
GPIO	Keypad	Input / Output	User input
GPIO	L293D	Output	Motor control
EINT	Switch	Input	External interrupt
Communication Architecture
                         LPC2148
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
        ▼                   ▼                   ▼
      UART0               UART1                I2C
        │                   │                   │
        ▼                   ▼                   ▼
       GSM                 RFID               EEPROM
        │
        ▼
   OTP through SMS
________________________________________
📍 PIN CONFIGURATION
UART0 — GSM
LPC2148 Pin	Function	GSM
P0.0	TXD0	RX
P0.1	RXD0	TX
P0.0 TXD0 ─────────► GSM RX
P0.1 RXD0 ◄───────── GSM TX
________________________________________
UART1 — RFID
LPC2148 Pin	Function	RFID
P0.8	TXD1	RX
P0.9	RXD1	TX
P0.8 TXD1 ─────────► RFID RX
P0.9 RXD1 ◄───────── RFID TX
________________________________________
I2C — EEPROM
LPC2148 Pin	Signal	EEPROM
P0.2	SDA	SDA
P0.3	SCL	SCL
P0.2 ───────── SDA ───────── EEPROM
P0.3 ───────── SCL ───────── EEPROM
________________________________________
16×2 LCD
LCD Signal	LPC2148
D0–D7	P0.10–P0.17
RS	P0.19
RW	P0.20
EN	P0.21
P0.10 – P0.17  → LCD DATA
P0.19          → LCD RS
P0.20          → LCD RW
P0.21          → LCD EN
________________________________________
4×4 KEYPAD
The keypad is used for:
•	Password input 
•	OTP input 
•	Password-change operation 
The keypad-specific definitions are maintained in:
kpm_defines.h
________________________________________
L293D + DC MOTOR
LPC2148
   │
   │ Motor Control
   ▼
 L293D DRIVER
   │
   ▼
 DC MOTOR
   │
   ▼
  DOOR
The exact motor GPIO mapping is maintained in:
dc_motor.c
dc_motor.h
________________________________________
🔐 AUTHENTICATION SYSTEM
The authentication process consists of three sequential levels.
LEVEL 1 — RFID
RFID CARD
    ↓
UART1
    ↓
RFID VALIDATION
Result
INVALID RFID ─────► RFID WAITING

VALID RFID ───────► PASSWORD
________________________________________
LEVEL 2 — PASSWORD
4×4 KEYPAD
     ↓
PASSWORD INPUT
     ↓
PASSWORD VALIDATION
Result
INVALID PASSWORD ─────► RFID WAITING

VALID PASSWORD ────────► OTP GENERATION
________________________________________
LEVEL 3 — OTP
OTP GENERATION
      ↓
GSM MODULE
      ↓
SMS TO USER
      ↓
OTP INPUT
      ↓
OTP VALIDATION
Result
INVALID OTP ─────► RFID WAITING

VALID OTP ────────► ACCESS GRANTED
________________________________________
🔄 COMPLETE AUTHENTICATION FLOW
                    ┌──────────────┐
                    │    START     │
                    └──────┬───────┘
                           ↓
                    ┌──────────────┐
                    │  RFID CARD   │
                    └──────┬───────┘
                           ↓
                      VALID RFID?
                       /       \
                     NO         YES
                     │           │
                     │           ↓
                     │      ┌────────────┐
                     │      │  PASSWORD  │
                     │      └─────┬──────┘
                     │            ↓
                     │       VALID PASSWORD?
                     │        /          \
                     │      NO            YES
                     │      │              │
                     │      │              ↓
                     │      │       ┌────────────┐
                     │      │       │ OTP GENERATE│
                     │      │       └─────┬──────┘
                     │      │             ↓
                     │      │       ┌────────────┐
                     │      │       │ OTP VIA GSM│
                     │      │       └─────┬──────┘
                     │      │             ↓
                     │      │       ┌────────────┐
                     │      │       │  OTP INPUT │
                     │      │       └─────┬──────┘
                     │      │             ↓
                     │      │        VALID OTP?
                     │      │        /        \
                     │      │      NO          YES
                     │      │      │             │
                     │      │      │             ↓
                     │      │      │      ACCESS GRANTED
                     │      │      │             │
                     │      │      │             ↓
                     │      │      │       MOTOR FORWARD
                     │      │      │          5 SEC
                     │      │      │             │
                     │      │      │             ↓
                     │      │      │       DOOR OPENED
                     │      │      │             │
                     │      │      │             ↓
                     │      │      │       MOTOR REVERSE
                     │      │      │          3 SEC
                     │      │      │             │
                     │      │      │             ↓
                     │      │      │        DOOR LOCKED
                     │      │      │
                     └──────┴──────┴─────────────┐
                                                 ↓
                                         RFID CARD WAITING
Any failed authentication returns to the RFID-card stage and starts a fresh authentication cycle.
________________________________________
⭐ KEY FEATURES
1. RFID-Based Identification
•	RFID reader identifies the user. 
•	Communication through UART1. 
•	Valid card proceeds to password verification. 
•	Invalid card returns to RFID waiting. 
2. Password Verification
•	Password entered using 4×4 keypad. 
•	Stored authentication data is accessed through I2C EEPROM. 
•	Valid password proceeds to OTP. 
•	Invalid password returns to RFID waiting. 
3. GSM OTP Authentication
•	OTP generated after successful password verification. 
•	GSM sends OTP through SMS. 
•	User enters OTP through keypad. 
•	OTP is validated before access is granted. 
4. LCD Status Display
The LCD provides the current system status:
Waiting for RFID
       ↓
Valid Card
       ↓
Enter Password
       ↓
Valid Password
       ↓
OTP Generated
       ↓
Enter OTP
       ↓
Valid OTP
       ↓
Door Opening
       ↓
Door Opened
       ↓
Door Closing
       ↓
Door Locked
5. EEPROM Data Storage
•	RFID data storage 
•	Password storage 
•	I2C-based communication 
6. Motor-Controlled Door
•	L293D motor driver 
•	DC motor 
•	Forward rotation for door opening 
•	Reverse rotation for door closing 
7. External Interrupt
•	External switch-based interrupt functionality 
•	Interrupt handling included in the project firmware 
________________________________________
⚙️ DOOR CONTROL
After successful authentication:
ACCESS GRANTED
      ↓
MOTOR FORWARD
      ↓
   5 SECONDS
      ↓
MOTOR STOP
      ↓
DOOR OPENED
      ↓
MOTOR REVERSE
      ↓
   3 SECONDS
      ↓
MOTOR STOP
      ↓
DOOR LOCKED
Stage	Motor	Duration
Door Opening	Forward	5 sec
Door Open	Stop	—
Door Closing	Reverse	3 sec
Door Locked	Stop	—
________________________________________
🖥️ LCD USER INTERFACE
Authentication Messages
Stage	LCD Message
Waiting	Waiting for RFID CARD
RFID	Valid card
Password	Enter the pass
Password	Valid password
OTP	OTP Generating
OTP	Enter the OTP
OTP	Valid OTP
Door	DOOR OPENING
Door	DOOR OPENED
Door	DOOR CLOSING
Door	DOOR LOCKED
________________________________________
🧪 TESTING
Test	Input	Expected Result
1	Invalid RFID	Return to RFID
2	Valid RFID	Password requested
3	Invalid Password	Return to RFID
4	Valid Password	OTP generated
5	OTP generated	GSM sends SMS
6	Invalid OTP	Return to RFID
7	Valid OTP	Access granted
8	Successful authentication	Door motor operates
________________________________________
🛠️ SOFTWARE & TOOLS
Tool	Usage
Embedded C	Firmware development
Keil µVision	Compilation and debugging
Flash Magic	LPC2148 programming
Proteus	Circuit simulation
LPC2148 Board	Hardware implementation
________________________________________
🔨 BUILD & PROGRAM
Keil µVision
Open Keil Project
       ↓
Select LPC2148
       ↓
Verify Source / Header Files
       ↓
Build Project
       ↓
Generate HEX
Flash Magic
Connect LPC2148
       ↓
Open Flash Magic
       ↓
Select LPC2148
       ↓
Select HEX File
       ↓
Program
       ↓
Verify
       ↓
Run Hardware
________________________________________
📸 PROJECT RESULTS
Hardware Output
Place actual hardware photographs in:
outputs/
└── hardware/
    ├── complete_setup.jpg
    ├── rfid_authentication.jpg
    ├── gsm_otp.jpg
    └── door_motor_operation.jpg
Proteus Output
Place simulation screenshots in:
outputs/
└── proteus/
    ├── complete_circuit.png
    ├── rfid_authentication.png
    ├── password_authentication.png
    ├── otp_authentication.png
    └── door_motor_operation.png
________________________________________
📋 PROJECT QUICK REFERENCE
Category	Details
Controller	LPC2148 ARM7
Language	Embedded C
Authentication	RFID + Password + OTP
RFID Interface	UART1
GSM Interface	UART0
EEPROM Interface	I2C
Input	4×4 Keypad
Display	16×2 LCD
Motor Driver	L293D
Door Mechanism	DC Motor
IDE	Keil µVision
Programmer	Flash Magic
Simulation	Proteus
________________________________________
🚀 FUTURE SCOPE
•	Fingerprint authentication 
•	Face recognition 
•	Mobile application integration 
•	Multiple RFID-user management 
•	Access-history logging 
•	Cloud-based monitoring 
•	Remote door monitoring 
•	IoT-based security monitoring 
________________________________________
🏅 AUTHOR
Nookala Jithendra Sai Kumar

