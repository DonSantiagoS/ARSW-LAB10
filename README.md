## Escuela Colombiana de Ingeniería

## Arquitecturas de Software - ARSW

### LABORATORIO N°10

## Escalamiento en Azure con Maquinas Virtuales, Sacale Sets y Service Plans

### Dependencias
* Cree una cuenta gratuita dentro de Azure. Para hacerlo puede guiarse de esta [documentación](https://azure.microsoft.com/en-us/free/search/?&ef_id=Cj0KCQiA2ITuBRDkARIsAMK9Q7MuvuTqIfK15LWfaM7bLL_QsBbC5XhJJezUbcfx-qAnfPjH568chTMaAkAsEALw_wcB:G:s&OCID=AID2000068_SEM_alOkB9ZE&MarinID=alOkB9ZE_368060503322_%2Bazure_b_c__79187603991_kwd-23159435208&lnkd=Google_Azure_Brand&dclid=CjgKEAiA2ITuBRDchty8lqPlzS4SJAC3x4k1mAxU7XNhWdOSESfffUnMNjLWcAIuikQnj3C4U8xRG_D_BwE). Al hacerlo usted contará con $200 USD para gastar durante 1 mes.

De manera gratuita con Imagine Azure se creo la cuenta estudiantil gratuita para llevar a cabo la solucion del laboratorio

![](images/solucion/cuentaNueva.PNG)

Una vez creada la cuenta se procede a iniciar con el desarrollo del Laboratorio

### Parte 0 - Entendiendo el escenario de calidad

Adjunto a este laboratorio usted podrá encontrar una aplicación totalmente desarrollada que tiene como objetivo calcular el enésimo valor de la secuencia de Fibonnaci.

**Escalabilidad**
Cuando un conjunto de usuarios consulta un enésimo número (superior a 1000000) de la secuencia de Fibonacci de forma concurrente y el sistema se encuentra bajo condiciones normales de operación, todas las peticiones deben ser respondidas y el consumo de CPU del sistema no puede superar el 70%.

### Parte 1 - Escalabilidad vertical

1. Diríjase a el [Portal de Azure](https://portal.azure.com/) y a continuación cree una maquina virtual con las características básicas descritas en la imágen 1 y que corresponden a las siguientes:
    * Resource Group = SCALABILITY_LAB
    * Virtual machine name = VERTICAL-SCALABILITY
    * Image = Ubuntu Server 
    * Size = Standard B1ls
    * Username = scalability_lab
    * SSH publi key = Su llave ssh publica


![](images/solucion/parte01.PNG)

![](images/solucion/maquina1.PNG)

![](images/solucion/maquina2.PNG)


![Imágen 1](images/part1/part1-vm-basic-config.png)

2. Para conectarse a la VM use el siguiente comando, donde las `x` las debe remplazar por la IP de su propia VM.

    `ssh scalability_lab@xxx.xxx.xxx.xxx`
	
	![](images/solucion/connecion.PNG)

3. Instale node, para ello siga la sección *Installing Node.js and npm using NVM* que encontrará en este [enlace](https://linuxize.com/post/how-to-install-node-js-on-ubuntu-18.04/).
	
	![](images/solucion/nodeInstalado.PNG)
	
	Estando conectado a la maquina virtual instalar Node.js y npm usando NVM con el siguiente comando:
	
	`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash `
	
	![](images/solucion/node1.PNG)
	
	En esta seccion es importante cerrar la terminal y abrirla de nuevo para que se agreguen las rutas al nvm script de la secion del shell actual, o agregarlas manualmente
	Para posteriormente verificar con el comando nvm --version la version y de esta forma comprobar que todo esta en orden
	
	![](images/solucion/node2.PNG)
	
	Posterior a esto, ya contando con nvm se procede a instalar node.js con el siguiente comando:
	
	`nvm install node`
	
	obteniendo la siguiente salida y a su vez comprobando la version correspondiente del node.js:
	
	![](images/solucion/nodeinstalado.PNG)
	
	`node --version`
	
	![](images/solucion/nodeversion.PNG)
	
	
	posteriormente se llevara acabo la instalacion de la ultima version correspondientes a **LTS** y la version del mismo 8.10.0
	
	`nvm install --lts`
	
	`nvm install 8.10.0`
	
	![](images/solucion/lts.PNG)
	
4. Para instalar la aplicación adjunta al Laboratorio, suba la carpeta `FibonacciApp` a un repositorio al cual tenga acceso y ejecute estos comandos dentro de la VM:

    `git clone <your_repo>`

	![](images/solucion/clonado.PNG)

    `cd <your_repo>/FibonacciApp`

    `npm install`
	
	![](images/solucion/repo.PNG)

5. Para ejecutar la aplicación puede usar el comando `npm FibinacciApp.js`, sin embargo una vez pierda la conexión ssh la aplicación dejará de funcionar. Para evitar ese compartamiento usaremos *forever*. Ejecute los siguientes comando dentro de la VM.

    `npm install forever -g`

    `forever start FibinacciApp.js`

	![](images/solucion/start.PNG)

6. Antes de verificar si el endpoint funciona, en Azure vaya a la sección de *Networking* y cree una *Inbound port rule* tal como se muestra en la imágen. Para verificar que la aplicación funciona, use un browser y user el endpoint `http://xxx.xxx.xxx.xxx:3000/fibonacci/6`. La respuesta debe ser `The answer is 8`.


### B1ls


![](images/solucion/prueba1.PNG)

![](images/solucion/add.PNG)

### B2ms

![](images/solucion/consolaSegundos.PNG)

![](images/part1/part1-vm-3000InboudRule.png)

7. La función que calcula en enésimo número de la secuencia de Fibonacci está muy mal construido y consume bastante CPU para obtener la respuesta. Usando la consola del Browser documente los tiempos de respuesta para dicho endpoint usando los siguintes valores:
    
	### B1ls
	
	* 1000000
	
	![](images/solucion/consola1.PNG)
	
    * 1010000
	
	![](images/solucion/consola2.PNG)
	
    * 1020000
	
	![](images/solucion/consola3.PNG)
	
    * 1030000
	
	![](images/solucion/consola4.PNG)
	
    * 1040000
	
	![](images/solucion/consola5.PNG)
	
    * 1050000
	
	![](images/solucion/consola6.PNG)
	
    * 1060000
	
	![](images/solucion/consola7.PNG)
	
    * 1070000
	
	![](images/solucion/consola8.PNG)
	
    * 1080000
	
	![](images/solucion/consola9.PNG)
	
    * 1090000    
	
	![](images/solucion/consola10.PNG)
	

### B2ms

* 1000000
	
	![](images/solucion/consolaDiferente1.PNG)
	
    * 1010000
	
	![](images/solucion/consolaDiferente2.PNG)
	
    * 1020000
	
	![](images/solucion/consolaDiferente3.PNG)
	
    * 1030000
	
	![](images/solucion/consolaDiferente4.PNG)
	
    * 1040000
	
	![](images/solucion/consolaDiferente5.PNG)
	
    * 1050000
	
	![](images/solucion/consolaDiferente6.PNG)
	
    * 1060000
	
	![](images/solucion/consolaDiferente7.PNG)
	
    * 1070000
	
	![](images/solucion/consolaDiferente8.PNG)
	
    * 1080000
	
	![](images/solucion/consolaDiferente9.PNG)
	
    * 1090000    
	
	![](images/solucion/consolaDiferente10.PNG)


	

8. Dírijase ahora a Azure y verifique el consumo de CPU para la VM. (Los resultados pueden tardar 5 minutos en aparecer).

Las siguientes imagenes corresponden al consumo de la CPU en la VM, y el consumo de red total, al realizar las difirentes solicitudes para el endpoint con los valores propuestos en el punto (1000000,1010000,1020000,1030000,1040000,1050000,1060000,1070000,1080000,1090000), esto diferenciando el ejercicio primeramente realizado con B1ls y despues con B2ms al realizar el escalamiento vertical

### B1ls

![](images/solucion/consumo.PNG)

### B2ms

![](images/solucion/consumo2.PNG)

![Imágen 2](images/part1/part1-vm-cpu.png)


9. Ahora usaremos Postman para simular una carga concurrente a nuestro sistema. Siga estos pasos.
    * Instale newman con el comando `npm install newman -g`. Para conocer más de Newman consulte el siguiente [enlace](https://learning.getpostman.com/docs/postman/collection-runs/command-line-integration-with-newman/).
	
	
    * Diríjase hasta la ruta `FibonacciApp/postman` en una maquina diferente a la VM.
    * Para el archivo `[ARSW_LOAD-BALANCING_AZURE].postman_environment.json` cambie el valor del parámetro `VM1` para que coincida con la IP de su VM.
    * Ejecute el siguiente comando.

    ```
    newman run ARSW_LOAD-BALANCING_AZURE.postman_collection.json -e [ARSW_LOAD-BALANCING_AZURE].postman_environment.json -n 10 &
    newman run ARSW_LOAD-BALANCING_AZURE.postman_collection.json -e [ARSW_LOAD-BALANCING_AZURE].postman_environment.json -n 10
    ```
	
### B1ls

![](images/solucion/pruebaJson.PNG)

### B2ms

![](images/solucion/pruebaJson2.PNG)

10. La cantidad de CPU consumida es bastante grande y un conjunto considerable de peticiones concurrentes pueden hacer fallar nuestro servicio. Para solucionarlo usaremos una estrategia de Escalamiento Vertical. En Azure diríjase a la sección *size* y a continuación seleccione el tamaño `B2ms`.


![](images/solucion/resize.PNG)

![Imágen 3](images/part1/part1-vm-resize.png)

11. Una vez el cambio se vea reflejado, repita el paso 7, 8 y 9.
12. Evalue el escenario de calidad asociado al requerimiento no funcional de escalabilidad y concluya si usando este modelo de escalabilidad logramos cumplirlo.
13. Vuelva a dejar la VM en el tamaño inicial para evitar cobros adicionales.

**Preguntas**

1. ¿Cuántos y cuáles recursos crea Azure junto con la VM?

	+ Red virtual
	+ Maquina Virtual
	+ Dirección IP pública
	+ Grupo de seguridad de red
	+ Interfaz de red   
	+ Disco
	+ Clave SSH

2. ¿Brevemente describa para qué sirve cada recurso?

	+ Red virtual: Es la red privada de Azure quien permite la comunicacion entre los recursos, el internet y las redes on-premise de forma segura
	+ Maquina Virtual: Realiza una emulacion de un computador con sus respectivos componentes y recursos
	+ Dirección IP pública: El identificador unico para el ordenador que esta siendo simulado que permite las diferentes conexiones
	+ Grupo de seguridad de red: Tiene la tarea de filtrar el trafico de red bajo premisas de seguridad que permiten o restringen dicho trafico de entrada en la red, asi como tambien el trafico de salida
	+ Interfaz de red:	Realiza la funcion de conectar la maquina virtual con el internet y recursos del sistema
	+ Disco: Tiene el papel de almacenar los distintos datos del sistema	
	+ Clave SSH: es un protocolo de conexion cifrada que permite que las conexiones se realicen inicios de secion seguros en un contexto de conexiones que no son consideradas como seguras componiendose de una llave publicda y una llave privada, de manera que al conectarse el cliente SSH se asegura de que la llave privada sea la correcta para asi otorgar el acceso a la VM

3. ¿Al cerrar la conexión ssh con la VM, por qué se cae la aplicación que ejecutamos con el comando `npm FibonacciApp.js`? 

	Al utilizar el comando `npm FibonacciApp.js` , y cerrar la conexion SSH la aplicacion se detiene ya que el comando corre en dicha consola de la maquina virtual, por ende al teminar la conexion se termina por completo la conexion ya que el SSH hace un llamado a todos los procesos para que se cierren, puede suceder por inactividad, si existe algun error o simplemente por terminar la conexion, es por esto que se utiliza el comando `forever start FibonacciApp.js`, para que sea posible que siga en ejecucion el script sin importar si se cierra la conexion siempre y cuando la maquina virtual este encendida

¿Por qué debemos crear un *Inbound port rule* antes de acceder al servicio?

	Se debia agregar este *Inbound port rule* para permitir la conexion al servicio por el puerto 3000 como se realizo en este caso, ademas de poder elegir por que protocolo se recibira o denegara segun la eleccion que se realice es una forma de personalizar la conexion (el trafico de red) segun la necesidad

4. Adjunte tabla de tiempos e interprete por qué la función tarda tando tiempo.

	Se presentan dos tablas correspondientes al ejercicio de realizar las solicitudes al endpoint con los valores propuestos en el punto 7, obteniendo como resultado los siguientes datos, la primera tabla corresponde a los valores obtenidos con la opcion B1ls, observando que se tarda bastante por la capacidad de la maquina virtual en relacion a como se esta hayando el numero de Fibonacci, posteriormente al realizar el escalamiento vertical es posible observar como a pesar de contar con el mismo metodo para encontrar el numero de Fibonacci, pero contando con mayor capacidad en la maquina virtual los tiempos disminuyeron de manera optima, sin embargo continuaba tardando bastante, y apesar de representar una mejoria, no es significativa con respecto al la capacida de la maquina virtual inicialmente

	La manera en la cual se esta encontrando el numero de Fibonacci es de Complejidad O(n) razon por la cual al enviar solicitudes de numeros grandes tardara bastante al no contar con memorizacion y tener que calcular siempre desde cero las veces que lo necesita en las iteraciones para encontrar el resultado

### B1ls

| **Caso** | **Tiempo(s)**|
|---------|--------|
| 6 	  | 0.344 |
| 1000000 | 25.71 |
| 1010000 | 26.09 |
| 1020000 | 26.97 |
| 1030000 | 27.85 |
| 1040000 | 26.98 |
| 1050000 | 28.84 |
| 1060000 | 29.36 |
| 1070000 | 30.10 |
| 1080000 | 32.38 |
| 1090000 | 31.52 |

### B2ms

| **Caso**    | **Tiempo(s)** |
|---------|-----------|
| 6 	  | 0.344     |
| 1000000 | 23.19 	  |
| 1010000 | 23.55     |
| 1020000 | 24.27     |
| 1030000 | 26.03     |
| 1040000 | 25.29     |
| 1050000 | 26.74     |
| 1060000 | 26.67     |
| 1070000 | 27.17     |
| 1080000 | 27.92     |
| 1090000 | 28.27     |


5. Adjunte imágen del consumo de CPU de la VM e interprete por qué la función consume esa cantidad de CPU.

El alto consumo de la CPU se debe a que en la operacion de encontrar el numero de Fibonacci, la VM se encuentra realizando por un tiempo prolongado operaciones matematicas de numeros grandes esto en la capacidad de la maquina que contaba con bajas capacidades llegando a valores bastantes altos, de la misma forma aunque mejora con la configuracion de la VM en Bm2, baja el consumo de CPU pero sigue siendo alto para el momento en el cual se encuentra realizando dichas operaciones matematicas

### B1ls

![](images/solucion/consumo.PNG)

### B2ms

![](images/solucion/consumo2.PNG)

6. Adjunte la imagen del resumen de la ejecución de Postman. Interprete:
    * Tiempos de ejecución de cada petición.
    * Si hubo fallos documentelos y explique.
	
	En la ejecucion del Postman usando Newman, se obtuvieron los resultados que se presentan en las imagenes a cotinuacion:
	
### B1ls
	
![](images/solucion/proceso1.PNG)

![](images/solucion/pruebaJson.PNG)
	
	Primeramente con la capacidad de B1ls en la VM, obtenemos el resultado que se puede apreciar alli el cual representa un promedio de **26.5 segundos** por solicitud, para tener una duracion total en las 10 iteraciones de 4 minutos con 24.4 segundos, en esta coasion los 10 ejecutados compilaron y obtuvieron un resultado exitoso

### B2ms

![](images/solucion/pruebaJson2.PNG)
	
	Ahora bien, con la capacidad de B2ms en la VM, se obtuvo el resultado que se puede apreciar en el cual se obtiene un tiempo promedio por interacion de **23.9 segundos** , obteniendo como duracion total 4 minutos con 2.9 segundos en el procesamiento total de las 10 iteraciones
	
	Bajo estos resultados obtenidos es posible evidenciar una mejora con el escalamiento vertical, ya que la disminucion en el tiempo de ejecucion es importante, sin embargo no es tan siginificativa como se esperaba, pero es posible evidenciar una muestra de como el escalamiento vertical realiza una optimizacion de tiempos
	
	**Nota:** En el procesamiento de estas solicitudes, al realizar el ejercicio de concurrencia en algunos casos se presentaron errores aparatemente por dicha concurrencia, ya que la VM se quedaba sin recursos necesarios para responder la solicitud razon por la cual se presentaban errores por el alto consumo de la CPU el cual fue posible evidenciar mientras se hacian las solicitudes contando con un procentaje superior al 70 durante las solicitudes concurrentes.
	
7. ¿Cuál es la diferencia entre los tamaños `B2ms` y `B1ls` (no solo busque especificaciones de infraestructura)?

| Tamaño| Vcpu | RAM(GiB) | Discos de datos max | Gib de Almacenamiento temporal (SSD) | Base CPU Perf of VM |Rendimiento máximo de almacenamiento temporal y en caché: IOPS / MBps| Rendimiento máximo del disco sin almacenamiento en la caché: IOPS / MBps|   Costo    |
|-------|------|----------|---------------------|--------------------------------------|---------------------|---------------------------------------------------------------------|-------------------------------------------------------------------------|------------| 
| B1ls  |  1   |   0.5    | 		2           | 				4                      | 		5%			 |					200/10											   |								160/10	                             	 | 	$ 3.80 UD |
| B2ms  |  2   |    8     | 		3           | 				16                     | 		60%          |					2400/22.5$ 										   |  								1920/22.5								 |  $ 60.74 US|

8. ¿Aumentar el tamaño de la VM es una buena solución en este escenario?, ¿Qué pasa con la FibonacciApp cuando cambiamos el tamaño de la VM?

	Es una buena solución para reducir la carga del servidor y obtener tiempos de respuesta más rápidos porque el nuevo tamaño permite completar más operaciones en menos tiempo. Sin embargo, el sistema tampoco admite que muchos usuarios envíen solicitudes al mismo tiempo sin afectar el tiempo o la cantidad de respuesta.

	La aplicacion de Fibonacci se detiene a pesar de haber puesto forever ya que la VM cambio de tamaño y se debe reiniciar, razon por la cual el servicio se detiene

9. ¿Qué pasa con la infraestructura cuando cambia el tamaño de la VM? ¿Qué efectos negativos implica?

	La Maquina Virtual debe reiniciarse para poder cambiar su tamaño, lo que representa negativamente que el servicio no esté disponible durante el escalamiento vertical lo que podria representar sobrecostos y falta de disponibilidad

10. ¿Hubo mejora en el consumo de CPU o en los tiempos de respuesta? Si/No ¿Por qué?

	Se presento una mejora tanto en el consumo de CPU como tambien en los tiempos de respuesta para la solicitudes, sin embargo a pesar de mejorar con el escalamiento vertical, no representa una mejora realmente significativa ya que aun tarda bastante en las solicitudes y el consumo de CPU continua siendo alto para la operacion de la Aplicacion de Fibonacci

11. Aumente la cantidad de ejecuciones paralelas del comando de postman a `4`. ¿El comportamiento del sistema es porcentualmente mejor?

![](images/solucion/con4.PNG)

Al aumental la cantidad de solicitudes paralelas a 4m el comportamiento del sistema aumenta el tiempo de respuesta siginificativamente de igual manera los recusos no son sufucientes por lo que lanza bastantes errores al no poder responder tantas solicitudes al tiempo, como es posible evidenciar en la anterior imagen en 10 iteraciones fallo en 4, ademas de contar con un tiempo total de 8 minutos con 31.5 segundos lo que representa el doble que se habia tardado solo con 2 solicitudes paralelas,  contando con un tiempo promedio de 1 minuto con 7.7 segundos por solicitud, sin embargo con respecto al uso de la CPU el porcentaje de uso se optimiza al disminuir en las solicitudes realizadas

### Parte 2 - Escalabilidad horizontal

#### Crear el Balanceador de Carga

Antes de continuar puede eliminar el grupo de recursos anterior para evitar gastos adicionales y realizar la actividad en un grupo de recursos totalmente limpio.

1. El Balanceador de Carga es un recurso fundamental para habilitar la escalabilidad horizontal de nuestro sistema, por eso en este paso cree un balanceador de carga dentro de Azure tal cual como se muestra en la imágen adjunta.

![](images/solucion/PARTE2.PNG)

![](images/solucion/parte21.PNG)

![](images/solucion/balancer.PNG)

![](images/solucion/3compus.PNG)

![](images/part2/part2-lb-create.png)

2. A continuación cree un *Backend Pool*, guiese con la siguiente imágen.

![](images/solucion/pool.PNG)

![](images/part2/part2-lb-bp-create.png)

3. A continuación cree un *Health Probe*, guiese con la siguiente imágen.

![](images/solucion/health.PNG)

![](images/part2/part2-lb-hp-create.png)

4. A continuación cree un *Load Balancing Rule*, guiese con la siguiente imágen.

![](images/part2/part2-lb-lbr-create.png)

5. Cree una *Virtual Network* dentro del grupo de recursos, guiese con la siguiente imágen.


![](images/solucion/virtualn1.PNG)

![](images/solucion/virtualn2.PNG)

![](images/part2/part2-vn-create.png)


#### Crear las maquinas virtuales (Nodos)

Ahora vamos a crear 3 VMs (VM1, VM2 y VM3) con direcciones IP públicas standar en 3 diferentes zonas de disponibilidad. Después las agregaremos al balanceador de carga.

1. En la configuración básica de la VM guíese por la siguiente imágen. Es importante que se fije en la "Avaiability Zone", donde la VM1 será 1, la VM2 será 2 y la VM3 será 3.

### VM1

![](images/solucion/vm1load.PNG)

![](images/solucion/vm1.PNG)

### VM2

![](images/solucion/vm2.PNG)

### VM3

![](images/solucion/vm3.PNG)

![](images/solucion/vm3load.PNG)

![](images/part2/part2-vm-create1.png)

2. En la configuración de networking, verifique que se ha seleccionado la *Virtual Network*  y la *Subnet* creadas anteriormente. Adicionalmente asigne una IP pública y no olvide habilitar la redundancia de zona.

### VM1

![](images/solucion/vm1net.PNG)

### VM2

![](images/solucion/vm1net.PNG)

### VM3

![](images/solucion/vm3net.PNG)

![](images/part2/part2-vm-create2.png)

3. Para el Network Security Group seleccione "avanzado" y realice la siguiente configuración. No olvide crear un *Inbound Rule*, en el cual habilite el tráfico por el puerto 3000. Cuando cree la VM2 y la VM3, no necesita volver a crear el *Network Security Group*, sino que puede seleccionar el anteriormente creado.

![](images/part2/part2-vm-create3.png)

4. Ahora asignaremos esta VM a nuestro balanceador de carga, para ello siga la configuración de la siguiente imágen.

![](images/part2/part2-vm-create4.png)

5. Finalmente debemos instalar la aplicación de Fibonacci en la VM. para ello puede ejecutar el conjunto de los siguientes comandos, cambiando el nombre de la VM por el correcto

```
git clone https://github.com/daprieto1/ARSW_LOAD-BALANCING_AZURE.git

curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
source /home/vm1/.bashrc
nvm install node

cd ARSW_LOAD-BALANCING_AZURE/FibonacciApp
npm install

npm install forever -g
forever start FibonacciApp.js
```

![](images/solucion/vm1Terminada.PNG)

![](images/solucion/vm2Terminada.PNG)

![](images/solucion/vm3Terminada.PNG)

![](images/solucion/3maquinas.PNG)

Realice este proceso para las 3 VMs, por ahora lo haremos a mano una por una, sin embargo es importante que usted sepa que existen herramientas para aumatizar este proceso, entre ellas encontramos Azure Resource Manager, OsDisk Images, Terraform con Vagrant y Paker, Puppet, Ansible entre otras.

#### Probar el resultado final de nuestra infraestructura

1. Porsupuesto el endpoint de acceso a nuestro sistema será la IP pública del balanceador de carga, primero verifiquemos que los servicios básicos están funcionando, consuma los siguientes recursos:

```
http://52.155.223.248/
```

![](images/solucion/loadSirveSolo.PNG)

```
http://52.155.223.248/fibonacci/1
```
![](images/solucion/sirvioLoad.PNG)


2. Realice las pruebas de carga con `newman` que se realizaron en la parte 1 y haga un informe comparativo donde contraste: tiempos de respuesta, cantidad de peticiones respondidas con éxito, costos de las 2 infraestrucruras, es decir, la que desarrollamos con balanceo de carga horizontal y la que se hizo con una maquina virtual escalada.

![](images/solucion/loadCompleto1.PNG)

![](images/solucion/completoUltimoUno.PNG)

![](images/solucion/completoUltimoDos.PNG)

![](images/solucion/los4Simultaneo.PNG)

![](images/solucion/los4sa.PNG)

![](images/solucion/parte2soloUno.PNG)

![](images/solucion/parte2soloDos.PNG)

3. Agregue una 4 maquina virtual y realice las pruebas de newman, pero esta vez no lance 2 peticiones en paralelo, sino que incrementelo a 4. Haga un informe donde presente el comportamiento de la CPU de las 4 VM y explique porque la tasa de éxito de las peticiones aumento con este estilo de escalabilidad.

![](images/solucion/maquinas.PNG)

```
newman run ARSW_LOAD-BALANCING_AZURE.postman_collection.json -e [ARSW_LOAD-BALANCING_AZURE].postman_environment.json -n 10 &
newman run ARSW_LOAD-BALANCING_AZURE.postman_collection.json -e [ARSW_LOAD-BALANCING_AZURE].postman_environment.json -n 10 &
newman run ARSW_LOAD-BALANCING_AZURE.postman_collection.json -e [ARSW_LOAD-BALANCING_AZURE].postman_environment.json -n 10 &
newman run ARSW_LOAD-BALANCING_AZURE.postman_collection.json -e [ARSW_LOAD-BALANCING_AZURE].postman_environment.json -n 10
```

**Preguntas**

* ¿Cuáles son los tipos de balanceadores de carga en Azure y en qué se diferencian?  

	**Equilibrador de carga público:** Proporciona conexiones de salida para máquinas virtuales dentro de la red virtual, dichas conexiones se establecen convirtiendo sus direcciones IP privadas en direcciones IP públicas.
	
	**Equilibrador de carga privado:** Se utiliza para equilibrar el trafico dentro de la red virtual tambien cuando solo se necesitan direcciones IP privadas en el front-end.

* ¿Qué es SKU, qué tipos hay y en qué se diferencian?
		
	Azure Container Registry está disponible en una variedad de niveles de servicio (también conocidos como SKU). SKU (Stock Keeping Unit) es un código único asignado a un servicio o producto dentro de azure que representa la capacidad de adquirir existencias.
		
	**Basico:** Un punto de entrada con bajos costos para que los desarrolladores aprendan sobre Azure Container Registry. Los registros básicos tienen las mismas capacidades de programación que los registros estándar y premium.
	**Estandar:** Los registros estándar tienen las mismas capacidades que los registros básicos, pero con un mayor rendimiento de imágenes y almacenamiento incluido. Los registros estándar deben satisfacer las necesidades de la gran mayoría de estudios de producción.
	**Premium:** Los registros premium proporcionan la mayor cantidad de almacenamiento incluido y operaciones simultáneas, lo que permite producciones a gran escala. Además de un mayor rendimiento de imágenes, también incluye características como la replicación geoespacial para administrar un solo registro en múltiples regiones y confianza en el contenido para el desarrollo de etiquetas de imagen, enlace privado con puntos finales privados para restringir el acceso al registro.
		
	Azure presenta la siguiente de los diferentes Sku:
		
![](images/solucion/sku.PNG)
	
* ¿Por qué el balanceador de carga necesita una IP pública?
	
	Para ser accesible desde Internet, se debe asociar a una dirección IP pública con una instancia de Azure Load Balancer la cual sirve como una dirección IP de carga equilibrada

* ¿Cuál es el propósito del *Backend Pool*?
	
	Definir cómo evaluar diferentes Back-Ends utilizando sondas de estado, dichos Back-Ends se despliegan dentro de una misma region o en diferente regiones, los cuales pieden estar en modo de despliegue Activo/Activo o en configuracion Activo/Pasivo, Backend Pool se encarga tambien de lograr el equilibrio de carga entre ellos, es decir, definir el conjunto de recursos que proporcionarán tráfico para una regla de equilibrio de carga específica.
	
* ¿Cuál es el propósito del *Health Probe*?

	Cuando se usan las reglas de equilibro de carga con Azure Load Balancer, es de gran importancia especificar las sonas de estado para permitir que Load balancer detecte el estado del punto de conexion de Backend, estas sondas pueden admitir distintos protocolos segun el SKU TCP, HTTP o HTTPS, y se debe configurar junto con sus respectivas respuestas de forma que determinen que instancias del grupo de backend recibiran los nuevos flojos, tambien puede ser utilizada para encontrar fallas en la aplicacion de algun endpoint del backend. Ademas es posible generar una respuesta personalizada a una sonda de estado usandola para el controll de flujo y de esta forma administrar la carga o el tiempo de inactividad de manera planeada.

* ¿Cuál es el propósito de la *Load Balancing Rule*? ¿Qué tipos de sesión persistente existen, por qué esto es importante y cómo puede afectar la escalabilidad del sistema?.

	Define la configuración de IP de interfaz para el tráfico entrante y el grupo de back-end para recibir el tráfico, junto con el puerto de origen y destino requerido. Para Azure Load Balancer, el modo de distribución predeterminado es un hash de tuplas con cinco elementos. Los siguientes son los contenidos de la tupla:

		+ IP de origen
		+ Puerto de origen
		+ IP de destino
		+ Puerto de destino
		+ Tipo de protocolo

	Proporciona equilibrio de carga entrante y la regla de salida controla el NAT saliente proporcionado para la VM. Este inicio rápido utiliza dos grupos de backend separados, uno para entrada y otro para salida, para ilustrar la capacidad y permitir flexibilidad para este escenario. Con las reglas de salida establecidas, tiene un poder declarativo completo sobre la conectividad a Internet. Las reglas de salida le permiten escalar y ajustar esta capacidad a sus necesidades específicas.

	El hash se utiliza para distribuir el tráfico entre los servidores disponibles. El algoritmo solo proporciona adhesión dentro de una única sesión de transporte. Siguiendo el punto de conexión con equilibrio de carga, todos los paquetes de una misma sesión se envían a la misma dirección IP del centro de datos. Cuando un cliente inicia una nueva sesión desde la misma dirección IP, el puerto de origen cambia, lo que hace que el tráfico fluya a otro punto de conexión en el centro de datos.

	Para asignar tráfico a los servidores disponibles, el modo utiliza un hash de tupla de dos elementos (IP de origen e IP de destino) o tres elementos (IP de origen, IP de destino y tipo de protocolo). Las conexiones que han comenzado desde el mismo equipo del cliente van al mismo punto de conexión en el centro de datos gracias al uso de la similitud de la IP de origen.

* ¿Qué es una *Virtual Network*?  

	Azure Virtual Network proporciona un entorno seguro y aislado en el que ejecutar aplicaciones y máquinas virtuales. Utilice sus propias direcciones IP privadas y defina subredes, directivas de control de acceso, etc. Para usar Azure como su propio centro de datos, cree una red virtual.

* ¿Qué es una *Subnet*?

	Las redes virtuales se pueden subdividir en subredes para hacer coincidir la implementación de la nube con la implementación local. Es posible transferir una máquina a otra subred dentro de una red virtual, pero no a la subred de otra red virtual. Interfaz a Internet: una máquina virtual está conectada a una subred a través de una tarjeta de interfaz de red (NIC).

* ¿Para qué sirven los *address space* y *address range*?

	**Address space:** al establecer una red virtual, es esencial designar un espacio de direcciones IP personalizado que utilice direcciones públicas y privadas (RFC 1918). Azure asigna una dirección IP privada a los recursos de la red virtual según el espacio de direcciones que especifique. Por ejemplo, si una máquina virtual se implementa en una red virtual con espacio de direcciones, 10.0

	**Address range** el rango debe estar dentro del espacio de direcciones en el que ingresó a la red virtual. El rango más pequeño que se puede definir es 29, que proporciona ocho direcciones IP para la subred. Azure reserva la primera y la última dirección en cada subred para el cumplimiento del protocolo.

* ¿Qué son las *Availability Zone* y por qué seleccionamos 3 diferentes zonas?. 

	Las zonas de disponibilidad de Azure son centros de datos que están física y lógicamente separados, cada uno con su propia fuente de alimentación, red y sistema de refrigeración. Cuando están conectados a una red con latencias extremadamente altas, se convierten en un bloque de creación para entregar aplicaciones de alta disponibilidad.

	En el ejercicio realizado seleccionamos 3 zonas de disponibilidad diferentes para curarse en salud con respecto a las fallas que se pueden presentar en cada una de las VM y de esta fprma garantizar resiliencia protegiendo las aplicaciones y los datos del centro de datos

*¿Qué significa que una IP sea *zone-redundant*?

	Significa que la IP es replicada por la plataforma automaticamente en todas las zonas

* ¿Cuál es el propósito del *Network Security Group*?

	Es una línea de defensa en Azure para la protección de nuestros recursos, usando un firewall de capa 4 que filtrará el tráfico en base a direcciones IP de origen y destino, puertos de origen y destino y protocolo (TCP / UDP), pero no por contenido.

* Informe de newman 1 (Punto 2)

En esta segunda parte se llevo a cabo el ejercio usando los Json atravez de newman, tal como se realizo en la primera parte, de esta forma verificando el load balancer implementado observando su respectivo funcionamiento con las 3 maquinas creadas.

Para estas pruebas con newman es importante tener en claro la siguiente informacion con respecto a las Ip's de los componentes:

vm1 --> 52.155.164.189 

vm2 --> 52.155.165.245

vm3 --> 52.156.204.48

vm4 --> 20.82.224.241

SCALABILITY_LAB_LOAD_BALANCER_IP --> 20.82.249.93


Primeramente se realizo una prueba con el Json respectivo con newman usando el siguiente comando:

```
newman run ARSW_LOAD-BALANCING_AZURE.postman_collection.json -e [ARSW_LOAD-BALANCING_AZURE].postman_environment.json -n 10
```

Esto con el fin de verificar que estaba funcionado de manera correcta, obteniendo como resultado para un total de 10 iteraciones un tiempo de 4 minutos con 55.5 segundos y un tiempo promedio de 29 segundos por solicitud

![](images/solucion/loadCompletoUno.PNG)

Ahora bien se realizo la solicitud de manera paralela obteniendo como resultado una mejora de tiempo con respecto al anterior ejercicio, obteniendo un tiempo total de 3 minutos con 45.1 segundos y un tiempo promedio de 22.4 segundos por solicitud

![](images/solucion/completoUltimoUno.PNG)

Posteriormente se realizo la prueba usando el siguiente comando, obteniendo como resultado una totalidad el 100% exitoso, sin presentar errores y con un tiempo promedio y similar a la primera prueba

```
newman run ARSW_LOAD-BALANCING_AZURE.postman_collection.json -e [ARSW_LOAD-BALANCING_AZURE].postman_environment.json -n 10 & newman run ARSW_LOAD-BALANCING_AZURE.postman_collection.json -e [ARSW_LOAD-BALANCING_AZURE].postman_environment.json -n 10
```

![](images/solucion/completoUltimoDos.PNG)

Posteriormente se realizo una prueba de 4 solicitudes concurrentes paralelas en cada una de las maquinas propuestas obteniendo como resultado que en de las VM utilizadas se encontro un error, y 9 exitos de las iteraciones y en una 4 VM se encontro con dos errores, y 8 exitos, dos de estas maquinas tuvieron un tiempo aproximado de 5 minutos y medio y las otras dos maquinas un tiempo estimado de 4 minutos y medio como es posible evidenciar en la siguiente imagen

![](images/solucion/los4Simultaneo.PNG)

Contando con el siguiente rendimiento de CPU en cada maquina respectiva:

### VM1

![](images/solucion/consumoParalelovm4.JPG) 

### VM2

![](images/solucion/consumoParalelovm2.JPG)

### VM3

![](images/solucion/consumoParalelovm1.JPG)
 
### VM4

![](images/solucion/consumoParalelovm2.JPG)


* Presente el Diagrama de Despliegue de la solución.

![](images/solucion/solucion.PNG)

 
# Control de versiones

por: [Santiago Buitrago](https://github.com/DonSantiagoS) 

Version: 1.0
Fecha: 20 de Abril 2021

## Autor

* **Santiago Buitrago** - *Laboratorio N°10* - [DonSantiagoS](https://github.com/DonSantiagoS)

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## Licencia 

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Agradecimientos

* Persistencia en lograr el objetivo