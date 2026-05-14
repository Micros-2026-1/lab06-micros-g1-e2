[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/MCJunYEq)
[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=23734386&assignment_repo_type=AssignmentRepo)
# Lab06: Comunicación UART con PIC18F45K22

## Integrantes
*[Andrés Felipe Muñoz Martinéz](https://github.com/Andresfmm2007)

 *[Paula Andrea Cortés Espinosa](https://github.com/Cortes271)

 *[Reny Alexander Roncancio](https://github.com/renyroncancio-glitch)

 *[Laura Katerin Sanchez](https://github.com/laurakasanchezgu-oss)

## Documentación
En esta práctica se implementó el control de una pantalla LCD 16x2 mediante comunicación I2C con un microcontrolador, lo que permitió reducir el uso de pines y simplificar el cableado gracias al uso de un adaptador I2C. Se configuraron los registros necesarios y se desarrollaron funciones para enviar datos y mostrar información en la pantalla, como mensajes y porcentajes, Además, se trabajó con la comunicación serial UART entre un microcontrolador PIC y un computador, permitiendo el envío de datos para su visualización en una gráfica en Python. Esto facilitó el análisis del comportamiento de señales en función del tiempo;En conjunto, la práctica permitió comprender el funcionamiento de los protocolos I2C y UART, así como su aplicación en la visualización y transmisión de datos.


## Objetivos
### Objetivo general

Desarrollar un sistema de visualización utilizando una pantalla LCD 16x2 con comunicación I2C, controlada mediante un microcontrolador.

### Objetivos especificos

-Comprender el funcionamiento de la comunicación UART.

-Configurar la transmisión serial entre el PIC y el computador.

-Visualizar información en una pantalla LCD.

-Recibir datos seriales en Python.

-Generar una gráfica de voltaje vs tiempo.

-Analizar el comportamiento de la señal transmitida.

## Procedimiento

Se realizó la conexión del microcontrolador PIC con la pantalla LCD sobre la protoboard.

Se configuró la comunicación UART utilizando los pines TX y RX del microcontrolador.

Se programó el PIC para enviar datos seriales continuamente al computador.

Se conectó el módulo USB-UART al computador para permitir la transmisión serial.

En Python se configuró la lectura de datos enviados mediante UART.

Se generó una gráfica de voltaje contra tiempo utilizando los datos recibidos.

Finalmente, se analizó el comportamiento de la señal obtenida.

## Diagramas interno

<img width="1228" height="573" alt="image" src="https://github.com/user-attachments/assets/eb408e14-c34e-405d-b64f-56f0e7a55054" />



Activación del puerto serie (RCSTA_bit7_R/W): 1→Habilitado, 0→ Deshabilitado.

Habilitación de transmisión (TXTA_bit5_R/W): 1→Habilitado, 0→ Deshabilitado.

Determinar el divisor del reloj para establecer la velocidad de baudios.

Dato a transmitir.

Registro de desplazamiento interno. Es el registro interno al que el hardware del UART transfiere los datos del buffer TXREG para luego enviarlos bit a bit por la línea TX.

Estado del registro TSR (TXSTA_bit1_R): 1→Vacío, 0→ Lleno.

Noveno bit a transmitir (TXSTA_bit0_W). Bit de paridad.

Habilitación del noveno bit (TXSTA_bit6_R/W): 1→Habilitado, 0→ Deshabilitado.

Habilitación de interrupción de transmisión (PIE1_bit4_R/W): 1→Habilitado, 0→ Deshabilitado.

Flag de interrupción de transmisión (PIR1_bit4_R/W): 1→Buffer de datos vacío, 0→ Deshabilitado.

## Diagramas del montaje 

<img width="1389" height="1132" alt="image" src="https://github.com/user-attachments/assets/226d9863-49e4-4f20-8f71-b12fe70920fa" />



Durante el desarrollo del proyecto se realizó correctamente el montaje de un sistema de comunicación UART utilizando el microcontrolador PIC18f45k22, una pantalla LCD 16x2 y un módulo USB-TTL sobre protoboard. Se logró establecer la comunicación serial entre el microcontrolador y el computador, permitiendo transmitir y visualizar información de manera adecuada.

Además, la pantalla LCD mostró correctamente los mensajes programados, demostrando el funcionamiento adecuado de las conexiones y de la configuración del microcontrolador. El uso del protocolo UART permitió enviar datos de forma simple y eficiente mediante los pines TX y RX.
## Diagrama en python 

<img width="1628" height="966" alt="image" src="https://github.com/user-attachments/assets/ce5c6440-fe3e-4f47-8a88-926a2cf76572" />


Por otra parte, la gráfica obtenida permitió analizar el comportamiento de la señal transmitida. Se observó una variación periódica del voltaje entre 0V y 5V con forma de diente de sierra, lo cual indica que el sistema estaba generando y enviando datos continuamente durante el tiempo de prueba.

Este proyecto permitió comprender de manera práctica el funcionamiento de la comunicación serial UART, la conexión de dispositivos electrónicos en protoboard y el uso de pantallas LCD para visualizar información. También ayudó a fortalecer conocimientos sobre programación de microcontroladores, transmisión de datos y análisis de señales eléctricas mediante gráficas.

Finalmente, se concluye que el sistema funcionó correctamente, cumpliendo con los objetivos propuestos y demostrando la importancia de la comunicación serial en aplicaciones electrónicas y sistemas
## Documentacion del codigo
## Configuración del microcontrolador (MAIN)

Esta parte del programa es una de las más importantes, ya que aquí se realiza la configuración inicial del PIC18F4550 antes de comenzar la comunicación serial UART. En esta sección se define la frecuencia de trabajo del microcontrolador y se inicializa el módulo UART, el cual permitirá enviar datos desde el PIC hacia la computadora mediante el módulo FT232RL.


<img width="928" height="68" alt="image" src="https://github.com/user-attachments/assets/6581c187-2c91-427b-abb6-64bf1a4642f8" />

La instrucción OSCCON = 0b01110000; configura el oscilador interno del PIC para trabajar a una frecuencia de 16 MHz. Esta frecuencia es importante porque determina la velocidad de ejecución de las instrucciones y afecta directamente la comunicación UART. Si la frecuencia no está configurada correctamente, la transmisión serial puede presentar errores o pérdida de datos.

Posteriormente, la función UART_Init(); se encarga de configurar todos los parámetros necesarios para la comunicación serial. Dentro de esta función se habilitan los pines de transmisión y recepción, se configura el baudrate a 9600 baudios y se activa el módulo UART del microcontrolador.

Después de la configuración inicial, el programa entra en un ciclo infinito utilizando while(1), lo que permite que el PIC ejecute continuamente el envío de datos seriales.


<img width="945" height="108" alt="image" src="https://github.com/user-attachments/assets/2c0a8a84-023f-4e1d-82eb-cfac6dfe80df" />

En esta parte del código, la función UART_WriteString() envía un mensaje de texto a través de UART hacia la computadora. El texto transmitido es "Hola, UART funcionando!", lo que sirve para comprobar que la comunicación serial está funcionando correctamente.

La instrucción __delay_ms(1000); genera un retardo de un segundo entre cada transmisión. Esto evita que los datos sean enviados demasiado rápido y permite observar claramente la recepción de la información en el monitor serial o en la gráfica realizada con Python.

En conjunto, esta sección del código permite inicializar el sistema, establecer la comunicación serial y realizar el envío continuo de datos desde el PIC18F45k22 hacia la computadora.


## Configuración UART (uart.c)

Esta sección del código es una de las más importantes del proyecto, ya que aquí se realiza toda la configuración necesaria para establecer la comunicación serial UART entre el PIC18F45k22 y la computadora mediante el módulo FT232RL. En esta parte se definen los pines de transmisión y recepción, la velocidad de comunicación y la habilitación del módulo serial del microcontrolador.


<img width="950" height="60" alt="image" src="https://github.com/user-attachments/assets/dcfe5658-7906-4933-8f0d-d2231cc090f2" />

Las instrucciones anteriores configuran los pines RC6 y RC7 del PIC. El pin RC6 funciona como TX (transmisión) y se configura como salida porque enviará los datos hacia la computadora. El pin RC7 funciona como RX (recepción) y se configura como entrada porque recibirá información externa si es necesario.

Posteriormente se configura el baudrate de comunicación.


<img width="947" height="82" alt="image" src="https://github.com/user-attachments/assets/4c9f160c-ef4c-4635-a5bf-3b762b8af06b" />

La variable SPBRG1 establece la velocidad de transmisión serial en 9600 baudios utilizando un oscilador de 16 MHz. Esta velocidad debe coincidir con la configurada en Python para evitar errores de comunicación. Además, se selecciona el modo de baja velocidad y un generador de baudrate de 8 bits.

Después se habilita el módulo UART.


<img width="962" height="102" alt="image" src="https://github.com/user-attachments/assets/cad8278c-8b0d-4a5b-8497-5557097ba34f" />

La instrucción SPEN = 1 activa el puerto serial del PIC. Luego, SYNC = 0 configura la UART en modo asíncrono, que es el modo utilizado para la comunicación con computadoras. Finalmente, TXEN = 1 habilita la transmisión de datos y CREN = 1 activa la recepción serial.

Gracias a esta configuración, el PIC puede enviar y recibir información correctamente a través del FT232RL.

También se habilitan las interrupciones UART.


<img width="947" height="112" alt="image" src="https://github.com/user-attachments/assets/05b9f70b-5397-45f3-83bc-4e237d100195" />

Estas instrucciones permiten activar las interrupciones relacionadas con la recepción serial. Las interrupciones ayudan a que el microcontrolador pueda detectar datos entrantes sin necesidad de revisar constantemente el puerto serial.


## Función de transmisión de caracteres

Esta función se encarga de enviar caracteres individuales por UART.


<img width="941" height="195" alt="image" src="https://github.com/user-attachments/assets/c9f92868-7a0a-4579-aa57-9099db2e93be" />

La instrucción while (!TXSTA1bits.TRMT); espera hasta que el buffer de transmisión esté vacío. Esto garantiza que el carácter anterior ya fue enviado antes de transmitir uno nuevo.

Luego, TXREG1 = data; coloca el dato dentro del registro de transmisión UART para enviarlo serialmente.

Esta función es importante porque controla el envío correcto de cada carácter hacia la computadora.


## Función de envío de cadenas de texto

Esta función permite transmitir mensajes completos utilizando UART.


<img width="947" height="241" alt="image" src="https://github.com/user-attachments/assets/413d29b5-e322-4cf5-afdd-9de6ac93dffc" />

La función recibe una cadena de texto y envía cada carácter individualmente utilizando UART_WriteChar().

El ciclo while (*str) recorre toda la cadena hasta encontrar el carácter nulo \0, que indica el final del texto.

Gracias a esta función, el PIC puede enviar mensajes completos como:


<img width="945" height="47" alt="image" src="https://github.com/user-attachments/assets/e754cc36-73de-4647-b637-00625690f1a1" />


## Archivo uart.h

El archivo .h contiene las declaraciones de funciones y configuraciones generales del proyecto UART.


<img width="942" height="41" alt="image" src="https://github.com/user-attachments/assets/06fb48bc-a898-4d52-9355-176f3c0871e2" />

Esta línea define la frecuencia del oscilador del microcontrolador en 16 MHz. Esta definición es importante porque las funciones de retardo, como __delay_ms(), dependen de esta frecuencia para funcionar correctamente.

También se declaran las funciones utilizadas en el programa.


<img width="957" height="135" alt="image" src="https://github.com/user-attachments/assets/47f66daf-8c37-4520-9ca2-f60af9e72da9" />

Estas declaraciones permiten que las funciones puedan ser utilizadas desde otros archivos del proyecto.


## Código Python – Comunicación serial

Esta sección del programa Python se encarga de recibir los datos enviados por el PIC y procesarlos para generar una gráfica en tiempo real.

<img width="929" height="31" alt="image" src="https://github.com/user-attachments/assets/39068678-3810-45e2-bf1c-a472de4ce8d8" />


Esta instrucción abre el puerto serial COM10 y establece la comunicación a 9600 baudios, la misma velocidad configurada en el PIC.

El parámetro timeout=1 indica que Python esperará un segundo por nuevos datos antes de continuar.


## Lectura de datos UART en Python

<img width="952" height="41" alt="image" src="https://github.com/user-attachments/assets/4fcdcb50-40e1-403b-bda3-a15cd509948c" />


Aquí Python lee una línea completa enviada desde el PIC a través del puerto serial.

La función decode('utf-8') convierte los datos binarios en texto legible y strip() elimina espacios o saltos de línea innecesarios.



## Extracción del voltaje recibido


<img width="951" height="32" alt="image" src="https://github.com/user-attachments/assets/ae63bba7-0d72-4fc6-b3f6-6b696c58f5f4" />

Esta línea utiliza expresiones regulares para buscar el valor numérico del voltaje dentro del mensaje recibido.

Por ejemplo, si el PIC envía:


<img width="942" height="46" alt="image" src="https://github.com/user-attachments/assets/05d0cef0-53d7-46d7-9fef-bbd3918df570" />

Python extrae únicamente el valor 3.5 para utilizarlo en la gráfica.


## Graficación en tiempo real


<img width="947" height="32" alt="image" src="https://github.com/user-attachments/assets/3a35db69-71c1-42fc-815a-b070938557c7" />


Esta instrucción genera la gráfica en tiempo real utilizando Matplotlib.

El eje X representa el tiempo y el eje Y representa el voltaje recibido desde el PIC.

También se configuran los límites y etiquetas de la gráfica.



<img width="946" height="185" alt="image" src="https://github.com/user-attachments/assets/515182d9-430b-45ce-8553-8a5361382aa8" />

Estas instrucciones mejoran la visualización de los datos, mostrando claramente el comportamiento del voltaje enviado mediante UART.


## Animación de la gráfica


<img width="943" height="32" alt="image" src="https://github.com/user-attachments/assets/f08d8ede-51f2-4b19-a279-d90c6e92b612" />

Esta función actualiza automáticamente la gráfica cada segundo, permitiendo visualizar los datos en tiempo real conforme llegan desde el PIC.

Gracias a esto, el sistema puede monitorear continuamente la señal transmitida por UART.
