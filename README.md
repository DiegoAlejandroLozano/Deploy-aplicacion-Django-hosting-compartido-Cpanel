# Deploy de una aplicación Django en un hosting compartido con Cpanel

Para hacer el deploy en un hosting compartido de una aplicación desarrollada en Django, es muy importante que el hosting cuente con el panel de control Cpanel, la posibilidad de crear una aplicación Python desde el Cpanel y de conectarse al hosting mediante una conexión SSH. 

**Paso  1:** Ingresar al Cpanel y dentro de la sección “software”, ubicar la opción “Setup Python App”.


 
**Paso 2:** Una vez dentro de la opción “Setup Python App”, creamos una aplicación especificando el directorio donde esta se guardará (No es necesario crear un directorio con anterioridad, este se crea automáticamente al crear la App) y el dominio de la App (Si se especifica “/”, se puede ingresar a la aplicación desde el dominio principal).

 

Como se puede apreciar en la imagen de arriba, la aplicación se guardará en el directorio llamado “app_django” y para acceder a esta, se debe ingresar a “TuDominio.com/app”. 

**Paso 3:** Una vez tenemos estos campos, presionamos en el botón Setup, dando como resultado la siguiente imagen.

 

**Paso 4:** Antes de subir nuestro proyecto Django al hosting, es muy importante que se genere el archivo de requerimientos mediante el siguiente comando:
 
     pip freeze > requerimientos.txt

 






**Paso 5:** Una vez creado el archivo de requerimientos, se procede a empaquetar el proyecto dentro de un archivo .zip para que pueda ser subido mediante el Cpanel dentro del directorio “app_django”. 

 

**Paso 6:** Después de que el archivo ha sido subido, se procede a descomprimir el proyecto y a borrar el archivo .zip.

 

**Paso 7:** Para instalar los requerimiento de la aplicación (requerimientos.txt), se abre una conexión ssh con el hosting. 

**Paso 8:** Se ingresa dentro del directorio de la aplicación mediante el siguiente comando: 

    cd /home/usuario/app_django

 
**Paso 9:** Se activa el entorno virtual como se especifica en el Cpanel.

 

 

**Paso 10:** Cuando el entorno virtual ha sido activado, se procede a instalar los requerimientos con el siguiente comando: 

    pip install –r requerimientos.txt












**Paso 11:** Con la ayuda del Cpanel, se edita el archivo “settings.py” y se realiza los siguientes cambios: 
~~~
DEBUG = False
ALLOWED_HOSTS = [‘*’]
DATABASES = {
    'default': {
        'ENGINE'		: 'mysql_cymysql', (Driver de la base de datos)
        'NAME'      	: 'nombre de la base de datos',
        'HOST'      		: 'host donde se encuentra la base datos',
        'USER'      		: 'usuario de la base datos',
        'PASSWORD'  	: 'contraseña',
        'PORT'      		: 'puerto donde se conectará a la base de datos'
    }
}
STATIC_ROOT = '/home/usuario/public_html/static'
MEDIA_URL   = '/media/'
MEDIA_ROOT  = '/home/usuario/public_html/media '
~~~

**Paso 12:** Se crea la carpeta “static” y “media” en el directorio “public_html” con los siguientes comandos:

~~~
cd /home/usuario/public_html
mkdir static
mkdir media
~~~

**Paso 13:** Se comprueba que no haya ningún problema de configuración mediante el comando `./manage.py check`. Para poder ejecutar el archivo “manage.py”, primero se debe habilitar con el comando `chmod +x manage.py`.

**Paso 14:** Se ejecutan las migraciones con los siguientes comandos:

~~~
./manage.py makemigrations
./manage.py migrate
~~~

**Paso 15:** Se define la locación del archivo WSGI. Para hacer esto, primero se obtiene la ruta completa del archivo wsgi.py con el comando pwd.

 





Luego se pega la ruta en el App settings, se presiona el botón Save y por último, el botón Update.

 

**Paso 16:** Finalmente se ejecuta el comando para recolectar todos los archivos estáticos:

    ./manage.py collectstatic
