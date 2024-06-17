```markdown
# Agent Sync Git Docker Image

## Descripción
Esta imagen Docker permite sincronizar repositorios Git utilizando OpenVPN y SSH. Es ideal para integraciones con servicios como Git + Semaphore, permitiendo la clonación y sincronización de repositorios hacia otros repositorios en plataformas como GitHub o Gitea alojado en servidores propios.

## Requisitos
- Docker
- Docker Compose

## Archivos Necesarios
Antes de desplegar la imagen, asegúrese de tener los siguientes archivos en la ruta especificada:

1. `auth.txt`: Contiene las credenciales del usuario para la conexión VPN.
    ```
    user
    pass
    ```

2. `client.ovpn`: Archivo de configuración del cliente OpenVPN.

3. `ClavePublicaSSH.txt`: Clave pública SSH utilizada por Git + Semaphore para clonar los repositorios.

## Estructura del Proyecto
Asegúrese de tener la siguiente estructura de directorios y archivos:

```
project-directory/
├── Dockerfile
├── docker-compose.yml
└── volumenes/
    ├── auth.txt
    ├── client.ovpn
    └── ClavePublicaSSH.txt
```

## Despliegue de la Imagen

### Paso 1: Crear el Dockerfile
Cree un archivo llamado `Dockerfile` con el siguiente contenido:

```dockerfile
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
```

### Paso 2: Crear el archivo de Docker Compose
Cree un archivo llamado `docker-compose.yml` con el siguiente contenido:

```yaml
version: '3.8'

services:
  agent-sync-git_v1:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: agent-sync-git_v1
    devices:
      - /dev/net/tun:/dev/net/tun
    cap_add:
      - NET_ADMIN
    ports:
      - "2222:22"    
    volumes:
      - /home/DockerStore/Git_OpenVPN/volumenes/client.ovpn:/etc/openvpn/client.ovpn
      - /home/DockerStore/Git_OpenVPN/volumenes/auth.txt:/etc/openvpn/auth.txt
      - /home/DockerStore/Git_OpenVPN/volumenes/ClavePublicaSSH.txt:/root/.ssh/authorized_keys
    restart: unless-stopped
```

### Paso 3: Crear los archivos de configuración
En el directorio `volumenes`, cree los archivos necesarios (`auth.txt`, `client.ovpn`, `ClavePublicaSSH.txt`) con el contenido adecuado.

### Paso 4: Construir y desplegar el contenedor
Navegue al directorio del proyecto y ejecute los siguientes comandos:

```bash
docker-compose build
docker-compose up -d
```

Esto construirá la imagen Docker y desplegará el contenedor en segundo plano.

### Paso 5: Verificación
Para verificar que el contenedor está funcionando correctamente, ejecute:

```bash
docker ps
```

Debería ver una entrada para `agent-sync-git_v1` con el puerto 2222 expuesto.

## Uso
- **SSH**: Puede conectarse al contenedor mediante SSH en el puerto `2222`.
- **OpenVPN**: El contenedor utilizará la configuración y credenciales proporcionadas para establecer una conexión VPN.
- **Git + Semaphore**: El servicio Git + Semaphore puede usar la clave pública SSH configurada para clonar y sincronizar repositorios.

## Notas
- Asegúrese de que el archivo `auth.txt` y `client.ovpn` contengan la configuración correcta para su VPN.
- La clave pública SSH debe estar correctamente configurada en los servicios de Git + Semaphore para la autenticación.

## Mantenimiento
Para detener y eliminar el contenedor, ejecute:

```bash
docker-compose down
```

Para reconstruir la imagen después de realizar cambios en el Dockerfile o archivos de configuración, ejecute:

```bash
docker-compose build
docker-compose up -d
```

## Contribución
Si desea contribuir a este proyecto, por favor, haga un fork del repositorio en GitHub y envíe un pull request.

## Contacto
Para cualquier consulta, póngase en contacto con el mantenedor:

Salvador Sanchez Sanchez - ssanchezhlg@gmail.com

[Documentación del proyecto](https://github.com/ssanchezhlg/agent-sync-git)

---

Gracias por usar Agent Sync Git Docker Image.
```
