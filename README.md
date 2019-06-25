# Call--Center

En el siguiente proyecto se muestra una implementación de un Call Center. Tiene como objetivo asignar las llamadas telefónicas provenientes de clientes a los distintos tipos de empleados. Para asignar estas llamadas, se utiliza el siguiente orden de asignación:
1.	Operador
2.	Supervisor
3.	Director
Diseño
Durante la implementación se utilizaron Threads, tanto para operadores, supervisores y directores, como para los clientes y el mismo dispatcher encargado de asignar las llamadas. Para manejar la concurrencia se utilizan dos semáforos: uno es un mutex para garantizar que no habrá más de un proceso modificando ningún objeto o recurso. El segundo semáforo se utiliza para manejar la cantidad de llamadas concurrentes. Se utilizan 3 empleados: una para Operadores, otra para Supervisores y otra para Directores. Esto es útil a la hora de asignar las llamadas, ya que facilita manejar las prioridades. También fue necesario tener una lista con las llamadas entrantes, guardando los id de los clientes creados. Este tipo de lista utilizada, garantiza el orden de inserción de datos. Esta fue una solución pensada para resolver uno de los puntos extras del ejercicio.
Operador, Director, Supervisor
Cada clase es un Thread que implementan un runneable, el cual se despierta cuando hay una llamada para asignarle. Toma la llamada, espera un tiempo random entre 5 y 10 segundos, y finaliza la llamada. Tiene la responsabilidad de liberar el recurso que le fue asignado al tomar la llamada. 
Dispatcher
Es un Thread encargado de asignar las llamadas telefónicas entrantes mediante un orden de prioridad, según lo explicado anteriormente. Este maneja un mutex para bloquear la modificación y asignación de llamadas.
 
Cliente
Es un Thread que representa el cliente que intenta llamar al call center. Este proceso tiene un loop infinito que se "anota" para llamar, y espera un tiempo, con el fin de simular otra llamada.
Llamada
Esta clase es la que crea el Dispatcher, creando los empleados disponibles para tomar llamadas y crea un set de clientes que van a generar un flujo de llamados continuo. De las estructuras que maneja este objeto, tenemos un semáforo para administrar la cantidad de llamadas concurrentes y una lista de llamadas entrantes que están pendientes para ser atendidas.
Iniciador
Este nos ayuda a arrancar el servicio. 
Extra/Plus
•	Dar alguna solución sobre qué pasa con una llamada cuando no hay ningún empleado libre. Se utiliza una lista de llamadas entrantes, para mantener el orden de llegada. Cuando no hay empleados libres, la llamada entrante se mantiene en la lista. Esto permite que la siguiente llamada a ser tomada, sea la primera que está en la lista.
•	Dar alguna solución sobre qué pasa con una llamada cuando entran más de 10 llamadas concurrentes. Para esta solución se utiliza un semáforo con una cantidad de recursos seteados al instanciar las llamadas. Cuando hay una llamada entrante y están todos los recursos tomados, es decir que el semáforo no tiene permisos para asignar, el proceso intenta hacer un acquire del recurso y se queda esperando hasta que uno sea liberado.
