# Servicio Social
Para hacer mas ágil el proceso de adopción del código se creó un contenedor docker con el ambiente virtual utilizado instalado en el mismo.

El contenedor puede ser encontrado en [DockerHub](https://hub.docker.com/r/emmanuelzula/servicio).

## Preparando el contenedor
Una vez dentro del contenedor creamos una carpeta donde descargar repositorio actual y nos desplazamos a ella con los comandos .
```
mkdir torchmd-net
cd torchmd-net
```
Descargamos el repositorio actual
```
git clone https://github.com/emmanuelzula/ServicioSocial
```
Activamos el entorno virtual
```
mamba activate torchmd-net
```
Ahora el código esta listo para ser ejecutado
Se recomienda utilizar Visual Studio Code para hacer las fluido el flujo de trabajo.