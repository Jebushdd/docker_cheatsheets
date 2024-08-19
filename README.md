# Aprendiendo Docker
En este repo voy dejando cheatsheets, fun facts y workarounds de mi uso cotidiano de docker. También voy dejando los links a las fuentes de donde busco información.

Uno de mis proyectos públicos donde uso docker es un proceso de ETL [en este repo](https://github.com/Jebushdd/postgres_etl)

### <img title="YouTube" alt="YouTube" src="https://img.shields.io/badge/YouTube-%23FF0000.svg?style=for-the-badge&logo=YouTube&logoColor=white" height="25" style="margin: 0 auto; margin-bottom: -7px;"> &emsp; [Eduardo Claro](https://www.youtube.com/@returngis)
**De su video [Installing Colima and Docker in Apple Silicon ARM based machines](https://www.youtube.com/watch?v=7zgNe1CRJl0)**

Si usas una pc Mac, tal vez sea necesario que revises la mejor manera de instalar y ejecutar docker en MacOs. También podés seguir los pasos [en este doc](./notes/install_docker_macos.md)

### <img title="YouTube" alt="YouTube" src="https://img.shields.io/badge/YouTube-%23FF0000.svg?style=for-the-badge&logo=YouTube&logoColor=white" height="25" style="margin: 0 auto; margin-bottom: -7px;"> &emsp;  [Return(GiS);](https://www.youtube.com/@returngis)
**De su playlist [Serie sobre contenedores 📦🐳](https://www.youtube.com/playlist?list=PLO9JpmNAsqM6PxlmKj6kfX-a8WwZJnwD9)**

Los comandos más comunes de usar al dar nuestros primeros pasos con Docker son:
#### Correr una imagen de Docker
1. docker run [nombre de la imagen]
    ```
    docker run hello-world
    ```
    Cuando sabemos qué imagen queremos iniciar con docker solo es necesario usar el comando `run` junto con su nombre y las opciones que requiera. Si no la tenemos descargada aún, docker las buscará en [docker hub](https://hub.docker.com/).
2. docker search [clave de búsqueda]
    ```
    docker search nginx
    ```
    Si no conocemos el nombre exacto de la imagen pero tenemos alguna palabra clave que me ayude a encontrarla, podemos usar este comando para recibir una lista de imagenes cuyo nombre coincida con nuestra búsqueda.

#### Aceder a un puerto del contenedor de Docker
3. docker run -p [puerto local]:[puerto de la imagen] [nombre de la imagen]
    ```
    docker run -p 8080:80 nginx
    ```
    Si iniciamos una imagen de docker con la que queremos interactuar a través de sus puertos (por ejemplo un servidor web, un servicio de bases de datos, etc.), necesitamos conectar nuestro puerto local con el de dicha imagen.  
    Siguiendo el código de ejemplo, usando nuestro puerto local 8080 podremos interactuar con la imagen de nginx a través de su puerto 80.

#### Iniciar un contenedor de docker y poder seguir usando la misma ventana
4. docker run -d [nombre de la imagen]
    ```
    docker run -d -p 8080:80 nginx
    ```
    Por defecto, cuando iniciamos un contenedor comenzamos a ver todo el log del proceso. Seguido a esto, también vemos la actividad del contenedor mientras esté en ejecución, dejando nuestra ventana reservada exclusivamente para monitorear esa actividad.  

    Para evitar esto, la opción `-d` nos permite iniciar el contenedor ocultando el log de su actividad. Con esto podemos seguir ejecutando comandos sin necesidad de abrir nuevas ventanas.

#### Ver los últimos logs del contenedor
5. docker logs [id del contenedor]
    
    Si queremos saber cual fue la última actividad de nuestro contenedor sin tener que ingresar al mismo, solo ejecutamos este comando y va a imprimir los logs más recientes.

#### Ingresar al contexto del contenedor
6. docker logs [id del contenedor] -f
    
    Si queremos monitorear la actividad del contenedor, ejecutamos este comando y vamos a ingresar al contexto del mismo. Para salir del contexto y volver a tener disponible nuestra línea de comandos solo debemos presionar `control + c`.
    
#### Darle nombre a mis contenedores
6. docker run --name [nombre del contenedor] [nombre de la imagen]
    ```
    docker run --name web -d -p 8080:80 nginx
    ```
    Darle un nombre personalizado al contenedor que iniciemos nos ayuda a gestionarlos fácilmente y tener un entorno más prolijo.
    
#### Pasar comandos a un contenedor activo
7. docker exec [nombre del contenedor] [comando para el contenedor]
    ```
    docker exec web cat /etc/os-release
    ```
    Si solamente deseamos que nuestro contenedor ejecute un comando cuando ya está activo.

#### Ingresar a la terminal de un contenedor activo
8. docker exec -it [nombre del contenedor] [tipo de terminal]
    ```
    docker exec -it web bash
    ```
    En este caso si deseamos usar el terminal de un contenedor activo como si estuvieramos dentro del mismo.

#### Detener un contenedor activo
9. docker stop [nombre del contenedor]
    ```
    docker stop web
    ```
    Detenemos un contenedor que ya no necesitemos seguir usando.

#### Eliminar un contenedor
10. docker rm [nombre del contenedor]
    ```
    docker rm web
    ```
    Eliminamos un contenedor que ya no necesitemos. Podemos forzar la eliminación usando la opción `-f`.
    
#### Obtener una lista de mis contenedores
11. docker ps [-a] [-q]
    
    Con la opción `-a` listamos todos los contenedores, estén o no activos.  
    Con la opción `-q` solo recibimos el id de los contenedores listados.
    
#### Eliminar todos los contenedores
12. docker rm $(docker ps -a -q) [-f]
    
    Podemos usar la opción `-f` para forzar la eliminación de contenedores activos.

#### Obtener una lista de mis imágenes
13. docker images [-q]
    
    Podemos usar la opción `-q` para obtener solo los ids
    
#### Eliminar una imagen
14. docker rm [nombre o id de la imagen]
    ```
    docker rmi nginx
    ```
    Eliminamos una imagen que ya no necesitemos. 
    
#### Eliminar todas mis imágenes
15. docker rmi $(docker images -q)
    
    Eliminamos todas nuestras imágenes de docker.
    