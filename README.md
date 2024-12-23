# Optimizar-el-rendimiento-de-un-script-de-python-lento-

 En una simulación de un caso real,  nos encargan la tarea de realizar copias de seguridad de los datos del NAS de producción (montado en/data/prod en el servidor) en el NAS de copia de seguridad (montado en/data/prod_backup en el servidor). Un antiguo miembro del equipo desarrolló un script en Python (ruta completa/scripts/dailysync.py) que hacía copias de seguridad de los datos a diario. Pero últimamente se han generado muchos datos y el script no está alcanzando la velocidad necesaria. Como resultado, el proceso de copia de seguridad tarda ahora más de 20 horas en terminar, lo que no es nada 
 eficiente para una copia de seguridad diaria.

 # Herramientas que usaremos 


