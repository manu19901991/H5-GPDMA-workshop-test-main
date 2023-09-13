----!
Presentation
----!

<awarning>
**WE ARE GOING TO USE A DIFFERENT .IOC FILE FROM THE ONE YOU DOWNLOADED IN PREREQUISITES. PLEASE GET IT FROM THIS **[link](https://github.com/manu19901991/STM32_WBA_EX2_Template)** this contains the template project that we will use in our handson****
</awarning>

---

# Add application code to move to discoverable

1.code needs to be added in **STM32_WPAN/App/app_ble.c** inside the function App_BLE_Init ~line 293 in **/*USER CODE BEGIN APP_BLE_Init_2*/**

```c
APP_BLE_Procedure_Gap_Peripheral(PROC_GAP_PERIPH_ADVERTISE_START_FAST);
```
2.stiil in **STM32_WPAN/App/app_ble.c** inside SVCCTL_App_Notification function
~line 343 in **/*USER CODE BEGIN EVT_DISCONN_COMPLETE*/**

```c
APP_BLE_Procedure_Gap_Peripheral(PROC_GAP_PERIPH_ADVERTISE_START_FAST);
```




