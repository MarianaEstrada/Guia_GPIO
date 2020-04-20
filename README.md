# General purpose input/output (GPIO)
Son pines genéricos que pueden ser configurados como una entrada o una salida para ser usados en el tiempo de ejecución.Para la configuración de estos pines se deben seguir los siguientes pasos:

## AHB2 peripheral clock enable register (RCC_AHB2ENR)

Primero se deben activar los bancos que van a ser usados, para esto es necesario mirar en el esquemático de la tarjeta ([MB1136  schematic board](https://drive.google.com/drive/folders/1zetsGlHlD79WABo6YgIWQx73YNY_aocd "MB1136 Schematic Board ")) a qué banco pertenece el pin que va a ser usado. Para activar un banco se debe poner un 1 en la casilla correspondiente y posteriormente convertir este valor a hexadecimal.

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

Convirtiendo a hexadecimal:

![GPIOH_ODR_ej1_ba](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/GPIOH_ODR_ej1_ba.PNG)

Entonces:

GPIOA -> ODR |= 0x00E0;

Para el banco B en los pines 5 y 6.

![GPIOB_ODR_ej1_bb](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/GPIOB_ODR_ej1_bb.PNG)

Convirtiendo a hexadecimal

![GPIOH_ODR_ej1_bb](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/GPIOH_ODR_ej1_bb.PNG)

Entonces

GPIOA -> ODR |= 0x0060;

Hasta ahora se han configurado los pines que van a ser usados, ahora en la sección del programa while se va a programar como se va a ejecutar la secuencia:

Primero se va a definir la siguiente variable antes del main, de la siguiente forma:

~~~
#def LEDDELAY_1 50000
~~~

En el int main (void), se va a poner toda la configuración de los pines hecha anteriormente, además se fa a definir una variable a, de la siguiente forma:

~~~
//int main (void)
{
Int a;
a=0;
//Se habilita el banco A y B
RCC ->AHB2ENR= 0x00000003;
// Se habilitan como salidas los pines que se van a usar
//Para el banco A
GPIOA ->MODER &=  0XABFFFFFF
GPIOA ->MODER &= 0xFFFFF57FF;
//Para el banco B
GPIOB ->MODER &= 0xFFFFFEBF
GPIOA ->MODER &= 0xFFFFD7FF;
//Poner en estado lógico alto los pines que se van a usar
GPIOA -> ODR |= 0x00E0;
GPIOA -> ODR |= 0x0060;
~~~

Ahora en el While (1), se hará lo siguiente:

~~~
While(1)
{
~~~

Se van a poner los LED en el siguiente orden A6,B5,A5,B6,A7

Primero se va a encender el LED ubicado en el  pin A5, por esta razón se le va a enviar un 1 a esta posición y las otras estarán en 0.
Convirtiendo a hexadecimal

![while_ej1](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/while_ej1.PNG)

~~~
//Entonces
GPIOA ->ODR = 0x0020;
//Como el banco B no se va a encender ningún pin se va a poner todo en 0:
GPIOB ->ODR = 0x0000;
//Y este estará encendido, mientras se cumpla lo siguiente:
While(a< LEDDELAY_1){
a=a+1;}
// Segundo se van a encender los LED’s ubicados en los pines A5,B5,B6 siguiendo la metodología anterior, tratando cada uno de los bancos por separado, se tiene que:
a=0
GPIOA ->ODR = 0x0020;
GPIOB ->ODR = 0x0060;
While(a< LEDDELAY_1){
a=a+1;}
 // Tercero se van a encender los LED’s ubicados en los pines A5,A6,A7,B5 Y B6 siguiendo la metodología anterior, tratando cada uno de los bancos por separado, se tiene que:
a=0
GPIOA ->ODR = 0x00E0;
GPIOB ->ODR = 0x0060;
While(a< LEDDELAY_1){
a=a+1;}
// Cuarto se van a apagar los LED’s ubicados en los pines A6 y A7 siguiendo la metodología anterior, tratando cada uno de los bancos por separado, se tiene que:
a=0
GPIOA ->ODR = 0x0020;
GPIOB ->ODR = 0x0060;
While(a< LEDDELAY_1){
a=a+1;}
// Quinto se van a apagar los LED’s ubicados en los pines A6,A7,B5 y B6 siguiendo la metodología anterior, tratando cada uno de los bancos por separado, se tiene que:
a=0
GPIOA ->ODR = 0x0020;
GPIOB ->ODR = 0x0000;
While(a< LEDDELAY_1){
a=a+1;}
// Sexto se va a apagar el LED ubicado en el pin A5 siguiendo la metodología anterior, tratando cada uno de los bancos por separado, se tiene que:
a=0
GPIOA ->ODR = 0x0000;
GPIOB ->ODR = 0x0000;
While(a< LEDDELAY_1){
a=a+1;}
}
}
~~~

## Ejercicio 

Se va a realizar una secuencia de luces de la siguiente forma:

![ej2](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/ej2.PNG)

Se van a usar los pines B1, B2,B7,C7 y C9

* Primero se van a activar los bancos B y C, poniendo en 1 en las casillas 1 y 2.

![RCCB_EJ2](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/RCCB_EJ2.PNG)

Convirtiendo este valor a hexadecimal:

![RCCH_EJ2](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/RCCH_EJ2.PNG)

Entonces
RCC ->AHB2ENR= 0x00000006;

* Segundo, se van a configurar los pines como salidas: Para el banco B se tienen los pines 1, 2 y 7; como son salidas de propósito general se pone 01.

![GPIOB_moder_ej2_bbc](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/GPIOB_moder_ej2_bbc.PNG)

Convirtiendo este valor a hexadecimal:

![GPIOH_moder_ej2_bb](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/GPIOH_moder_ej2_bb.PNG)

Entonces, primero se reinicia el pin con los valores ya predeterminados

 GPIOB ->MODER &=  0XFFFF7FD7
 
Y finalmente se pone la configuración de salidas.

GPIOB ->MODER &= 0XFFFF7F5F;

Para el banco C se tienen los pines 7 y 9; como son salidas de propósito general se pone 01.

![GPIOB_moder_ej2_bb](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/GPIOB_moder_ej2_bb.PNG)

Conviertiendo este valor a hexadecimal:

![GPIOH_moder_ej2_bc](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/GPIOH_moder_ej2_bc.PNG)

Entonces, primero se reinicia el pin con los valores ya predeterminados

 GPIOC ->MODER &= 0XFFFFFFFF;
 
Y finalmente se pone la configuración de salidas.

GPIOC ->MODER &= 0XFFF77FFF;

* Finalmente se pone un estado de alto lógico en los pines que se van a utilizar: Para el banco B en los pines 1,2 y 7

![GPIOB_ODR_ej2_bb](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/GPIOB_ODR_ej2_bb.PNG)

Convirtiendo a hexadecimal:

![GPIOH_ODR_ej2_bb](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/GPIOH_ODR_ej2_bb.PNG)

Entonces

GPIOB -> ODR |= 0x0085;

Para el banco C en los pines 7 y 9.

![GPIOB_ODR_ej2_bc](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/GPIOB_ODR_ej2_bc.PNG)

Convirtiendo a hexadecimal:

![GPIOH_ODR_ej2_bc](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/GPIOH_ODR_ej2_bc.PNG)

Entonces:

GPIOC -> ODR |= 0x0240;

Hasta ahora se han configurado los pines que van a ser usados, ahora en la sección del programa while se va a programar como se va a ejecutar la secuencia:

Primero se va a definir la siguiente variable antes del main, de la siguiente forma:

~~~
#define LEDDELAY_1 500000
~~~

En el int main (void), se va a poner toda la configuración de los pines hecha anteriormente, además se fa a definir una variable a, de la siguiente forma:

~~~
//int main (void)
{
Int a;
a=0;
//Se habilita el banco B y C
RCC ->AHB2ENR= 0x00000006;
// Se habilitan como salidas los pines que se van a usar
//Para el banco B
GPIOB ->MODER &=  0XFFFF7FD7
GPIOB ->MODER &= 0XFFFF7F5F;
//Para el banco C
GPIOC ->MODER &= 0XFFFFFFFF;
GPIOC ->MODER &= 0XFFF77FFF;
//Poner en estado lógico alto los pines que se van a usar
GPIOB -> ODR |= 0x0085;
GPIOC -> ODR |= 0x0240;
~~~

Ahora en el While (1), se hará lo siguiente:

~~~
while(1)
{
~~~

Se van a poner los LED en el siguiente orden B1,B2,B7,C7,C9

Primero se va a encender el LED ubicado en el  pin B1, por esta razón se le va a enviar un 1 a esta posición y las otras estarán en 0.

Convirtiendo a hexadecimal:

![while_ej2](https://github.com/MarianaEstrada/Guia_GPIO/blob/master/Imagenes/while_ej2.PNG)

~~~
//Entonces
GPIOB->ODR = 0x0002;
//Como el banco C no se va a encender ningún pin se va a poner todo en 0:
GPIOC ->ODR = 0x0000;
//Y este estará encendido, mientras se cumpla lo siguiente:
While(a< LEDDELAY_1){
a=a+1;}
// Segundo se va a encender el LED ubicado en el pin B2 siguiendo la metodología anterior, tratando cada uno de los bancos por separado, se tiene que:
a=0
GPIOB ->ODR = 0x0004;
GPIOC ->ODR = 0x0000;
While(a< LEDDELAY_1){
a=a+1;}
 // Tercero se va a encender el LED ubicado en el pin B7 siguiendo la metodología anterior, tratando cada uno de los bancos por separado, se tiene que:
a=0
GPIOB ->ODR = 0x0080;
GPIOC ->ODR = 0x0000;
While(a< LEDDELAY_1){
a=a+1;}
// Cuarto se va a encender el LED ubicado en el pin C7 siguiendo la metodología anterior, tratando cada uno de los bancos por separado, se tiene que:
a=0
GPIOB ->ODR = 0x0000;
GPIOC ->ODR = 0x0080;
While(a< LEDDELAY_1){
a=a+1;}
// Quinto se va a encender el LED ubicado en el pin C9 siguiendo la metodología anterior, tratando cada uno de los bancos por separado, se tiene que:
a=0
GPIOB ->ODR = 0x0000;
GPIOC ->ODR = 0x0200;
While(a< LEDDELAY_1){
a=a+1;}
// Sexto e va a encender el LED ubicado en el pin C7 siguiendo la metodología anterior, tratando cada uno de los bancos por separado, se tiene que:
a=0
GPIOB ->ODR = 0x0000;
GPIOC ->ODR = 0x0080;
While(a< LEDDELAY_1){
a=a+1;}
// Séptimo e va a encender el LED ubicado en el pin B7 siguiendo la metodología anterior, tratando cada uno de los bancos por separado, se tiene que:
a=0
GPIOB ->ODR = 0x0080;
GPIOC ->ODR = 0x0000;
While(a< LEDDELAY_1){
a=a+1;}
// Octavo e va a encender el LED ubicado en el pin B2 siguiendo la metodología anterior, tratando cada uno de los bancos por separado, se tiene que:
a=0
GPIOB ->ODR = 0x0004;
GPIOC ->ODR = 0x0000;
While(a< LEDDELAY_1){
a=a+1;}
}
}
~~~

## Tarea

Realizar una secuencia de LED's diferente a las anteriormente planteadas.


