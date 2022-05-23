# Embedded-Systems
Repository with evidence regarding the embedded systems course taken at my university


A class-wide PCB with an STM32F4 microcontroller and an MPU-6050 sensor was designed. The class was divided into teams of 3-4 people, and, with the help of the professor, we were instructed to design a circuit CAD for the said system using EasyEDA. Below is the chosen design for the ordering of the board.

![chosenCAD](https://github.com/CarlosKiamy/Embedded-Systems/blob/main/img/chosenCAD.png)

![pcb](https://github.com/CarlosKiamy/Embedded-Systems/blob/main/img/pcb.png)

![physical_PCB](https://github.com/CarlosKiamy/Embedded-Systems/blob/main/img/physical_PCB.png)

This particular design was chosen since, at the time of ordering, the BOM was complete in this circuit. The task given at my group in order to test the functionality of the PCB is as follows:

```
Tilt 0 degrees --> Output: All 4 LEDs light up permanently
Tilt 10 degrees --> Output:0, LED1 and LED2 are permanently on, all other LEDs off
Tilt 20 degrees --> Output: LED0 and LED1 are permanently lit, all other LEDs off
Tilt 30 degrees --> Output: LED0 lights up permanently, all other LEDs offTilt 0 degrees --> Output: All 4 LEDs light up permanently
Tilt 10 degrees --> Output:0, LED1 and LED2 are permanently on, all other LEDs off
Tilt 20 degrees --> Output: LED0 and LED1 are permanently lit, all other LEDs off
Tilt 30 degrees --> Output: LED0 lights up permanently, all other LEDs off
```

Microcontroller pinout:

![pinout](https://github.com/CarlosKiamy/Embedded-Systems/blob/main/img/pinout.png)

Physical setup:

![pinout](https://github.com/CarlosKiamy/Embedded-Systems/blob/main/img/proto.png)

For data verification, the SWO pin was used in order to use the printf function. In order to use printf, the ITM console must be setup first.
```
int _write(int file, char*ptr, int len){
int i=0;
for(i=0; i<len; i++)
	ITM_SendChar((*ptr++));
return len;
}
```

Afterwards, a library specific for the MPU-6050 sensor was used in order to do the quickcheck of the system.
Library used: https://github.com/leech001/MPU6050

The following code under the main function initializes the MPU sensor with I2C communication, and afterwards reads the values given under the infinite loop while(1).
```
  while (MPU6050_Init(&hi2c1) == 1);
 
  while (1)
  {
	  MPU6050_Read_All(&hi2c1, &MPU6050);
	  HAL_Delay (100);
  }
```

Under the MPU6050_Read_All() function, the following conditions were placed in here for the X angle in order to carry on with the given task.
``` 
float ejeX = Kalman_getAngle(&KalmanX, roll, DataStruct->Gx, dt);
printf("%f\n\r", ejeX);
    if (ejeX < 10){
    	 HAL_GPIO_WritePin(GPIOA, LED0_Pin, GPIO_PIN_SET);
    	 HAL_GPIO_WritePin(GPIOA, LED1_Pin, GPIO_PIN_SET);
    	 HAL_GPIO_WritePin(GPIOA, LED2_Pin, GPIO_PIN_SET);
    	 HAL_GPIO_WritePin(GPIOA, LED3_Pin, GPIO_PIN_SET);

    }
    if (ejeX > 10 && ejeX < 20){
    	 HAL_GPIO_WritePin(GPIOA, LED0_Pin, GPIO_PIN_SET);
    	 HAL_GPIO_WritePin(GPIOA, LED1_Pin, GPIO_PIN_SET);
    	 HAL_GPIO_WritePin(GPIOA, LED2_Pin, GPIO_PIN_SET);
    	 HAL_GPIO_WritePin(GPIOA, LED3_Pin, GPIO_PIN_RESET);

    }
    if (ejeX > 20 && ejeX < 30){
    	 HAL_GPIO_WritePin(GPIOA, LED0_Pin, GPIO_PIN_SET);
    	 HAL_GPIO_WritePin(GPIOA, LED1_Pin, GPIO_PIN_SET);
    	 HAL_GPIO_WritePin(GPIOA, LED2_Pin, GPIO_PIN_RESET);
    	 HAL_GPIO_WritePin(GPIOA, LED3_Pin, GPIO_PIN_RESET);

    }
    if (ejeX > 30){
    	 HAL_GPIO_WritePin(GPIOA, LED0_Pin, GPIO_PIN_SET);
    	 HAL_GPIO_WritePin(GPIOA, LED1_Pin, GPIO_PIN_RESET);
    	 HAL_GPIO_WritePin(GPIOA, LED2_Pin, GPIO_PIN_RESET);
    	 HAL_GPIO_WritePin(GPIOA, LED3_Pin, GPIO_PIN_RESET);

    }
 ```
