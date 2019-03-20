# laravel_docker
mi primera app laravel Dockerizada

# Correr El Contenedor

cp .env.example .env

Y le configuramos las variables que hacen sentido con nuestro contenedor

DB_CONNECTION=mysql;
DB_HOST=db
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=usuario_laravel
DB_PASSWORD=el_password_de_la_app

Y finalmente

docker-compose up -d

Si esto es correcto, podemos correr dos comandos mas para adicionar seguridad a nuestra aplicación:

docker-compose exec app php artisan key:generate
docker-compose exec app php artisan config:cache

Con lo cual agregamos una llave de encripción generada para este contenedor, y cacheamos nuestra configuración.

Migrando Datos
Ahora que ya tenemos todo montado, ya deberíamos ser capaces de ver nuestra aplicación en el navegador. Y para migrar la data necesitamos crear un usuario para la base de datos. Entonces primero accedemos a la base de datos asi:

docker-compose exec db bash

Y luego accedemos al cliente de mysql:

mysql -u root -p <su password>

una vez dentro vemos las bases de datos que estan creadas:

y vemos que esta la que nombramos laravel. Entonces creamos el usuario corriendo las siguientes consultas:


mysql> GRANT ALL ON laravel.* TO 'usuario_laravel'@'%' IDENTIFIED BY 'su_password_de_la_base_de_datos';
mysql> FLUSH PRIVILEGES;
mysql> EXIT;

y finalmente nos salimos del contenedor:

	
root@{id_del_contenedor}:/# exit

Ahora a correr las migraciones:

docker-compose exec app php artisan migrate

