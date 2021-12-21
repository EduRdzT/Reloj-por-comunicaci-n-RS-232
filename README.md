# Reloj-por-comunicaci-n-RS-232
#### `Reloj controlado por comunicación serial con C++`

**Índice**
- [Introduccion](#id1)
- [Objetivos](#id2)
- [Marco Teórico](#id3)
- [Desarrollo Experimental](#id4)
   - [Objetivo 1](#id4-1)
   - [Objetivo 2](#id4-2)
   - [Objetivo 3](#id4-3)
- [Anexo](#id5)


## INTRODUCCIÓN<a name="id1"></a>
  
En esta práctica se utilizará el hardware de la practica anterior y utilizando la misma biblioteca del ioUSBUS para imprimir la fecha y la hora en una matriz de 8 x 24.

Para esto se tendrá que realizar un nuevo software para la interfaz de esta comunicación.

A continuación, se presenta la información que se debe conocer previamente para la realización de la práctica, por ejemplo: el funcionamiento de los componentes a usar ya sea el PIC18F4550, los transistores; las funciones que nos proporciona la biblioteca del ioUSBUS.

## OBJETIVOS<a name="id2"></a>
  
### Objetico general

- Usando el Hardware de la practica anterior implementar una matriz de leds para mostrar la fecha y la hora desplazadas y manteniendo los mensajes por 30 segundos.

### Objetivos específicos 

####Objetivo 1: Hardware 

- Conectar a los dispositivos descritos en la práctica de comunicación serial, una matriz de Led’s de 8x24 tomando en cuenta todas las consideraciones de diseño electrónico (como son: Impedancias, Consumo de energía, etc.). 

#### Objetivo 2: Software desarrolle el programa requerido: 

- Desplegar en forma de mensaje Alfa-Numéricos la Hora y Fecha del sistema, de manera alterna en intervalos de 30 segundos.
Con el siguiente formato
1. Hora Hora:Minutos:Segundos
2. Fecha Día/Mes/Año
3. Ej. Del despliegue
   - Para la hora, formar la cadena de caracteres “Hora: 10:37:42” y mostrarla en la matriz de leds.
   - Para fecha, formar la cadena de caracteres “Fecha: 10/10/18” y mostrarla en la matriz de leds.

Nota: 
- _Los mensajes deben ser desplegados a velocidad tal que puedan ser leídos._
- _Debido a la longitud de los mensajes, los caracteres deben de rotar de derecha a izquierda de tal forma que se despliegue todo el mensaje, y además de manera continua._

Sugerencia:
- Se propone el uso de apuntadores, para optimizar y hacer uso de programación avanzada. 

## MARCO TEÓRICO<a name="id3"></a>

#### PIC18F4550
El PIC18F4550 es uno de los microcontroladores más populares cuando se trata de conectividad USB. Ofrece un alto rendimiento informático con agregado de memoria de programa flash de alta resistencia mejorada, ideal para pequeñas potencias) y aplicaciones de conectividad.

Algunas de sus características son:
- Opera con un rango de voltaje que va de 2V a 5.5 V
- Puerto USB V2.
- RAM 1-Kbyte accesible por USB.
- Relojes externos hasta de 48 MHz.
- Oscilador interno de 31 KHz – 8 MHz configurable por software.
- Pines con salida de alta corriente de hasta 25 mA.
- Puerto USART con soporte para comunicaciones MSSP, SPI e I²C.
- Hasta 13 canales ADC de 10 bits.
- Memoria FLASH con 100,000 ciclos de lecturas escritura típicos.
- Memoria EEPROM con 1, 000,000 ciclos de lectura escritura típicos y retención de datos de hasta 40 años.
- Programación con código de protección.
- Programación ICSP vía dos pines.
- Tiene 40 pines

#### Biblioteca ioUSBUS.
En esta práctica se ocupó la biblioteca ioUSBUS, la cual nos ayuda a leer y a escribir a los dispositivos. Esta biblioteca nos brinda las siguientes funciones:

#### Transistores.
#### Matriz de leds de 8x8.
#### Multiplexación. 

## DESARROLLO EXPERIMENTAL<a name="id4"></a>

#### Objetivo 1: Hardware<a name="id4-1"></a>

El mismo hardware usado en la práctica 1 y el mismo software de la practica 4.

Objetivo 2: Hardware<a name="id4-2"></a>

_Figura 1. Circuito de la practica 5_

Para esta práctica se utilizó el mismo hardware de la práctica anterior (Figura 1) solo que en igual de conectar él envió de datos a los leds se conectó un dispositivo a la matriz de leds y el otro a los transistores.

_Figura 2. Circuito de multiplexación._

En la Figura 2 se muestra como multiplexamos la matriz, aunque solo se muestra una matriz es exactamente lo mismo para las demás, donde el dispositivo 0 controla las columnas y el dispositivo 1 controla las filas y la selección de la matriz que se va a usar.

Objetivo 3: Software, crear un programa para el IDE visual studio (VC).<a name="id4-3"></a>

Realizar un proyecto en VC VS2010 para probar las funciones descritas en el documento.

La solución propuesta para este objetivo se muestra en el siguiente diagrama de flujo.

El código desarrollado a partir del diagrama de flujo anterior se muestra en el anexo 1.

## ANEXO<a name="id5"></a>

####Código desarrollado 

- Inicio del código: declaración de librerías y variables globales.
```C++
#include “stdafx.h"
#include<windows.h>
#using "USBioBUS.dll"

#include <stdlib.h>
#include <iostream>

#include<stdio.h>
#include<conio.h>
#include<math.h>

#include <time.h>
// variables locales 
int matriz[25];
int sel = 1;
int i;
int x[30];
int p[30];

//Código para la muestra de los números, se utilizan tres registros para cada número.

void numeros(int p[30]){//decodifica los numeros para que imprima la matriz
//cero
		p[0] = 0x1F;
		p[10] = 0x11;
		p[20] = 0x1F;
//uno
		p[1] = 0x00;
		p[11] = 0x00;
		p[21] = 0x1F;
//dos
		p[2] = 0x17;
		p[12] = 0x15;
		p[22] = 0x1D;
//tres
		p[3] = 0x15;
		p[13] = 0x15;
		p[23] = 0x1F;
//cuatro
		p[4] = 0x1C;
		p[14] = 0x04;
		p[24] = 0x1F;
//cinco
		p[5] = 0x1D;
		p[15] = 0x15;
		p[25] = 0x17;
//seis
		p[6] = 0x1F;
		p[16] = 0x15;
		p[26] = 0x17;
//siete
		p[7] = 0x10;
		p[17] = 0x14;
		p[27] = 0x1F;
//ocho
		p[8] = 0x1F;
		p[18] = 0x15;
		p[28] = 0x1F;
//nueve
		p[9] = 0x1D;
		p[19] = 0x15;
		p[29] = 0x1F;
}
```
Para que se pueda elegir un numero solo se busca el número que se necesita para la primera posición por ejemplo para el cero se necesita el numero cero para la primera columna del número, para la segunda columna se suma el mismo 0 mas 10 y para la tercera columna se suma al mismo cero mas 20.

- Código para mostrar la fecha y la hora.
```C++
void fecha_hora(){		
	int n=0;
	char dia[2],mes[2],año[2],hora[2],minuto[2],segundo[2];
        time_t tiempo = time(0);
        struct tm *tlocal = localtime(&tiempo);
												// strftime(output,128,"Fecha: %d/%m/%y \nHora: %H:%M:%S",tlocal);
		strftime(dia,30,"%d ",tlocal);
		strftime(mes,30,"%m ",tlocal);
		strftime(año,30,"%y ",tlocal);
		strftime(hora,30,"%H ",tlocal);
		strftime(minuto,30,"%M ",tlocal);
		strftime(segundo,30,"%S ",tlocal);
       // printf("Fecha:%s/%s/%s \n",dia,mes, año);
														/*printf("mes %s\n",mes);
														printf("año %s\n",año);*/
		//printf("Hora: %s:%s:%s \n",hora,minuto,segundo);
														//printf("minuto %s\n",minuto);
														//printf("segundo %s\n",segundo);

			////////////*separación de dígitos*///////////
//dia
for(i=0;dia[i+1]!='\0';i++) 
{
  if(isdigit(dia[i]))
	  n=n+1;
	x[n]=dia[i]-48;
}
//mes
for(i=0;mes[i+1]!='\0';i++) 
{
  if(isdigit(mes[i]))
	  n=n+1;
	x[n]=mes[i]-48;
 }
//año
for(i=0;año[i+1]!='\0';i++) 
{
	if(isdigit(año[i]))
		n=n+1;
		x[n]=año[i]-48;
}
//hora
for(i=0;hora[i+1]!='\0';i++) 
{
	if(isdigit(hora[i]))
		n=n+1;
		x[n]=hora[i]-48;
}
//minuto
for(i=0;minuto[i+1]!='\0';i++) 
{
	if(isdigit(minuto[i]))
		n=n+1;
		x[n]=minuto[i]-48;
}
//segundo
for(i=0;segundo[i+1]!='\0';i++) 
{
	if(isdigit(segundo[i]))
		n=n+1;
		x[n]=segundo[i]-48;
}
}
```
Para esta parte del código se manda a llamar la función para la fecha y la hora, luego se separan en registros las horas, minutos, segundos, año, mes y día para después separarlos por dígitos en vectores para luego ser llamados.

- Código para guardar en un vector todos los datos a mostrar en la matriz.
```C++
void decoder(int sel,int p[30],int x[30]){
	if (sel == 1){  //HORA
		//H
		matriz[4] = 0x1F;
		matriz[5] = 0x04;
		matriz[6] = 0x1F;
		//O
		matriz[8] = 0x1F;
		matriz[9] = 0x11;
		matriz[10] = 0x1F;
		//R
		matriz[12] = 0x1F;
		matriz[13] = 0x16;
		matriz[14] = 0x1D;
		//A
		matriz[16] = 0x0F;
		matriz[17] = 0x14;
		matriz[18] = 0x0F;
	}
	else if(sel == 2){  //numeros
		matriz[0] = p [x[7]];
		matriz[1] = p [x[7]+10];
		matriz[2] = p [x[7]+20];

		matriz[4] = p [x[8]];
		matriz[5] = p [x[8]+10];
		matriz[6] = p [x[8]+20];

		matriz[7] = 0x0A;

		matriz[8] = p [x[9]];
		matriz[9] = p [x[9]+10];
		matriz[10] = p [x[9]+20];

		matriz[12] = p [x[10]];
		matriz[13] = p [x[10]+10];
		matriz[14] = p [x[10]+20];

		matriz[15] = 0x0A;

		matriz[16] = p [x[11]];
		matriz[17] = p [x[11]+10];
		matriz[18] = p [x[11]+20];

		matriz[20] = p [x[12]];
		matriz[21] = p [x[12]+10];
		matriz[22] = p [x[12]+20];
	}
	else if(sel == 3){   //FECHA
		//F
		matriz[2] = 0x1F;
		matriz[3] = 0x14;
		matriz[4] = 0x10;
		//E
		matriz[6] = 0x1F;
		matriz[7] = 0x15;
		matriz[8] = 0x11;
		//C
		matriz[10] = 0x1F;
		matriz[11] = 0x11;
		matriz[12] = 0x11;
		//H
		matriz[14] = 0x1F;
		matriz[15] = 0x04;
		matriz[16] = 0x1F;
		//A
		matriz[18] = 0x0F;
		matriz[19] = 0x14;
		matriz[20] = 0x0F;
	}
	else{  //numeros
		
		matriz[0] = p [x[1]];
		matriz[1] = p [x[1]+10];
		matriz[2] = p [x[1]+20];

		matriz[4] = p [x[2]];
		matriz[5] = p [x[2]+10];
		matriz[6] = p [x[2]+20];

		matriz[7] = 0x0A;

		matriz[8] = p [x[3]];
		matriz[9] = p [x[3]+10];
		matriz[10] = p [x[3]+20];

		matriz[12] = p [x[4]];
		matriz[13] = p [x[4]+10];
		matriz[14] = p [x[4]+20];

		matriz[15] = 0x0A;

		matriz[16] = p [x[5]];
		matriz[17] = p [x[5]+10];
		matriz[18] = p [x[5]+20];

		matriz[20] = p [x[6]];
		matriz[21] = p [x[6]+10];
		matriz[22] = p [x[6]+20];
	}
}

//Código para limpiar la matriz.
void limpiar(int matriz[25]){
	for(i = 0; i <= 25; i++){
	matriz[i] = 0;
	}
}
```
Este código solo se usa al terminar de mostrar un mensaje para que al mostrar el siguiente mensaje no se junten los datos.

- Código para mostrar la palabra hora y fecha.

Este código se divide en dos partes. Una para desplazar de derecha hacia al centro de la matriz y la otra para desplazar del centro de la matriz hacia la izquierda.

1. Desplazar de derecha al centro de la matriz.
```C++
sel = 1;
	decoder(sel,p,x);
	for(k = 24;k >= 0; k--){
		for(d = 0; d < 1; d++){
		for(j = 0; j <= 16; j += 8){
			y = y << 1;
			for(c = 0; c < 1; c++){
			for(i = 0; i < 8; i++){
				columna = 1 << i;
				z = (j + i) - k;
				if (z < 0){
					z = 0;
				}
				num = matriz[z] | y;
				MiBUS.EscDisp( 1 , 0 );
				MiBUS.EscDisp( 0 , columna );
				MiBUS.EscDisp( 1 , num );
				for(int g = 0 ; g < 5 ; g ++){}
			}
		}
		}
	y = 16;
		}
	}
```
En la variable sel, si es 1 es para mostrar la palabra “hora” y si es 3 es para mostrar la palabra “fecha”.

2. Desplazar del centro de la matriz hacia la izquierda.
```C++
for(k = 0;k <= 23; k++){
		for(d = 0; d < 1; d++){
		for(j = 0; j <= 16; j += 8){
			y = y << 1;
			for(c = 0; c <1; c++){
			for(i = 0; i < 8; i++){
				columna = 1 << i;
				z = (j + i) + k;
				num = matriz[z] | y;
				MiBUS.EscDisp( 1 , 0 );
				MiBUS.EscDisp( 0 , columna );
				MiBUS.EscDisp( 1 , num );
				for(int g = 0 ; g < 5 ; g ++){}
			}
		}
		}
	y = 16;
		}
	}
```
Para esta parte del código es igual que la anterior para que se centre en la matriz solo que entre los dos códigos de desplazamiento hay uno para mantener la información en el centro durante 30 segundos.
- Código para mostrar las horas – minutos – segundos o día – mes -año.
```C++
for(d = 0 ; d < 160 ; d++){
	fecha_hora();
				decoder(sel,p,x);
	for(j = 0; j <= 16; j += 8){
			y = y << 1;
			for(c = 0 ; c < 1 ; c++){
			for(int s = 0; s < 8; s++){
				columna = 1 << s;
				z = (j + s);
				if (z < 0){
					z = 0;
				}
				
				num = matriz[z] | y;
				MiBUS.EscDisp( 1 , 0 );
				MiBUS.EscDisp( 0 , columna );
				MiBUS.EscDisp( 1 , num );
				for(int g = 0 ; g < 5 ; g ++){}
				//Sleep(1);
			}
	}
		}
	y = 16;
}
```
Para la variable sel, si es 2 muestra hora-minutos-segundos y si es 4 muestra día-mes-año.
