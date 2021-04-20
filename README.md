## Escuela Colombiana de Ingeniería

## Arquitecturas de Software - ARSW

### LABORATORIO N°10

## Escalamiento en Azure con Maquinas Virtuales, Sacale Sets y Service Plans

### Dependencias
* Cree una cuenta gratuita dentro de Azure. Para hacerlo puede guiarse de esta [documentación](https://azure.microsoft.com/en-us/free/search/?&ef_id=Cj0KCQiA2ITuBRDkARIsAMK9Q7MuvuTqIfK15LWfaM7bLL_QsBbC5XhJJezUbcfx-qAnfPjH568chTMaAkAsEALw_wcB:G:s&OCID=AID2000068_SEM_alOkB9ZE&MarinID=alOkB9ZE_368060503322_%2Bazure_b_c__79187603991_kwd-23159435208&lnkd=Google_Azure_Brand&dclid=CjgKEAiA2ITuBRDchty8lqPlzS4SJAC3x4k1mAxU7XNhWdOSESfffUnMNjLWcAIuikQnj3C4U8xRG_D_BwE). Al hacerlo usted contará con $200 USD para gastar durante 1 mes.

De manera gratuita con Imagine Azure se creo la cuenta estudiantil gratuita para llevar a cabo la solucion del laboratorio

![](images/solucion/nuevaCuenta.PNG)

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

![Imágen 1](images/part1/part1-vm-basic-config.png)

2. Para conectarse a la VM use el siguiente comando, donde las `x` las debe remplazar por la IP de su propia VM.

    `ssh scalability_lab@xxx.xxx.xxx.xxx`

3. Instale node, para ello siga la sección *Installing Node.js and npm using NVM* que encontrará en este [enlace](https://linuxize.com/post/how-to-install-node-js-on-ubuntu-18.04/).
	
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

![](images/solucion/prueba1.PNG)

![](images/solucion/add.PNG)

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
	

### B2m

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

### B1ls

![](images/solucion/consumo.PNG)

### B2m

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

### B2m

	![](images/solucion/pruebaJson2.PNG)

10. La cantidad de CPU consumida es bastante grande y un conjunto considerable de peticiones concurrentes pueden hacer fallar nuestro servicio. Para solucionarlo usaremos una estrategia de Escalamiento Vertical. En Azure diríjase a la sección *size* y a continuación seleccione el tamaño `B2ms`.

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

+ Al utilizar el comando `npm FibonacciApp.js` , y cerrar la conexion SSH la aplicacion se detiene ya que el comando corre en dicha consola de la maquina virtual, por ende al teminar la conexion se termina por completo la conexion ya que el SSH hace un llamado a todos los procesos para que se cierren, puede suceder por inactividad, si existe algun error o simplemente por terminar la conexion, es por esto que se utiliza el comando `forever start FibonacciApp.js`, para que sea posible que siga en ejecucion el script sin importar si se cierra la conexion siempre y cuando la maquina virtual este encendida

¿Por qué debemos crear un *Inbound port rule* antes de acceder al servicio?

+ Se debia agregar este *Inbound port rule* para permitir la conexion al servicio por el puerto 3000 como se realizo en este caso, ademas de poder elegir por que protocolo se recibira o denegara segun la eleccion que se realice es una forma de personalizar la conexion (el trafico de red) segun la necesidad

4. Adjunte tabla de tiempos e interprete por qué la función tarda tando tiempo.

### B1ls

| **Caso** | **Tiempo(s)**|
|---------|--------|
| 6 | 0.344 |
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

### B2m

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
6. Adjunte la imagen del resumen de la ejecución de Postman. Interprete:
    * Tiempos de ejecución de cada petición.
    * Si hubo fallos documentelos y explique.
7. ¿Cuál es la diferencia entre los tamaños `B2ms` y `B1ls` (no solo busque especificaciones de infraestructura)?
8. ¿Aumentar el tamaño de la VM es una buena solución en este escenario?, ¿Qué pasa con la FibonacciApp cuando cambiamos el tamaño de la VM?
9. ¿Qué pasa con la infraestructura cuando cambia el tamaño de la VM? ¿Qué efectos negativos implica?
10. ¿Hubo mejora en el consumo de CPU o en los tiempos de respuesta? Si/No ¿Por qué?
11. Aumente la cantidad de ejecuciones paralelas del comando de postman a `4`. ¿El comportamiento del sistema es porcentualmente mejor?

### Parte 2 - Escalabilidad horizontal

#### Crear el Balanceador de Carga

Antes de continuar puede eliminar el grupo de recursos anterior para evitar gastos adicionales y realizar la actividad en un grupo de recursos totalmente limpio.

1. El Balanceador de Carga es un recurso fundamental para habilitar la escalabilidad horizontal de nuestro sistema, por eso en este paso cree un balanceador de carga dentro de Azure tal cual como se muestra en la imágen adjunta.

![](images/part2/part2-lb-create.png)

2. A continuación cree un *Backend Pool*, guiese con la siguiente imágen.

![](images/part2/part2-lb-bp-create.png)

3. A continuación cree un *Health Probe*, guiese con la siguiente imágen.

![](images/part2/part2-lb-hp-create.png)

4. A continuación cree un *Load Balancing Rule*, guiese con la siguiente imágen.

![](images/part2/part2-lb-lbr-create.png)

5. Cree una *Virtual Network* dentro del grupo de recursos, guiese con la siguiente imágen.

![](images/part2/part2-vn-create.png)

#### Crear las maquinas virtuales (Nodos)

Ahora vamos a crear 3 VMs (VM1, VM2 y VM3) con direcciones IP públicas standar en 3 diferentes zonas de disponibilidad. Después las agregaremos al balanceador de carga.

1. En la configuración básica de la VM guíese por la siguiente imágen. Es importante que se fije en la "Avaiability Zone", donde la VM1 será 1, la VM2 será 2 y la VM3 será 3.

![](images/part2/part2-vm-create1.png)

2. En la configuración de networking, verifique que se ha seleccionado la *Virtual Network*  y la *Subnet* creadas anteriormente. Adicionalmente asigne una IP pública y no olvide habilitar la redundancia de zona.

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

Realice este proceso para las 3 VMs, por ahora lo haremos a mano una por una, sin embargo es importante que usted sepa que existen herramientas para aumatizar este proceso, entre ellas encontramos Azure Resource Manager, OsDisk Images, Terraform con Vagrant y Paker, Puppet, Ansible entre otras.

#### Probar el resultado final de nuestra infraestructura

1. Porsupuesto el endpoint de acceso a nuestro sistema será la IP pública del balanceador de carga, primero verifiquemos que los servicios básicos están funcionando, consuma los siguientes recursos:

```
http://52.155.223.248/
http://52.155.223.248/fibonacci/1
```

2. Realice las pruebas de carga con `newman` que se realizaron en la parte 1 y haga un informe comparativo donde contraste: tiempos de respuesta, cantidad de peticiones respondidas con éxito, costos de las 2 infraestrucruras, es decir, la que desarrollamos con balanceo de carga horizontal y la que se hizo con una maquina virtual escalada.

3. Agregue una 4 maquina virtual y realice las pruebas de newman, pero esta vez no lance 2 peticiones en paralelo, sino que incrementelo a 4. Haga un informe donde presente el comportamiento de la CPU de las 4 VM y explique porque la tasa de éxito de las peticiones aumento con este estilo de escalabilidad.

```
newman run ARSW_LOAD-BALANCING_AZURE.postman_collection.json -e [ARSW_LOAD-BALANCING_AZURE].postman_environment.json -n 10 &
newman run ARSW_LOAD-BALANCING_AZURE.postman_collection.json -e [ARSW_LOAD-BALANCING_AZURE].postman_environment.json -n 10 &
newman run ARSW_LOAD-BALANCING_AZURE.postman_collection.json -e [ARSW_LOAD-BALANCING_AZURE].postman_environment.json -n 10 &
newman run ARSW_LOAD-BALANCING_AZURE.postman_collection.json -e [ARSW_LOAD-BALANCING_AZURE].postman_environment.json -n 10
```

**Preguntas**

* ¿Cuáles son los tipos de balanceadores de carga en Azure y en qué se diferencian?, ¿Qué es SKU, qué tipos hay y en qué se diferencian?, ¿Por qué el balanceador de carga necesita una IP pública?
* ¿Cuál es el propósito del *Backend Pool*?
* ¿Cuál es el propósito del *Health Probe*?
* ¿Cuál es el propósito de la *Load Balancing Rule*? ¿Qué tipos de sesión persistente existen, por qué esto es importante y cómo puede afectar la escalabilidad del sistema?.
* ¿Qué es una *Virtual Network*? ¿Qué es una *Subnet*? ¿Para qué sirven los *address space* y *address range*?
* ¿Qué son las *Availability Zone* y por qué seleccionamos 3 diferentes zonas?. ¿Qué significa que una IP sea *zone-redundant*?
* ¿Cuál es el propósito del *Network Security Group*?
* Informe de newman 1 (Punto 2)
* Presente el Diagrama de Despliegue de la solución.




