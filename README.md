# Instalacion Koel en AWS

> El objetivo del ejercicio es instalar Koel usando recursos de AWS. Al final deberíamos de poder ver el servidor de música en streaming funcionando en `https://koel.<tudominio>`

## ¿Qué es Koel?

[**Koel**](https://koel.dev/) es un servidor personal de música en streaming. Está escrito en `VueJS` en el frontend y `Laravel` en el backend.  

## Requisitos
* https://koel.dev/#req
* Repositorio: https://github.com/koel/koel
* Requisitos para instalar: https://docs.koel.dev/#server
* Documentación general de instalación: https://docs.koel.dev/#building-from-source

> Lee la documentación de cómo se construye desde el código fuente. 

## Instalación

> Como hay tareas que duran varios minutos, aprovecha el tiempo para ir documentando.

### 1. Crear una instancia EC2 (Amazon Linux 2023)
* Instala apache, php, composer, nodejs, git, yarn, etc.
* Prepara una base de datos (mysql o postgresql). Para la instalación necesitarás:
    * Host de la base de datos
    * Tipo de base de datos (mysql o postgresql)
    * Nombre de la base de datos
    * Usuario de la base de datos
    * Contraseña de la base de datos
> La base de datos puede estar instalada en local en EC2 o mejor si usas un servicio RDS

### Ayudas:
* Instalación apache y php: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-lamp-amazon-linux-2023.html
* Instalación composer: https://getcomposer.org/download/
* Instalación de nodejs (con dnf o yum)
* Instalación de yarn (con npm)
* El usuario que usa apache en este linux es `apache`

### 2. Instalar Koel
* Sigue las instrucciones de https://docs.koel.dev/#building-from-source
* Ve haciendo capturas de pantalla para la documentación. 

Si tienes errores al instalar con `composer install`, revisa los requisitos.

Con `php artisan koel:init` se crean las tablas de la base de datos, se crean los usuarios por defecto y se compilan los assets y más recursos.

### 3. Test del servidor
Una vez instalado, comprueba que funciona correctamente. Usa el servidor de desarrollo de laravel, que por defecto usa el puerto 8000. Tendrás que abrir ese puerto en la instancia EC2 para poder acceder. 

Por defecto al servidor de prueba solo se puede acceder desde el localhost. Para acceder desde otro equipo necesitas ejecutarlo así:
`$ php artisan serve --host 0.0.0.0`

Si no sabes qué usuario ha creado por defecto, lo verás al intentar cambiar la contraseña con `php artisan koel:admin:change-password`

Sube una canción y comprueba que se puede reproducir.

### 4. Configurar apache

Si ves que el servidor de pruebas ya funciona correctamente, haz que la web se vea usando apache (puerto 80). Configura el `Virtualhost` correspondiente.

### 5. Configurar el dominio

* Prepara un subdominio para el sitio web del tipo `koel.<tudominio>`. 
* Configura el DNS para que apunte a la instancia EC2 de Koel.
* Prepara el certificado SSL para el dominio (`certbot`) y configura apache para que use el certificado.
* Haz que las conexiones vayan siempre por `https`. 

### 6. Documentación
Además de realizar la práctica debes documentarla. Crea un nuevo archivo `INSTALL.md` donde vas a ir documentando en `markdown` todos los pasos que sigues para la instalación. 

