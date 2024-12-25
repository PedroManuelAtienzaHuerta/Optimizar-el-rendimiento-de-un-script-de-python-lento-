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
Una posible salida mostraría:

```1.2```

Esto muestra que la utilización de la CPU es baja. Tu ordenador tiene una **CPU con múltiples núcleos**; esto significa que un hilo de CPU/núcleo virtual completamente cargado equivale al 3.1% de la carga total. 


Por lo tanto, sólo utiliza un núcleo de la CPU a pesar de tener múltiples núcleos, lo que significa que la tarea está limitada por la CPU.



Ahora, usando **psutil.disk_io_counters()** y **psutil.net_io_counters()** obtendrás byte leído y byte escrito para E/S de disco y byte recibido y byte enviado para el ancho de banda de E/S de red.

Para comprobar la E/S de disco, puedes usar el siguiente comando:
```
psutil.disk_io_counters()
```
Salida:

```python
sdiskio(read_count=56089, write_count=113305, read_bytes=5736552448, write_bytes=3971753984, read_time=287876, write_time=905894, 
read_merged_count=19901, write_merged_count=546700, busy_time=193358)
```

Para comprobar el ancho de banda de E/S de red:
```
psutil.net_io_counters()
```
Salida:
```python
snetio(bytes_sent=153564, bytes_recv=32133915, packets_sent=2182, 
packets_recv=2738, errin=0, errout=0, dropin=0, dropout=0)
```

Salga del shell de Python usando **exit()**.

Después de comprobar la E/S de disco y el ancho de banda de red, has observado la **cantidad de bytes leídos y escritos** para la E/S de disco y bytes recibidos y enviados para el ancho de banda de E/S de red.

El script dailysync.py original contenía el siguiente código:
```python
#!/usr/bin/env python3

import subprocess
src = "home/student/data/prod" # ruta de origen de los datos 
dest = "home/student/data/prod_backup" # ruta de destino de los datos 
subprocess.call(["rsync", "-arq", src, dest]) #Llamamos al método call del módulo subprocess para usar el comando rsync para sincronizar los datos.
```

Usando el script anterior, puedes sincronizar tus datos recursivamente desde la ruta de origen a la ruta de destino.

Al ejecutar este script con grandes cantidades de datos tarda más de 20 minutos en realizar la sincronización.

# Editar el archivo lento 

Abre en el editor **nano** el script Python **dailysync.py** que hay que modificar.
Utilizaremos núcleos de CPU ociosos para la copia de seguridad.
```
nano ~/scripts/dailysync.py
```
Aplicaremos el **multiprocesamiento** y los métodos de módulo de **subprocess** para sincronizar los datos de **/data/prod** a la carpeta **/data/prod_backup**.

También usaremos el método **os.walk()** para generar los nombres de archivo en un árbol de directorios recorriendo el árbol de arriba hacia abajo o de abajo hacia arriba. Esto se utiliza para **recorrer el sistema de archivos** en Python.


El código completo de **dailysync ya optimizado en el editor nano** sería el siguiente:
```python
#!/usr/bin/env python3
import os
import subprocess
from multiprocessing import Pool

def sync_files(source_file):
    source_path = os.path.join('/home/student/data/prod', source_file)
    dest_path = os.path.join('/home/student/data/prod_backup', source_file)
    try:
        subprocess.run(['rsync', '-av', source_path, dest_path], check=True)
        print(f'Sincronizado: {source_file}')
    except Subprocess. CalledProcessError as e:
        print(f'Error al sincronizar {source_file}: {e}')

def main():
    source_dir = '/home/student/data/prod'
    files_to_sync = []
    for root, _, files in os.walk(source_dir):
        for file in files:
            files_to_sync.append(os.path.relpath(os.path.join(root, file), source_dir))
    with Pool() as pool:
      pool.map(sync_files, files_to_sync)

if __name__ = '__main__':
    main()
```


Una vez que hayas terminado de escribir el script Python, **guarda el archivo** pulsando Ctrl-o, la tecla Enter y Ctrl-x.

Ahora, conceda el **permiso ejecutable** al script Python dailysync.py para ejecutar este archivo.
```
sudo chmod  +x ~/scripts/dailysync.py
```
**Ejecuta el script** Python dailysync.py:
```
./scripts/dailysync.py
```
El resultado sería la sincronización de una gran cantidad de información en muy poco tiempo, por lo que **solucionamos eficazmente el problema** que teníamos con el script lento.

























