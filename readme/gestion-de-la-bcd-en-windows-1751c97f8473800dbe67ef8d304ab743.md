# Gestión de la BCD en Windows

## Ejercicio 1

1.  Asigna la letra D: a la partición EFI

    Abrimos la consola e introducimos el comando `diskpart` para poder manipular los discos y particiones, con el comando `list disk` veremos que discos hay disponibles

    ![VirtualBoxVM\_iaqZz979fL.png](<../img/Gestion de la bcd/VirtualBoxVM_iaqZz979fL.png>)

    Seleccionamos el disco correspondiente con `select disk Nº`y vemos que particiones hay disponibles con el comando `list partition`

    ![VirtualBoxVM\_XO3V2c7YXq.png](<../img/Gestion de la bcd/VirtualBoxVM_XO3V2c7YXq.png>)

    Ahora seleccionamos la particion que queremos con `select partition`y le asignamos la letra T con `assign letter=T` y salimos del programa `exit`

    ![VirtualBoxVM\_op8xGUW55X.png](<../img/Gestion de la bcd/VirtualBoxVM_op8xGUW55X.png>)

## Ejercicio 2

*   Busca la línea que contiene "Detected boot environment" en el fichero C:\Windows\Panther\setupact.log para saber qué firmware tiene la máquina

    Con el comando `find /I "Detected boot Environment" C:\Windows\Panther\setupact.log` veremos que nuestro sistema tiene instalado el firmware BIOS

    ![VirtualBoxVM\_2k2NCjf3Em.png](<../img/Gestion de la bcd/VirtualBoxVM_2k2NCjf3Em.png>)

## Ejercicio 3

1.  Crear una entrada para el modo seguro y configurala en este

    Creamos una copia de la entrada principal y le pones un nombre con el comando `bcdedit /copy {current} /d "ModoS"`

    ![VirtualBoxVM\_FdX5iJD36a.png](<../img/Gestion de la bcd/VirtualBoxVM_FdX5iJD36a.png>)

    Y para ponerla en modo safe ejecutamos lo siguiente: `bcdedit /set {Identficador} safeboot minimal`

    ![VirtualBoxVM\_rGwZNGrJfZ.png](<../img/Gestion de la bcd/VirtualBoxVM_rGwZNGrJfZ.png>)
2.  Crea una entrada para el modo recovery

    Hacemos otra copia del current y le ponemos el nombre recovery con el comando `bcdedit /copy {current} /d "ModoR"`

    ![VirtualBoxVM\_GP9KrJKlZy.png](<../img/Gestion de la bcd/VirtualBoxVM_GP9KrJKlZy.png>)

    Y ahora la ponemos en modo recovery con `bcdedit /set {Identificador} recoveryenabled yes`

    ![VirtualBoxVM\_o60iy7O172.png](<../img/Gestion de la bcd/VirtualBoxVM_o60iy7O172.png>)
3.  Ahora al encender la maquina comprobamos que nos da a elegir entre los diferentes metodos disponibles, entre ellos los que hemos creado

    ![VirtualBoxVM\_Zh91bdL22B.png](<../img/Gestion de la bcd/VirtualBoxVM_Zh91bdL22B.png>)

## Ejercicio 4

Repara el BCD con la copia de seguridad

1.  Primero hay que hacer una copia de seguridad del BCD y lo haremos con el comando `bcdedit /export C:\bcd_backup.bcd`

    ![VirtualBoxVM\_tZpmAl9ASD.png](<../img/Gestion de la bcd/VirtualBoxVM_tZpmAl9ASD.png>)
2.  Ahora destruimos la entrada default con bcdedit con el comando `bcdedit /delete {current}`

    ![VirtualBoxVM\_PF87jZPX6c.png](<../img/Gestion de la bcd/VirtualBoxVM_PF87jZPX6c.png>)
3.  Ahora para borrar el archivo BCD hay que hacer un par de cosas:

    1.  Con la ISO dentro de la maquina reinciamos y le damos a cualquier tecla cuando nos pregunte si queremos montar desde un CD o DVD y nos saldra esto:

        ![VirtualBoxVM\_ilS7QfGbwx.png](<../img/Gestion de la bcd/VirtualBoxVM_ilS7QfGbwx.png>)
    2.  Le damos a siguiente y luego a reparar el equipo

        ![VirtualBoxVM\_B9KdP0IOdH.png](<../img/Gestion de la bcd/VirtualBoxVM_B9KdP0IOdH.png>)
    3.  Y le damos a las opciones hasta llegar a esta consola

        ![VirtualBoxVM\_HTOcFBO5FL.png](<../img/Gestion de la bcd/VirtualBoxVM_HTOcFBO5FL.png>)
    4.  Ahora le asignamos una letra a la particion reservada para archivos del sistema

        ![VirtualBoxVM\_FJh4n0qGMa.png](<../img/Gestion de la bcd/VirtualBoxVM_FJh4n0qGMa.png>)
    5.  Y accedemos al disco y eliminamos el archivo bcd con el comando `dir /A |bcd.* /s`

        ![VirtualBoxVM\_bjnuvJm58G.png](<../img/Gestion de la bcd/VirtualBoxVM_bjnuvJm58G.png>)

    Y ahora si reiniciamos comprobaremos que esta roto

    ![VirtualBoxVM\_v6IRnShJQm.png](<../img/Gestion de la bcd/VirtualBoxVM_v6IRnShJQm.png>)

    Ahora reiniciamos y le damos a cualquier tecla para inciar el instalador de windows y llegar a la consola de antes. A continuacion le asignamos una letra para poder acceder al backup de la BCD para restaurarlo.

    ![VirtualBoxVM\_xkb9jbB6W6.png](<../img/Gestion de la bcd/VirtualBoxVM_xkb9jbB6W6.png>)

    Entonces ahora podemos acceder al disco Y y restaurar la BCD desde ahi con el comando `bcdedit /import {nombredelbackup}`

    ![VirtualBoxVM\_x2fmMDB0Yl.png](<../img/Gestion de la bcd/VirtualBoxVM_x2fmMDB0Yl.png>)

    Y si reiniciamos el windows veremos que ya funciona todo correctamente otra vez

    ![VirtualBoxVM\_OigSB4NfcH.png](<../img/Gestion de la bcd/VirtualBoxVM_OigSB4NfcH.png>)

Ahora repararemos el BCD con el programa bootrec y compararemos los resultados de estas dos maneras de reparar el BCD

1. Primero borramos la BCD igual que hemos hecho antes. (Mirar imagenes de arriba)
2. Entramos en el modo WinRe y vamos a la consola.
3. Asignamos una letra al disco donde tenemos el backup
4.  Accedemos al disco y ejecutamos el comando `bootrec /rebuildbcd`

    ![VirtualBoxVM\_WcT1MFfqw9.png](<../img/Gestion de la bcd/VirtualBoxVM_WcT1MFfqw9.png>)
5. Nos da a elegir un backup para la BCD, lo seleccionamos y ya podemos reiniciar la maquina normal otra vez

Ahora podemos comparar el estado del bcd despues de usar el metodo con el backup nuestro o mediante el comando `bootrec`

![VirtualBoxVM\_iaqIu1aQ9J.png](<../img/Gestion de la bcd/VirtualBoxVM_iaqIu1aQ9J.png>)

![ytRbLJ33rb.png](<../img/Gestion de la bcd/ytRbLJ33rb.png>)

Hay muchos cambios pero el principal para mi es que mediante el comando `bootrec` todas las otras BCD que habiamos creado antes como safeboot y recovery han desparecido. bruh'nt
