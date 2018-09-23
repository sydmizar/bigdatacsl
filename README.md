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

Después de haber dado clic en el anterior botón, se desplegarán dos opciones para introducir las contraseñas del usuario root y usuarios. Para esto elegiremos la contraseña del administrador root la cual será `toor`.

No importa el nombre del usuario que eliga, pero más adelante será importante trabajar con el usuario `hadoop`.

# Instalación de Hadoop en un nodo desde terminal en CentOS 7

Para ejecutar Hadoop y otras de las herramientas de Big Data se requieren instalar algunos paquetes para poder descargar nuestros archivos ejecutables.

Instalar la herramienta wget, para poder descargar java, implemente el siguiente comando:

Ingrese como superusuario (root) 
```r
sudo su -
yum install wget
```
NOTA: el comando sudo, se utiliza para tener permisos de super usuario.

Para poder visualizar nuestras interfaces de red en modo terminal
```r
yum install net-tools
```

Hadoop utiliza Java, así que INSTALE el archivo RPM con el siguiente comando:
```r
$ wget --no-cookies --no-check-certificat --header “Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie” “http://download.oracle.com/otn-pub/java/jdk/8u171-b11/512cd62ec5174c3487ac17c61aaa89e8/jdk-8u171-linux-x64.rpm”
```
Después de terminar la instalación implemente el siguiente comando para la instalación de java:
```r
sudo yum localinstall jdk-8u171-linux-x64.rpm
```
Nota: Al igual que el comando anterior, la instalación dependerá de la versión del jdk.

La versión de java instalada se conoce mediante el comando:
```r
java –version
```
Agregue un nuevo usuario
```r
sudo adduser hadoop
sudo passwd hadoop
```

Ingresar como usuario: hadoop
```r
su – hadoop
```
Configuración de la clave SSH para iniciar de forma segura sin contraseña.
```r
ssh-keygen -t rsa -P “”
```

Este comando le pedirá que ingrese la clave, usted presione enter a todas las opciones.

Ahora con el siguiente comando se generará la clave para que el usuario pueda iniciar sesión con la clave privada y posteriormente con el siguiente comando se le asignarán los permisos para ingesar sin utilizar la contraseña.
```r
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
```
Utilice el siguiente comando para saber si puede ingresar al localhost sin necesidad de utilizar contraseña.
```r
ssh localhost
```
Para salir del usuario: hadoop y regresar como super usuario introduciremos el comando
```r
exit
```
Ahora entraremos a la página oficial de apache hadoop para saber cuál es la versión más actual.

http://hadoop.apache.org/releases.html#Download

La versión más actual es la 2.9.1, desde terminal introduzca el siguiente comando.
INSTALE.
```r
wget http://mirror.rise.ph/apache/hadoop/common/hadoop-2.9.1/hadoop-2.9.1.tar.gz
```
Ahora descomprima el archivo con el siguiente comando.
```r
tar xvfz hadoop-2.9.1.tar.gz
```
Para poder mover la siguiente carpeta siendo usuario hadoop, permita que tenga permisos de superusuario.

Ingrese como usuario root
```r
sudo visudo
```
Agregue al usuario hadoop, debajo del usuario root.
Presione s para insertar, Esc para salir y :wq para guardar y salir.

Recomendación: Para que las configuraciones funcionen para todos los usuarios; las rutas /home/hadoop deben ser sustituidas por /opt/hadoop.

Desplace el contenido de hadoop-2.9.1 al directorio de la cuenta usuario

Mueva el comando como usuario hadoop
```r
sudo mv hadoop-2.9.1/* /home/hadoop/.
```

Nota: Después de copiar los archivos de hadoop-2.9.1 a la carpeta de usuario, no olvide poner los permisos necesarios, ya que usted está copiándolos siendo un superusuario. De no hacerlo tendrá problemas para editar todos los archivos. Si ingresa como usuario hadoop, usted no tendrá problemas con los permisos.

Asignar permisos al archivo
```r
chown --recursive hadoop:hadoop /home/hadoop/
```

Otra opción
```r
chown -R hadoop:hadoop /home/hadoop/
```
El editor de textos por defecto de CentOS, es vi, pero por comodidad se sugiere instalar nano. 
sudo yum install nano

Para encontrar la ruta de instalación de JVM (jre) escribir el comando
```r
sudo update-alternatives --config java
```
El sistema desplegará el siguiente mensaje (esto dependerá de la versión instalada de java)

/usr/java/jdk1.8.0_171-amd64/jre/bin/java

NOTA: En modo gráfico aparecen openjdk como su versión de java por defecto. En modo minimal no viene instalado ninguna versión de java.

Edite el archivo ~/.bashrc con el siguiente comando y escriba la siguiente información
```r
nano ~/.bashrc
```

```r
export PATH
export HADOOP_PREFIX="/home/hadoop"
export HADOOP_HOME="${HADOOP_PREFIX}"
export HADOOP_COMMON_HOME="${HADOOP_PREFIX}"
export HADOOP_CONF_DIR="${HADOOP_PREFIX}/etc/hadoop"
export HADOOP_HDFS_HOME="${HADOOP_PREFIX}"
export HADOOP_MAPRED_HOME="${HADOOP_PREFIX}"
export HADOOP_YARN_HOME="${HADOOP_PREFIX}"
export "PATH=${PATH}:${HADOOP_PREFIX}/bin:${HADOOP_PREFIX}/bin"
export "PATH=${PATH}:${HADOOP_PREFIX}/bin:${HADOOP_PREFIX}/sbin"
export JAVA_HOME=/usr/java/jdk1.8.0_171-amd64/jre
```

Para que tenga efecto estos cambios se debe reiniciar la terminal de comandos
```r
source ~/.bashrc
```
Otra opción
. ~/.bashrc

NOTA: Error hdfs namenode found es debido a que no está compilado las variables de entorno, utilizar source ~/.bashrc.

Los archivos del ambiente de Hadoop que se requieren configurar se presentan a continuación, estos se pueden modificar en cualquier orden. Entre al directorio de la ubicación mediante el comando:
```r
cd ~/etc/hadoop
```
Edite el archivo hadoop-env.sh, usted debe añadir la ruta de la máquina virtual de JAVA para que la utilice el ambiente de Hadoop, así como su directorio HOME y CONF_DIR

```r
nano $HADOOP_HOME/etc/hadoop/hadoop-env.sh

export JAVA_HOME=/usr/java/jdk1.8.0_171-amd64/jre
export HADOOP_HOME=/home/hadoop
export HADOOP_CONF_DIR=/home/hadoop/etc/hadoop
```
Editar archivos de configuración

En general 

http://hadoop.apache.org/docs/rVERSIÓN/hadoop-project-dist/hadoop-common/DeprecatedProperties.html

El resto de la configuración requiere que te dirijas a la ruta:

$HADOOP_HOME tiene la ruta de origen, usted puede modificar esta ruta en .bashrc
```r
cd $HADOOP_HOME/etc/hadoop
```
NOTA: Usted puede usar Tab para a completar la línea que desea escribir.

Editar core-site.xml

NOTA: fs.defaultFS    	Apunta al DATA NODE
NOTA: fs.default.name 	Apunta al NAME NODE
NOTA: La variable fs.default.name es obsoleta en la versión 3.0.3
```r
<configuration>
<property>
<name> fs.defaultFS</name> 
<value>hdfs://localhost:54310</value>
</property>
<property>
<name> fs.default.name</name> 
<value>hdfs://localhost:54310</value>
</property>
</configuration>
```
Editar hdfs-site.xml
```r
<configuration>
   <property>
      <name>dfs.replication</name>
      <value>1</value>
   </property>  
   <property>
      <name>dfs.blocksize</name>
      <value>4194304</value>
   </property>  
   <property>
      <name>dfs.namenode.name.dir</name>
      <value>/home/hadoop/hadoopdata/hdfs/namenode </value>
   </property>
   <property>
      <name>dfs.datanode.data.dir</name> 
      <value>/home/hadoop/hadoopdata/hdfs/datanode </value> 
   </property>
</configuration>
```

Por el momento se tomará este tamaño de bloque para la creación de archivos.

Copie mapred-site.xml.templace como mapred-site.xml
```r
cp mapred-site.xml.templace mapred-site.xml
```
Editar mapred-site.xml
```r
<configuration>
   <property> 
      <name>mapreduce.framework.name</name>
      <value>yarn</value>
   </property>
<property>
        <name>mapred.job.tracker</name>
        <value>localhost:54311</value>
    </property>
    <property>
        <name>mapreduce.jobhistory.webapp.address</name>
        <value>localhost:19888</value>
    </property>
</configuration>
```

Editar yarn-site.xml
```r
<configuration>
   <property>
      <name>yarn.nodemanager.aux-services</name>
      <value>mapreduce_shuffle</value> 
   </property>
<property>
  <name>yarn.nodemanager.aux-services.mapreduce_shuffle.class</name>
  <value>org.apache.hadoop.mapred.ShuffleHandler</value>
  </property>
</configuration>
```

Para el caso de un sistema pseudo distribuido se procede a editar el archivo con el nombre de la máquina maestra33
```r
nano masters
```
El contenido de este archivo debe ser:

localhost

El último archivo que se edita es el que indica las máquinas esclavas, como es un nodo único, solamente es un equipo. El comando de edición es:
```r
nano slaves
```
El contenido de este archivo debe ser:

localhost

NOTA: Verifica que los archivos contengan las palabras localhost, en versiones futuras slaves cambiara por workers 

Al concluir la edición de estos archivos, se procede a formatear el sistema de archivos de Hadoop mediante el siguiente comando:
```r
$ hdfs namenode -format
```
Formatear sirve para iniciar el sistema de archivos distribuido.

Si se ha formateado de la manera correcta, debe aparecerle un mensaje como el siguiente

 

NOTA: Si formateaste como super usuario y quieres volver a intentar el mismo procedimiento como usuario, eliminar la ruta del proceso que incluye /tmp/hadoop-usuario-namenode.pid: Permission denied como usuario root y vuelve a compilar como usuario hadoop. Este error aparece solo al principio de la ejecución del namenode, así que si se encuentra trabajando en terminal tendrá problema para ver ese error ya que la terminal no le permite visualizar toda la ejecución.

Para eliminar un archivo
```r
rm –rf <Ruta>
```
Puede o no entrar a estar dirección para observar donde se encuentran estos procesos.
```r
cd $HADOOP_HOME/sbin/
```
El uso de Hadoop requiere que se activen varios servidores mediante dos scripts:

Se recomienda no usar el anterior comando y usar los siguientes dos comandos:
```r
start-dfs.sh
start-yarn.sh	Namenode,DataNode,SecondaryNameNode 
ResourceManager,NodeManager
```
NOTA: el comando start-all.sh ha quedado obsoleto.

Se comprueba si realmente está en funcionamiento, mediante el comando:
```r
jps
```
Se podrá visualizar los siguientes servicios.

 

NOTA: Si encuentra problemas al inicializar los procesos y no encuentra el datanode, esto se puede solucionar borrando la carpeta datanode, el cual fue asignado en la ruta que usted asignó en hdfs-site.xml.

NOTA: Si al ejecutar start-dfs.sh no aparece el namenode, problema proviene desde el formateo de hdfs namenode –format, posiblemente otra solución es que usted debe borrar el namenode creado en la ruta de hdfs-site.xml

La versión de Hadoop se verifica con:
hadoop version
