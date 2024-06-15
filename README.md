# Linux Distro Detection and Apache Setup Script

Este script en Bash detecta la distribución de Linux, identifica su gestor de paquetes, realiza una actualización del sistema, instala el servidor web Apache y modifica el archivo `index.html` con un mensaje personalizado.

## Instrucciones

### 1. Detección de la Distribución Linux

El script detecta si la distribución es Ubuntu, Debian, Fedora, CentOS o Arch Linux.

### 2. Identificación del Gestor de Paquetes

Dependiendo de la distribución, el script utilizará `apt` para Ubuntu/Debian, `dnf` para Fedora, `yum` para CentOS, y `pacman` para Arch Linux.

### 3. Actualización del Sistema

El script ejecuta los comandos necesarios para actualizar la lista de paquetes y realizar una actualización completa del sistema.

### 4. Instalación del Servidor Web Apache

Instala Apache y modifica el archivo `index.html` en el directorio raíz del servidor web para contener el siguiente contenido:

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Bootcamp DevOps</title>
</head>
<body>
<h1>Bootcamp DevOps Engineer</h1>
</body>
</html>
