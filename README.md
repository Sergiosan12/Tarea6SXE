# Tarea 6 SXE

<details>

  <summary>Creación del archivo DockerFile</summary>
<br>
Para crear este archivo se ejecuta el siguiente comando:

```bash
nano docker-compose.yaml
```
Este comando también permite modificar el archivo que se acaba de crear, así que se escribe lo siguiente dentro de él para crear los contenedotes de mySQL y PrestaShop con todo lo necesario:

```bash
services:
  mysql:
    image: mysql:latest # Descarga la última versión de mySQL
    container_name: base_mysql # Nombre del contenedor
    restart: always # Reinicia el contenedor siempre que se cierre.
    environment:
      MYSQL_DATABASE: prestashop_db # Especifica el nombre de la base de datos
      MYSQL_ROOT_PASSWORD: admin # Especifica la contraseña para el superusuario de la cuenta MySQL
    networks:
      - prestashop_network # Red que se va a crear y va a poder ser usada
  prestashop:
    image: prestashop/prestashop:latest # Última versión de prestashop
    container_name: prestashop
    restart: always
    depends_on:
      - mysql # Indica que prestashop depende primero del arranque de mysql 
    ports:
      - 8080:80 # Indica el puerto utilizado
    environment:
      DB_SERVER: mysql # Nombre de la base de datos mysql que se va a utilizar
      DB_NAME: prestashop_db # Sobreescribe el nombre de la base de datos
      DB_USER: root # Sobreescribe el nombre del usuario mysql por defecto
      DB_PASSWD: admin # Sobreescribe la contraseña mysql por defecto
    networks:
      - prestashop_network # Misma red que mysql
networks:
    prestashop_network:
  ```
Una vez guardado el archivo, se ejecuta el siguiente comando para lanzar este archivo:

```bash
docker compose up -d
```
Después de un tiempo de descarga la salida debería ser algo como esto:

![up](https://github.com/user-attachments/assets/6d316bfd-da8f-45f3-8517-fc8bc30a3f44)

Con este mensaje nos confirma que se crearon e iniciaron los contenedores deseados.

</details>

<details>

  <summary>Comprobación y configuración de PrestaShop</summary>
<br>
Para ver si todo funcionó bien se accede en el navegador a nuestra IP con el puerto configurado para los contenedores:

```bash
http://localhost:8080
```
Si todo salió bien dbería salir la página con el asistente de instalación de PrestaShop

![PS1](https://github.com/user-attachments/assets/55549fe0-b2de-4c27-ac4b-c8416e08f5d5)

Después de coninuar unos pasos habrá que configurar la conexión en la base de datos con los datos establecidos en el archivo.yaml.

En el apartado de Dirección del servidor de base de datos se pone el DB_Server de tu archivo .yaml seguido de :3306 porque es el puerto predeterminado de mySQL

  ![bdde](https://github.com/user-attachments/assets/51c05506-a20f-42dc-adcf-db192f47368d)

Con todo correcto, al darle a siguiente empezará a crear e instalar todo lo necesario.

Una vez instalado saldrá lo siguiente:

![finalI](https://github.com/user-attachments/assets/c5becc31-1201-4ded-a470-35db8fcab098)

</details>

<details>

  <summary>Acceso a tu tienda</summary>
  <br>
  Después de esto si queremos acceder a Administrar Tienda dará error.

  Para arreglar esto es necesario eliminar la carpeta Install como pone en la imagen de la siguiente manera:
  
  ```bash
  docker exec -it prestashop rm -rf /var/www/html/install
  ```
  Por último también cambiamos el nombre de la carpeta admin:
  
  ```bash
  docker exec -it prestashop mv /var/www/html/admin /var/www/html/admin552vw8sb9uvj8ucjobz
  ```

  En el navegador ponemos el siguiente enlace:
  
  ```bash
  http://localhost:8080/admin552vw8sb9uvj8ucjobz
  ```

  Esto nos llevará a la página de Prstashop donde nos pedirá nuestro nombre de usuario y contraseña antes establecidos.

  Una vez iniciado sesión ya podremos administrar nuestra tienda de Prestashop.
  
![final](https://github.com/user-attachments/assets/1ea386ef-be61-47ed-849b-8b5efe1e4629)

Enhorabuena!! Ya está todo listo para trabajar en ella!
  
</details>
