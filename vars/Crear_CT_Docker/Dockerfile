# Usar una imagen base de Alpine Linux
FROM alpine:latest

# Instalar OpenVPN y otras herramientas necesarias
RUN apk update && apk add --no-cache openvpn iptables bash git openssh-server python3

# Crear un directorio para los archivos de configuración de OpenVPN
RUN mkdir -p /etc/openvpn

# Copiar el archivo de configuración del perfil OpenVPN al contenedor
COPY client.ovpn /etc/openvpn/client.ovpn
COPY auth.txt /etc/openvpn/auth.txt


# Genera las claves de host SSH
RUN ssh-keygen -A   

# Copia la llave SSH al contenedor (cambia 'tu_llave_ssh' por el nombre de tu llave)
COPY ClavePublicaSSH.txt /root/.ssh/authorized_keys  

# Expone el puerto 22 para SSH
EXPOSE 22

# Inicia el servicio SSH
CMD ["/usr/sbin/sshd", "-D"]

# Comando para ejecutar OpenVPN con el perfil copiado
# CMD ["/bin/sh", "-c", "openvpn --config /etc/openvpn/client.ovpn"]

# CMD ["tail", "-f", "/dev/null"]
