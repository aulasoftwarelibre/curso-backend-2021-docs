# Día 1

## Instalación

### Docker

Es necesario seguir las instrucciones del [taller de Docker](https://aulasoftwarelibre.github.io/taller-de-docker/) y tener instalados:

- Docker-CE (última versión)
- Docker Composer (última versión)

También se recomienda configurar el usuario con el grupo _docker_ para tener permisos para usar los contenedores sin necesidad de usar sudo.

### Clonado del repositorio del curso

Iremos al repositorio del curso [https://github.com/aulasoftwarelibre/curso-backend-2021](https://github.com/aulasoftwarelibre/curso-backend-2021) y clonaremos el proyecto en nuestro perfil con la opción _Fork_.

También podemos hacerlo directamente pulsando en el siguiente enlace:

- [https://github.com/aulasoftwarelibre/curso-backend-2021/fork](https://github.com/aulasoftwarelibre/curso-backend-2021/fork)

Una vez tengamos nuestra copia del proyecto lo clonaremos en nuestro equipo:

    git clone https://github.com/nuestro-usuario/curso-backend-2021

## Construcción de la imagen

El repositorio está basado en la imagen de [dunglas/symfony-docker](https://github.com/dunglas/symfony-docker) y la usaremos por comodidad. Para instalar el proyecto usaremos la siguiente order:

    make dev

Cuando termine el proyecto estará accesible en [https://rutas.localhost](https://rutas.localhost)

### Permisos

El contenedor se compila e instala con permisos de superusuario, necesitaremos cambiar los permisos para poder modificar los ficheros. Ejecutaremos lo siguiente en otra consola:

    make fix

### Acceso al contenedor

Para obtener acceso shell al contenedor de nuestra aplicación ejecutaremos lo siguiente:

    make shell
