#!/bin/bash

# Detección de la Distribución Linux
detect_distro() {
    if [ -f /etc/os-release ]; then
        # Freedesktop.org and systemd
        . /etc/os-release
        DISTRO=$ID
    elif type lsb_release >/dev/null 2>&1; then
        # Linux Standard Base support
        DISTRO=$(lsb_release -si | tr '[:upper:]' '[:lower:]')
    elif [ -f /etc/lsb-release ]; then
        # For some versions of Debian/Ubuntu without lsb_release command
        . /etc/lsb-release
        DISTRO=$DISTRIB_ID
    elif [ -f /etc/debian_version ]; then
        # Older Debian/Ubuntu/etc.
        DISTRO="debian"
    elif [ -f /etc/redhat-release ]; then
        # Older Red Hat, CentOS, etc.
        DISTRO=$(cat /etc/redhat-release | awk '{print tolower($1)}')
    else
        DISTRO=$(uname -s)
    fi
}

# Identificación del Gestor de Paquetes y actualización del sistema
update_and_upgrade() {
    case $DISTRO in
        ubuntu|debian)
            PKG_MANAGER="apt"
            sudo apt update
            sudo apt upgrade -y
            ;;
        fedora)
            PKG_MANAGER="dnf"
            sudo dnf update -y
            sudo dnf upgrade -y
            ;;
        centos)
            PKG_MANAGER="yum"
            sudo yum update -y
            sudo yum upgrade -y
            ;;
        arch)
            PKG_MANAGER="pacman"
            sudo pacman -Syu --noconfirm
            ;;
        *)
            echo "Distribución Linux no soportada: $DISTRO"
            exit 1
            ;;
    esac
}

# Instalación de Apache
install_apache() {
    case $DISTRO in
        ubuntu|debian)
            sudo apt install -y apache2
            ;;
        fedora)
            sudo dnf install -y httpd
            ;;
        centos)
            sudo yum install -y httpd
            ;;
        arch)
            sudo pacman -S --noconfirm apache
            ;;
    esac

    # Iniciar y habilitar el servicio de Apache
    case $DISTRO in
        ubuntu|debian|fedora|centos)
            sudo systemctl start apache2 || sudo systemctl start httpd
            sudo systemctl enable apache2 || sudo systemctl enable httpd
            ;;
        arch)
            sudo systemctl start httpd
            sudo systemctl enable httpd
            ;;
    esac
}

# Modificación del archivo index.html
modify_index_html() {
    echo "<!DOCTYPE html>
<html lang=\"en\">
<head>
<meta charset=\"UTF-8\">
<meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">
<title>Bootcamp DevOps</title>
</head>
<body>
<h1>Bootcamp DevOps Engineer</h1>
</body>
</html>" | sudo tee /var/www/html/index.html > /dev/null
}

# Ejecución de funciones
detect_distro
update_and_upgrade
install_apache
modify_index_html

echo "Configuración completada en la distribución $DISTRO utilizando $PKG_MANAGER."
