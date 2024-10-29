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
    image: mysql:latest
    container_name: base_mysql
    restart: always
    environment:
      MYSQL_DATABASE: prestashop_db # Nmbre de la base de datos
      MYSQL_ROOT_PASSWORD: password # Contraseña para el superusuario de MySQL
    networks:
      - prestashop_network # Red que se crea
  prestashop:
    image: prestashop/prestashop:latest
    container_name: prestashop
    restart: always # Reinicia el contenedor cuandon se detiene
    depends_on:
      - mysql # Prestashop depende de la iniciación de mysql
    ports:
      - 8000:80 
    environment:
      DB_SERVER: base-mysql # Nombre de la base de datos mysql
      DB_NAME: prestashop_db # Sobreescribe el nombre de la base de datos de mysql por defecto
      DB_USER: sergio # Sobreescribe el nombre del usuario de mysql por defecto
      DB_PASSWD: password # Sobreescribe la contraseña de mysql por defecto
    networks:
      - prestashop_network
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
