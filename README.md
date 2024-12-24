# Optimizar-el-rendimiento-de-un-script-de-python-lento-

 En una simulación de un caso real,  nos encargan la tarea de realizar copias de seguridad de los datos del **NAS de producción** (/data/prod en el servidor) en el **NAS de copia de seguridad** (/data/prod_backup en el servidor). 

 
 Un antiguo miembro del equipo desarrolló un script en Python llamado **dailysync.py**(~/scripts/dailysync.py) que hacía copias de seguridad de los datos a diario. 

 
 Pero últimamente se han generado muchos datos y **el script no está alcanzando la velocidad necesaria.**
 
 
 Como resultado, el proceso de copia de seguridad tarda ahora más de 20 horas en terminar, lo que no es nada 
 eficiente para una copia de seguridad diaria.

 # Herramientas que usaremos 

Lo primero que haremos es usar la herramienta **psutil**(process and system utilities).


Es una biblioteca multiplataforma para recuperar información sobre los procesos en ejecución y la utilización del sistema (CPU, memoria, discos, red, sensores) en Python.

Instala la librería python psutil usando **pip3**.

Para ello, introduzca el siguiente comando en el terminal:

```
pip3 install psutil
```

Ahora abre el intérprete **python3** introduciendo el siguiente comando:
```
python3
```
**Importar psutil**, módulo de python3 para comprobar el uso de la CPU, así como la E / S y ancho de banda de red:
```python 
import psutil
psutil.cpu_percent()
```
La salida muestra:

```1.2```

Esto muestra que la utilización de la CPU es baja. Tu ordenador tiene una **CPU con múltiples núcleos**; esto significa que un hilo de CPU/núcleo virtual completamente cargado equivale al 3.1% de la carga total. 


Por lo tanto, sólo utiliza un núcleo de la CPU a pesar de tener múltiples núcleos, lo que significa que la tarea está limitada por la CPU.



Ahora, usando **psutil.disk_io_counters()** y **psutil.net_io_counters()** obtendrás byte leído y byte escrito para E/S de disco y byte recibido y byte enviado para el ancho de banda de E/S de red.

Para comprobar la E/S de disco, puedes usar el siguiente comando:
```
psutil.disk_io_counters()
```










