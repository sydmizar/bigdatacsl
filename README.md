# Configuracion Hadoop en Centos 7
This repository shows how to install, configure and use some technologies of the big data landscape. Complex System Laboratory - UPIITA

# Instructivo

NO se asume que el usuario es conocedor en el uso e instalación del sistema operativo CentOS. para este manual fue utilizado la versión 7 de 64 bits de forma minimal y Gnome, realizando la instalación de hadoop 2.9.1 la cual fue la última versión estable hasta ahora en Septiembre 2018.

Las configuraciones de la versión 2.7 a 2.9 son iguales, para la versión 3.0 en adelante cambia mucho. 

```r
letra normal ** negrita ** : comandos ingresados
itálicas : mensajes desplegados, notas.
normal: Explicación principal formato justificado. Explicación extra alineado a la derecha
```

# Instalacion de `CentOS 7` version minima en maquina nativa por medio de un DVD

Para realizar una instalación exitosa debe seguir los pasos detallados en el archivo completo de instalación. En resumen, debe:

* Ingresar a la página del sistema operativo de CentOS y seleccione la descarga mínima.
* Al ingresar en esta opción, seleccione la descarga de Actual Country. 
* Se recomienda grabar la imagen ISO en un disco de DVD, utilice algún software especializado para grabar la información, en este manual se utilizó el software ISO Burner.
* Introduce el disco de instalación en la computadora.
* Seleccione idioma, hora, fech, teclado, tarjeta de red, etc.
* Después de haber dado clic en el anterior botón, se desplegarán dos opciones para introducir las contraseñas del usuario root y usuarios. Para esto elegiremos la contraseña del administrador root la cual será `toor`.
* No importa el nombre del usuario que eliga, pero más adelante será importante trabajar con el usuario `hadoop`.
 


Si planea ir a esta platica, por favor chequee este página regularmente.  Aquí vamos a publicar las instrucciones. También, este repositorio va a tener la presentación y el código que se va a usar durante la sesión.

## Tarea para usted 

- Si desea practicar los ejercicios en vivo durante la sesión, por favor asegúrese de tener los siguientes paquetes de R pre-instalados en su computadora:

```r
  install.packages("sparklyr")
  install.packages("tidyverse")
  install.packages("dbplot")
  install.packages("nycflights13")
  install.packages("janeaustenr")
```

- Asegúrese de tener Java 8 instalado en su computadora, puede bajarlo aquí: https://www.java.com/en/

- También, es necesario tener Spark instalado en su computadora.  La siguiente instrucción también funciona en Windows:

```r
  spark_install("2.0.0")
```

- Para confirmar que todo funciona, use el siguiente comando:
```r
  sc <- spark_connect(master = "local", version = "2.0.0")
```
