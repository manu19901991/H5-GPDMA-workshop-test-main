----!
Presentation
----!

# Add application code to toggle LED from client

code needs to be added in **STM32_WPAN/App/p2p_server_app.c** inside the function P2P_SERVER_Notification() ~line 110 in **/*USER CODE BEGIN Service1Char1_WRITE_NO_RESP_EVT*/**

```c
HAL_GPIO_TogglePin(GPIOB, LD2_Pin|LD3_Pin|LD1_Pin);
```

# Add application code to rise an alarm from device to Smartphone

1. code needs to be added in **Core/Inc/app_conf.h** ~line 110  in **/*USER CODE BEGIN CFG_Task_Id_t*/**

```c
TASK_BUTTON_1,
```

2. in **STM32_WPAN/App/p2p_server_app.c** inside the function P2P_SERVER_APP_Init() ~line 180 add the below code in 
**/*USER CODE BEGIN Service1_APP_Init*/**

```c
UTIL_SEQ_RegTask( 1U << TASK_BUTTON_1, UTIL_SEQ_RFU, P2P_SERVER_Switch_c_SendNotification);
```

3. add the below code in **Core/Src/app_entry.c** ~line 416 inside **/*USER CODE BEGIN FD_WRAP_FUNCTIONS*/** 

```c
void HAL_GPIO_EXTI_Rising_Callback(uint16_t GPIO_Pin)
{
  if (GPIO_Pin == B1_Pin)
  {
	  flag=1;
    UTIL_SEQ_SetTask(1U << TASK_BUTTON_1, CFG_SEQ_PRIO_0);
  }

  return;
}
```
4.  in **STM32_WPAN/App/p2p_server_app.c** in the functionP2P_SERVER_Switch_c_SendNotification() ~line 205 inside **/*USER CODE BEGIN Service1Char2_NS_1*/** add:

```c
a_P2P_SERVER_UpdateCharData[0] = 0x01; /* Device Led selection */
a_P2P_SERVER_UpdateCharData[1] = 0x00;
/* Update notification data length */
p2p_server_notification_data.Length = (p2p_server_notification_data.Length) + 2;
notification_on_off = Switch_c_NOTIFICATION_ON;
```