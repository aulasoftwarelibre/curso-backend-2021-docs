# Día 2

## Consola

Symfony trae una consola que permite realizar un gran cantidad de operaciones con nuestro proyecto. Normalmente los comandos se identifican con un prefijo, que indica un grupo de operaciones y uno o varios sufijos que es la operación de ese grupo que queremos lanzar.

La consola está dentro del directorio bin y, si estamos en la raiz del proyecto la podemos lanzar con:

    bin/console

Para ver el listado de comandos:

    bin/console list

Los grupos que más nos interesan son:

- config: para ver la configuración del entorno
- debug: para obtener información de depuración
- doctrine: para manejar la base de datos.
- doctrine:database: permite crear o borrar la base de datos
- doctrine:migrations: permite administrar las migraciones
- doctrine:schema: permite administrar el esquema de nuestra base de datos
- make: conjunto de assistentes que nos permite crear nuevos elementos del sistema

Symfony soporta plugins, aquí llamados bundles, que permiten añadir nuevas funcionalidades al framework, entre ellas nuevos comandos a la consola.

El grupo de comandos make nos da acceso a diferentes asistentes que nos permitirán realizar una configuración básica del sistema. Algunas de estas funciones son:

## Controladores

### make:controller

Si ejecutamos:

    bin/console make:controller

Nos creará un controlador básico, cuyo ruta será el nombre del controlador. Ejemplo:

    bin/console make:controller homepage

Creará el archivo `src/Controller/HomepageController.php` y la correspondiente plantilla en `templates/homepage/index.html.twig`.

### make:crud

Normalmente, las operaciones que queremos realizar con nuestro proyecto son operaciones tipo CRUD. Con este asistente podremos crear un controlador típico que nos permitirá generar este tipo de controladores.

Lo veremos más adelante.

## Seguridad

### make:user

Si ejecutamos:

    bin/console make:user

Nos creará una clase que representa a un usuario de la aplicación. Ejemplo:

    bin/console make:user client

Y responderemos _yes_ para guardarlo en la base de datos, _email_ como nombre de usuario y _yes_ para guardar la contraseña.

La respuesta que obtendremos será esta:

    created: src/Entity/Client.php
    created: src/Repository/ClientRepository.php
    updated: src/Entity/Client.php
    updated: config/packages/security.yaml

Lo cual nos crea una entidad usuario y su repositorio y nos configura el archivo de seguridad.

### make:registration-form

Nos permite generar un formulario de registro. Ejemplo:

    bin/console make:registration-form

Y responderemos: _yes_ para asegurar que no se genera dos usuarios con el mismo correo, _no_ a enviar un correo de confirmación, _yes_ para autenticar al usuario automáticamente y escogeremos _homepage_ como página de retorno.

La respuesta que obtendremos:

    updated: src/Entity/Client.php
    created: src/Form/RegistrationFormType.php
    created: src/Controller/RegistrationController.php
    created: templates/registration/register.html.twig

Indica que ha actualizado la clase cliente, creado el formulario de registro, el controlador y la plantilla.

### make:auth

Nos permite generar una página de inicio de sesión. Ejemplo:

    bin/console make:auth

Y responderemos: _1. Login Form Auth_, _ClientAuthenticator_ como nombre de la clase, _ClientSecurityController_ como nombre del controlador y _yes_ a la pregunta final para generar la ruta de logout.

## Doctrine

Para nosotros lo más importante es generar nuestro diagrama de clases, pero para que podamos almacenar la información en la base de datos debemos establecer una relación entre clases y tablas, entre instancias y tuplas, entre attributos y campos.

Para ello tenemos otro asistente más que es:

    bin/console make:entity

Este asistente permite crear o modificar entidades. Por ejemplo, vamos a modificar la entidad cliente para añadir la fecha de nacimiento:

    bin/console make:entity client

Symfony detecta que ya hay una clase con ese nombre y entra en modo edición, si no existiera crearía una nueva, asignándole un id automáticamente.

Posteriormente el asistente nos irá preguntando el nombre de cada atributo de nuestra clase y de qué tipo de dato se trata, o si se trata de una relación (uno a uno, uno a muchos, muchos a uno, muchos a muchos). Dependiendo de si indicamos que es un atributo o una relación, nos hará preguntas extra, como si el valor pueder ser nulo en el caso de los atributos, o si la relación puede ser nula (0 ó 1, 0 ó n) en el caso de relaciones. Entre otras preguntas. Todas están relacionadas con los conocimientos que ya tenéis de bases de datos relaciones. Es importante que sepáis ver esa relación para entender lo que ocurre.

Vamos a añadir el campo _bornAt_ para almacenar la fecha de nacimiento. El sistema nos indicará que es del tipo datetime, nosotros indicaremos que es de tipo date. E indicaremos que puede ser nulo en la base de datos. Pulsaremos intro de nuevo y la clase Cliente se habrá modificado.

Symfony por defecto establece que todos los atributos son de tipo cadena, excepto para aquellos que acaben en _At_ que los trata como fecha y hora y los que empiezan por _is_ que los trata como si fueran boleanos. Es un convenio y no es obligatorio que se llamen así.

### Migraciones

Aunque podríamos manejar el esquema con la orden `doctrine:schema:update` de la consola es conveniente usar migraciones. Una migración es un conjunto de cambios entre una versión y otra. Es decir, que debemos generar las migraciones solo cuando vayamos a crear una nueva versión del software, o después de un conjunto de cambios (commits) de nuestro sistema, para compartirlos con el resto de usuarios.

Para generar una migración ejecutaremos:

    bin/console make:migration

Lo cual nos generará un nuevo script de migraciones en el directorio _migrations_. Podemos aprovechar para modificarlo si nos interesa y ejecutarlo con:

    bin/console doctrine:migrations:migrate

Más información de [Doctrine](https://symfony.com/doc/current/doctrine.html)
