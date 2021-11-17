# Práctica 1 - DNS Linux: MEMORIA PRINCIPAL

## Herramientas que tal vez no tengamos.
Antes de empezar a configurar el DNS del cliente ubuntu local, tendremos que descargarnos unas herramientas si no las tenemos ya instaladas:
- apt-get update
- ping: apt-get install iputils-ping
- tracepath: apt-get install iputils-tracepath
- mtr: apt install mtr
- ifconfig: apt -y install net-tools
- ifdown: apt-get install -y ifupdown
- host: apt-get install host
- ifplugd: apt-get install ifplugd
- whois: apt install whois
- curl: apt install curl
- wget: apt-get install wget
- dig: apt-get install dnsutils
- nano: apt-get install nano

## Crear los contenedores.
- Subir o crear un archivo docker-compose: docker-compose up

- Iniciar contenedores del docker-compose.yml: docker-compose start

- Parar contenedores: docker-compose stop

- Borrar contenedores: docker-compose down

- Reinstalar contenedores:docker-compose restart

## Configuración, arranque y parada de servicio bind9
Cuando se realizan cambios en los archivos de configuración del contenedor, hay que reiniciar el contenedor del servidor.

- Reiniciar el contenedor del servidor: docker restart nombre_del_contenedor

- Parar el contenedor del servidor: docker stop nombre_del_contenedor

- Iniciar el contenedor del servidor: docker start nombre_del_contenedor

- Borrar el contenedor del servidor: docker rm nombre_del_contenedor

## Docker-compose.yml: Crear una red.
- Para crear una red, introducimos: 
    - docker network create --subnet 10.1.0.0/24 --gateway 10.1.0.1 br02

## Añadir contenedores a una red.
Debemos indicar que los contenedores pertenecen a la red br02 con una ip fija:
~~~
networks:
    br02:
        ipv4_address: 10.1.0.4
~~~

Después, escribimos el siguiente comando en la etiqueta service para indicar que la red existe y que se va a trabajar con esta:
~~~
networks: 
    br02:
        external: true
~~~

## Asignar un servidor predeterminado.
Escribiendo este comando en la etiqueta image de cada contenedor, especificamos que la ip hace referencía al servidor dns.
~~~
dns:
    - 10.1.0.4
~~~

Pese a que funcione, a partir de cierta version del SO ya no se utiliza el archivo **resolv.conf** como configurador del servidor dns predeterminado, sino que se utiliza el servicio systemd. Debido a esto, al reiniciarse el cliente, el valor del archivo resolv.conf volvía al predeterminado (127.0.0.11).
