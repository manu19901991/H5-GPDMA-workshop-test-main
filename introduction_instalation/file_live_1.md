----!
Presentation
----!

# Introduction
## Dear Participant of STM32WBA Workshop,
<br>

Welcome to this short step-by-step guide which could help you to prepare to the live version of STM32WBA Workshop session.

You will find here:

 - all information about prerequisites (software and hardware), 
 - short information about installation process, 
 - links to materials useful for this session

Additionally, in appendixes you can find some basic information about the board we will use during the session and useful information about configuration and usage of STM32CubeIDE built in terminal
<br>

To navigate within this manual, please use navigation buttons:
<br>

  ![navigation](./img/navigation.gif)

<br>

In case of any questions / problems please send us a mail at  **[manuel.marcias@st.com](manuel.marcias@st.com)**

See you on STM32H5 Workshop live session
<br>

## Yours,
## STMicroelectronics 
<br>

# Prerequisites
- Hardware:
  - **PC with MS Windows 10 operating system and admin rights granted**
  - **1 USB A to Micro-B Cable** cable 
  <br>
  ![microUSB cables](./img/uUSB.jpg)
  <br>
  - **[NUCLEO-WBA52CG](https://www.st.com/en/evaluation-tools/nucleo-wba52cg.html)** Nucleo-64 development board 
  <br>
  ![H5_DK](./img/5.png)
  <br>
- Software (PC with **MS Windows 10** operating system):
  - **[STM32CubeMX](https://www.st.com/en/development-tools/stm32cubemx.html)** in version 6.9.1
  - **[STM32CubeIDE](https://www.st.com/en/development-tools/stm32cubeide.html)** in version 1.13.1
  - **[STM32WBA Cube library](https://www.st.com/en/embedded-software/stm32cubewba.html)** in version 1.1.0
  - **[Virtual COM port drivers](https://www.st.com/en/development-tools/stsw-stm32102.html)**
  -  any **serial terminal** application (e.g. **[Termite](https://termite.software.informer.com/3.4/)**)
  - ST BLE ToolBox Smartphone Application - this has to be downloaded on your mobile
    - **[Android version](https://play.google.com/store/apps/details?id=com.st.dit.stbletoolbox&hl=it&gl=US&pli=1)**
    - **[IOS version](https://apps.apple.com/it/app/st-ble-toolbox/id1531295550)**

<br>

# Installation process
- download **STM32CubeMX** from [here](https://www.st.com/en/development-tools/stm32cubemx.html)
- install **STM32CubeMX** (if not yet done)
- download **STM32CubeIDE** from [here](https://www.st.com/en/development-tools/stm32cubeide.html)
- Install **STM32CubeIDE** (if not yet done)
- download and install **STM32WBA Cube library** (if not done yet):
  - run **STM32CubeMX**
  - go to `Help -> Manage Embedded Software Packages`
  - within package manager window find `STM32WBA`, unroll it and select newest available version ( in your case STM32WBA 1.1.0)
  - press `install now`
<br>
![H5_Lib_Install](./img/6.png)

<ainfo>
STM32CubeMX and STM32CubeIDE are using the same repository by default, so the installed STM32WBA Cube library will be visible in both tools.
</ainfo>


<br>
----


# Verification process before the workshop
The purpose of this part is checking whether all software components are installed properly.
<br>
Additionally prepared test project can be a base for next hands-on parts during the workshop.

## **STM32CubeIDE and STM32WBA Cube library**
<br>

----

<br>
**Task definition**
<br>

- Using STM32CubeIDE
  - Enable SWD for debug
  - Disable TrustZone
  - Configure ICACHE (in any of available modes)
- Select and configure USART3
  - in asynchronous mode,
  - using default settings (115200bps, 8D, 1stop bit, no parity) 
  - on PA9/PA10 pins
<br>

----

<br>
## **Step1** - project creation and peripherals configuration
 - Run **STM32CubeIDE**
 - Specify workspace location (i.e. `C:\_Work\WBA_ex1`)

<br>
- Start new project using one of the below methods:
  - by selecting `File->New->STM32Project` 
  - by click on `Start new STM32 project` button
  <br>
  ![Workspace_start2](./img/New_prj_start_2.gif)
<br>
- switch to **Board Selector** tab
- select **NUCLEO-WBA52CG** board
- press `Next` button
- within STM32 Project window:
  - specify project name (i.e. `WBA_UART`)
  - keep **enable TrustZone** option unchecked
  - press `Finish` button
  - on question pop-up window "Initialize all peripherals with their default state?" press `No` button 
  - on question pop-up window "Switch to proper CubeIDE perspective?", if it is showed, press `Yes` button 
  - on worning pop-up window "Do you still want a code generation?", press `No` button 
  - on following information pop-up window, it was our decision did not generate code, press `OK` button 
  <br>
   ![Workspace_start3](./img/CubeIDE_Start.apng)
<br>
- Peripherals configuration: Pinout&Configuration tab
- **ICACHE configuration** (System Core group)
  - select either 1-way or 2-ways (we will not focus on performance within this workshop)
  <br>
  ![ICACHE configuration](./img/8.gif)
  <br>
- **USART3 configuration** (Connectivity group)
  - select Asynchronous mode
  - keep default settings in configuration:
    - Basic parameters: 115200bps, 8bits data, 1 stop bit, no parity
    - Pins assignment: PA8, PB12
    - no interrupts, no DMA usage
  <br>
    ![USART3 configuration](./img/9.gif)
<br>
- **Project settings**
  - select `Project Manager` tab
  - check project location (.ioc file)
  - check project name
<br>
   ![Project settings](./img/CubeIDE_Proj.apng)
<br>
  - generate project by one of the ways:
    - by pressing "gear" icon
    - by select `Project->Generate Code`
    - by pressing **Alt+K**
<br>
  ![Project generation](./img/CubeIDE_GenCode.apng)
<br>

----

<br>
## **Step2** - coding part (`main.c` file)
<br>
Define the buffer of bytes to be sent over **USART3** (`USER CODE PV` section):
<br>

```c
  uint8_t buffer[]={"Hello STM32H5\r\n"};
```

<br>
![Coding1](./img/CubeIDE_Coding1.apng)
<br>
Turn on **LED1_GREEN** (`USER CODE 2` section):
<br>

```c
  HAL_GPIO_WritePin(LED1_GREEN_GPIO_Port, LED1_GREEN_Pin, 1);
```

<br>
![Coding2](./img/CubeIDE_Coding2.apng)
<br>
Toggle **LED1_GREEN** (`USER CODE 3` section)
<br>
Toggle **LED2_YELLOW** 
<br>
Start transmit of the data over **USART3** using prepared buffer and ***polling*** method
<br>
Wait for 250 ms
<br>

```c
  HAL_GPIO_TogglePin(LED1_GREEN_GPIO_Port, LED1_GREEN_Pin);
  HAL_GPIO_TogglePin(LED2_YELLOW_GPIO_Port, LED2_YELLOW_Pin);
  HAL_UART_Transmit(&huart3, buffer, 15, 200);
  HAL_Delay(250);
```

<br>
![Coding3](./img/CubeIDE_Coding3.apng)
<br>

----

<br>
## **Step 3** - build the project
- Build the project using `hammer` button or `Project->Built All` or **Ctrl+B**
<br>
![Project build](./img/CubeIDE_Build.apng)
<br>

<ainfo>
In case of neither errors nor warnings after this process, STM32CubeIDE and STM32WBA library are installed correctly. Last point - debug session will be verified during first hands on part on the workshop.
</ainfo>


<ainfo>
## **Congratulations** You have completed installation part. Now you are fully prepared for the live workshop session. 
</ainfo>

----

# Materials for the session
- Access to tools dedicated web pages:
  - [STM32CubeIDE](https://www.st.com/en/development-tools/stm32cubeide.html)
  - [STM32CubeMX](https://www.st.com/en/development-tools/stm32cubemx.html)
  - [STM32WBA Cube library](https://www.st.com/en/embedded-software/stm32cubewba.html)
- [STM32 on-line training resources](https://www.st.com/content/st_com/en/support/learning/stm32-education/stm32-moocs.html)
- documentation
  - [STM32WBA52xx datasheet](https://www.st.com/resource/en/datasheet/stm32wba52ce.pdf)
  - [STM32WBA52xx reference manual](https://www.st.com/en/microcontrollers-microprocessors/stm32wb-series/documentation.html)
- Complete slide deck for the session [this link](https://github.com/RRISTM/stm32u5_workshop/tree/master/material_pdf)


