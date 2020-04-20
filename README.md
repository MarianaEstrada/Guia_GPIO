# General purpose input/output (GPIO)
Son pines genéricos que pueden ser configurados como una entrada o una salida para ser usados en el tiempo de ejecución.Para la configuración de estos pines se deben seguir los siguientes pasos:

## AHB2 peripheral clock enable register (RCC_AHB2ENR)

Primero se deben activar los bancos que van a ser usados, para esto es necesario mirar en el esquemático de la tarjeta (está ubicado en la carpeta compartida de diseño digital, en la carpeta documentos) a qué banco pertenece el pin que va a ser usado. Para activar un banco se debe poner un 1 en la casilla correspondiente y posteriormente convertir este valor a hexadecimal.

![Img1](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/Img1.PNG)

## GPIO port mode register (GPIOx_MODER) (x =A to I)

Segundo se debe configurar para que va a ser usado el pin, si es una entrada, una salida, unas funciones alternativas o un modo análogo.
Antes de determinar para que va a ser usado el pin, es necesario reiniciarlo con los siguientes valores dependiendo del banco al que pertenezca:

* Para el banco A= 0XABFFFFFF
* Para el banco B= 0xFFFFFEBF
* Para el banco C al G y el banco I =0XFFFFFFFF
* Para el banco H= 0x0000000F  


Cada uno de los bancos poseen 15 pines, es decir, que se va a poner alguna de las siguientes instrucciones:

* 00 Entrada
* 01 Salida de propósito general
* 10 Función alternativa
* 11 Modo análogo

  En el Moder xx, donde xx el número del pin que va a ser utilizado.
  
  ![gpio_moder](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/gpio_moder.PNG)
  
  Posteriormente se convierte este valor a hexadecimal.
  
## GPIO port output data register (GPIOx_ODR) (x = A to I) 

Finalmente se activa con un estado de alto lógico (1) a los pines que van a ser usados.

![gpio_odr](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/gpio_odr.PNG)

## Ejercicio 1

Se va a realizar una secuencia de luces de la siguiente forma:

![ej1](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/ej1.PNG)

Se van a usar los pines A5,A6,A7,B5,B6.
* Primero se van a activar los bancos A y B, poniendo en 1 en las casillas 0 y 1.

![RCCB_EJ1](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/RCCB_EJ1.PNG)

Convirtiendo este valor a hexadecimal

![RCCH_EJ1](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/RCCH_EJ1.PNG)

Entonces:

RCC ->AHB2ENR= 0x00000003;

* Segundo, se van a configurar los pines como salidas:

Para el banco A se tienen los pines 5, 6 y 7; como son salidas de propósito general se pone 01.

![GPIO_moder_ej1_ba](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/GPIO_moder_ej1_ba.PNG)

Convirtiendo este valor a hexadecimal:

![GPIOH_moder_ej1_ba](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/GPIOH_moder_ej1_ba.PNG)

Entonces, primero se reinicia el pin con los valores ya predeterminados.

GPIOA ->MODER &=  0XABFFFFFF

Y finalmente se pone la configuración de salidas.

GPIOA ->MODER &= 0xFFFFF57FF;

Para el banco B se tienen los pines 5 y 6; como son salidas de propósito general se pone 01.

![GPIOB_moder_ej1_bb](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/GPIOB_moder_ej1_bb.PNG)

Convirtiendo a hexadecimal

![GPIOH_moder_ej1_bb](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/GPIOH_moder_ej1_bb.PNG)

Entonces, primero se reinicia el pin con los valores ya predeterminados

 GPIOB ->MODER &= 0xFFFFFEBF
 
Y finalmente se pone la configuración de salidas.

GPIOA ->MODER &= 0xFFFFD7FF;

* Finalmente se pone un estado de alto lógico en los pines que se van a utilizar:

Para el banco A en los pines 5,6 y 7

![GPIOB_ODR_ej1_ba](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/GPIOB_ODR_ej1_ba.PNG)


