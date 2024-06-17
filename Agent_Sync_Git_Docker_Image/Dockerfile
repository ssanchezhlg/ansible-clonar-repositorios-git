# Usar una imagen base de Alpine Linux
FROM alpine:latest

# Información del mantenedor y otros detalles
LABEL maintainer="Salvador Sanchez Sanchez <ssanchezhlg@gmail.com>"
LABEL version="1.0"
LABEL description="Imagen Docker para sincronizar repositorios Git usando OpenVPN y SSH."
LABEL project="Agent Sync Git"
LABEL documentation="https://github.com/ssanchezhlg/agent-sync-git"
LABEL build_date="2024-06-16"

# Instalar OpenVPN y otras herramientas necesarias
RUN apk update && apk add --no-cache openvpn git openssh-server python3 bash curl 

# Crear un directorio para los archivos de configuración de OpenVPN
RUN mkdir -p /etc/openvpn

# Genera las claves de host SSH
RUN ssh-keygen -A   

# Crear el directorio .ssh y establecer permisos
RUN mkdir -p /root/.ssh && chmod 700 /root/.ssh

# Exponer el puerto 22 para SSH
EXPOSE 22

# Inicia el servicio SSH y mantiene el contenedor en ejecución
CMD ["/bin/sh", "-c", "/usr/sbin/sshd && tail -f /dev/null"]