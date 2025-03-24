### **Introduction**

In Singapore's aging society, wearable health monitoring systems are increasingly used for elderly care to track key health metrics, detect emergencies, and provide real-time feedback. This assignment focuses on developing an STM32 microcontroller-based monitoring system capable of tracking body temperature, detecting falls, identifying abnormal movements, and monitoring body posture. The system will operate in two modes: **Standard Mode** for general health tracking and **Emergency Mode** for immediate intervention when thresholds of certain sensors are exceeded. Students will need to do a group presentation with slides along with a demonstration of the wearable device designed by them on the assessment day.

### **Devices Used**

- **ACCELEROMETER:** 3D accelerometer LSM6DSL
- **GYROSCOPE:** 3D gyroscope LSM6DSL
- **HUMIDITY_SENSOR:** Capacitive digital sensor HTS221
- **TEMPERATURE_SENSOR:** Capacitive digital sensor HTS221
- **MAGNETOMETER:** High-performance 3-axis magnetometer LIS3MDL
- **PRESSURE_SENSOR:** Absolute digital output barometer LPS22HB
- **LED:** Green LED2
- **MODE_TOGGLE:** USER Button (blue), read using interrupts
- **UART:** Data transmission from STM32 to a ****Terminal program (Tera Term/Mac Terminal) on a personal computer/laptop displaying telemetry

### **Modes of Operation**

### **Standard Mode**

- Activated by default when the system is powered on.
- The device must be on a horizontal surface when powered on for the first time. Upon powering on, the device must have the following Standard Mode behaviors:
    - LED_2 is off
    - The following message is sent once to PC/laptop **each time** when Standard Mode is entered:
        
        ***"Entering Standard Mode.\r\n"***
        
    - The TEMPERATURE_SENSOR, ACCELEROMETER, GYROSCOPE, MAGNETOMETER, HUMIDITY_SENSOR, and BAROMETER are **sampled once every second**. The format of the transmitted data to **PC through UART ****should be as follows:
        - ***XXX_TEMP_tt.tt_ACC_x.xx_y.yy_z.zz \r\n***
            
            ***XXX GYRO nnn.n MAGNETO X.XX Y.YY Z.ZZ \r\n***
            
            ***XXX HUMIDITY h.hh and BARO b.bb \r\n***
            
        - XXX represents a 3-digit value that starts from 000 and increments by 001 after each transmission to PC.
        - XXX never resets itself to 000, unless the device itself is powered on from a power-off state. It is assumed that 999 will never be reached.
        - tt.tt stands for the temperature reading from the sensor up to 2 decimal places.
        - x.xx is the x-axis reading from the accelerometer, in 'g's, up to 2 decimal places. Similarly, y.yy and z.zz are the accelerometer’s readings in ‘g’s on y and z axes. Note : 1 g = 9.8 m/s2.
        - nnn.n stands for the Gyroscope reading from the sensor up to 2 decimal places.
        - X.XX, Y.YY and Z.ZZ are readings from Magnetometer sensor on x, y and z axes to 2 decimal places.
        - h.hh stands for Humidity sensor reading and b.bb stands for Barometer reading to 2 decimal places.
- If **MODE_TOGGLE** *twice* within a second within a second during the Standard Mode, the device goes into Emergency Mode. **The printing of the data through UART to PC stops.**

### **Emergency Mode**

- Activated by pressing **MODE_TOGGLE** twice within one second.
- Emergency mode can be turned on by the elderlies when they require detailed care. As soon as the device enters Emergency Mode, the following behavior occurs in Emergency Mode:
    - LED_2 blinking at 1 Hz frequency (on and off 1 time in 1 second).
    - The following message is sent once to PC  when Emergency Mode is entered:***"Entering Standard Mode.\r\n"***
        
        **each time**
        
    - Features of Emergency Mode are enabled (see next section)
- Emergency Mode can be deactivated and switch back to Standard Mode by pressing **MODE_TOGGLE** once.

### **Features of Emergency Mode**

### **1. Body Temperature Monitoring**

- Utilizes the **TEMPERATURE_SENSOR** to track and print the body temperature.
- If the reading exceeds **TEMP_THRESHOLD**, the system will:
    - Display "***Fever is detected!\n***" through **UART** telemetry.
    - Activate LED blinking at 3 Hz.
- In Emergency mode, TEMPERATURE_SENSOR's readings and warning (if any) need to be sent to PC every 5 seconds. **Warning can be disabled once TEMPERATURE_SENSOR's readings are below TEMP_THRESHOLD.**

### **2. Fall Detection**

- Uses the **ACCELEROMETER** to print the accelerometer reading and detect a fall.
- If the acceleration drops below **ACC_THRESHOLD**:
    - Display "***Falling is detected!\n***" through **UART** telemetry.
    - Activate LED blinking at 2 Hz.
- In Emergency mode, ACCELEROMETER's readings and warning (if any) need to be sent to PC every 5 seconds. **Warning can be disabled once ACCELEROMETER's readings are below ACC_THRESHOLD.**
- For basic requirements, fall detection can be detected using polling only. If the fall detection is detected using an interrupt instead, this contributes to an enhancement feature. Marks for enhancement will be given.

### **3. Sudden Movement Detection**

- Uses the **GYROSCOPE** to print gyroscope readings and detect sudden twisting or twitching movements.
- If readings exceed **GYRO_THRESHOLD**:
    - Display "***Abnormal movement detected!\n***" through **UART** telemetry.
    - Activate LED blinking at 2 Hz.
- In Emergency mode, GYROSCOPE's readings and warning (if any) need to be sent to PC every 5 seconds. **Warning can be disabled once GYROSCOPE's readings are below GYRO_THRESHOLD.**

### **4. Posture Monitoring**

- Uses the **MAGNETOMETER** to print magnetometer readings and detect improper body posture.
- If readings exceed **MAG_THRESHOLD**:
    - Display "Incorrect posture detected!\n" through **UART** telemetry.
    - Activate LED blinking at 2 Hz.
- MAGNETOMETER's readings and warning (if any) need to be sent to PC every 5 seconds. **Warning can be disabled once MAGNETOMETER's readings are below MAG_THRESHOLD.**

## enhancement (done after basic functions are achieved)

1. UART receive (echoing, extra emergency → extra modes!)
2. interrupt for freefall
3. external sensors 
4. wifi 
5. board to board communication
6. maybe buzzer
