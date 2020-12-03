/**********************************************************************
* - Description:		esp32-mpu6050
* - File:				main.c
* - Compiler:			xtensa-esp32
* - Debugger:			USB2USART
* - Author:				Mohamed El-Sabagh
* - Target:				ESP32
* - Created:			2017-12-11
* - Last changed:		2017-12-11
*
**********************************************************************/


#include "esp_log.h"
#include "freertos/FreeRTOS.h"
#include "freertos/FreeRTOSConfig.h"
#include "freertos/task.h"
#include "i2c/i2c_application.h"
#include "i2c/i2c_driver.h"
#include "nvs_flash.h"
#include "mpu6050/mpu6050_application.h"

#define STACK_SIZE_2048 2048


/*---------------------------------------------------------------------------/
/  APPConfig - Application 	Ver 1.00
/---------------------------------------------------------------------------*/

#ifndef _APP_CONFIG_H
#define _APP_CONFIG_H

#define _APPConfig 100	/* Revision ID */

/*---------------------------------------------------------------------------/
/ Function Configurations
/---------------------------------------------------------------------------*/


#define DEBUG	  1
/* This option Enables the debug interface. */

#define	MICRO_BUFFER_SIZE			32
#define	MINI_BUFFER_SIZE			64
#define	TINY_BUFFER_SIZE			128
#define	SMALL_BUFFER_SIZE			256
#define	MEDIUM_BUFFER_SIZE			512
#define	BIG_BUFFER_SIZE				1024
#define	LARGE_BUFFER_SIZE			2048
#define	MASSIVE_BUFFER_SIZE			4096
#define	ENORMUS_BUFFER_SIZE			8192
/* Those options specify the size of the buffers used between different
/  interfaces. */

#define  osPriorityIdle             configMAX_PRIORITIES - 6
#define  osPriorityLow              configMAX_PRIORITIES - 5
#define  osPriorityBelowNormal      configMAX_PRIORITIES - 4
#define  osPriorityNormal       	configMAX_PRIORITIES - 3
#define  osPriorityAboveNormal      configMAX_PRIORITIES - 2
#define  osPriorityHigh             configMAX_PRIORITIES - 1
#define  osPriorityRealtime         configMAX_PRIORITIES - 0

#endif



/* Structure that will hold the TCB of the task being created. */
StaticTask_t xI2CWriteTaskBuffer;
StaticTask_t xI2CReadTaskBuffer;
StaticTask_t xMPU6050TaskBuffer;
/* Buffer that the task being created will use as its stack.  Note this is
    an array of StackType_t variables.  The size of StackType_t is dependent on
    the RTOS port. */
StackType_t xStack_I2C_Write[ STACK_SIZE_2048 ];
StackType_t xStack_I2C_Read[ STACK_SIZE_2048 ];
StackType_t xStack_MPU6050Task[ STACK_SIZE_2048 ];

void app_main(void)
{
    esp_err_t ret;
    // Initialize NVS.
    ret = nvs_flash_init();
    if (ret == ESP_ERR_NVS_NO_FREE_PAGES) {
        ESP_ERROR_CHECK(nvs_flash_erase());
        ret = nvs_flash_init();
    }
    ESP_ERROR_CHECK(ret);

    // Initializations
	vI2CInit();



    xTaskCreateStaticPinnedToCore(vI2CWrite,            /* Function that implements the task. */
                                  "vI2CWrite",          /* Text name for the task. */
                                  STACK_SIZE_2048,      /* Number of indexes in the xStack array. */
                                  NULL,                 /* Parameter passed into the task. */
                                  osPriorityHigh,       /* Priority at which the task is created. */
                                  xStack_I2C_Write,     /* Array to use as the task's stack. */
                                  &xI2CWriteTaskBuffer, /* Variable to hold the task's data structure. */
                                  0                     /*  0 for PRO_CPU, 1 for APP_CPU, or tskNO_AFFINITY which allows the task to run on both */
                                  );

    xTaskCreateStaticPinnedToCore(vI2CRead,            /* Function that implements the task. */
                                  "vI2CRead",          /* Text name for the task. */
                                  STACK_SIZE_2048,     /* Number of indexes in the xStack array. */
                                  NULL,                /* Parameter passed into the task. */
                                  osPriorityHigh,      /* Priority at which the task is created. */
                                  xStack_I2C_Read,     /* Array to use as the task's stack. */
                                  &xI2CReadTaskBuffer, /* Variable to hold the task's data structure. */
                                  0                    /*  0 for PRO_CPU, 1 for APP_CPU, or tskNO_AFFINITY which allows the task to run on both */
                                  );

    xTaskCreateStaticPinnedToCore(vMPU6050Task,        /* Function that implements the task. */
                                  "vMPU6050Task",      /* Text name for the task. */
                                  STACK_SIZE_2048,     /* Number of indexes in the xStack array. */
                                  NULL,                /* Parameter passed into the task. */
                                  osPriorityNormal,    /* Priority at which the task is created. */
                                  xStack_MPU6050Task,  /* Array to use as the task's stack. */
                                  &xMPU6050TaskBuffer, /* Variable to hold the task's data structure. */
                                  0                    /*  0 for PRO_CPU, 1 for APP_CPU, or tskNO_AFFINITY which allows the task to run on both */
                                  );
}