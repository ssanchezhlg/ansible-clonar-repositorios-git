# Ansible Playbook para Clonar y Configurar Repositorios Git

Este Ansible Playbook está diseñado para clonar repositorios Git y configurar remotos adicionales. Incluye soporte para el uso opcional de un proxy y permite habilitar o deshabilitar repositorios específicos según sea necesario.

## Archivos

- `playbook.yml`: El archivo principal del playbook.
- `vars/datos_generales.yml`: Archivo de variables que contiene la configuración de los repositorios y del proxy.

## Estructura del Archivo de Variables

El archivo `vars/datos_generales.yml` contiene las siguientes configuraciones:

### Configuración de Repositorios

Cada repositorio tiene las siguientes variables:

- `use_repo`: Booleano para habilitar (`true`) o deshabilitar (`false`) el uso del repositorio.
- `token`: Token de acceso para el repositorio.
- `user`: Nombre de usuario del repositorio.
- `host`: Host del repositorio.
- `repo`: Nombre del repositorio.

### Configuración del Proxy

El proxy tiene las siguientes variables:

- `use_proxy`: Booleano para habilitar (`true`) o deshabilitar (`false`) el uso del proxy.
- `host`: Dirección del host del proxy.
- `port`: Puerto del proxy.

### Ejemplo de `vars/datos_generales.yml`

```yaml
gitea_repo:
  use_repo: true
  token: "9a4fb0610c684bebf227378cd425be9dc72950ba"
  user: "ssanchezhlg"
  host: "github.hlg.sld.cu"
  repo: "testin_clonado.git"

github_vps_repo:
  use_repo: true
  token: "35a36576a7b1fa94c781a2dc50c8334db87196a7"
  user: "ssanchezhlg"
  host: "git.softnet.cu"
  repo: "testin_clonado.git"

github_oficial_repo:
  use_repo: true
  token: "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"
  user: "ssanchezhlg"
  host: "github.com"
  repo: "testin_clonado.git"

proxy:
  use_proxy: true
  host: "10.10.10.217"
  port: 3128
```

## Uso del Playbook

### Paso 1: Clonar el Repositorio

```bash
git clone https://tu-repositorio.git
cd tu-repositorio
```

### Paso 2: Configurar Variables

Edite el archivo `vars/datos_generales.yml` para proporcionar los valores adecuados para sus repositorios y configuración del proxy.

### Paso 3: Ejecutar el Playbook

Ejecute el siguiente comando para ejecutar el playbook:

```bash
ansible-playbook -i hosts playbook.yml
```

### Hosts

Asegúrese de tener un archivo de inventario `hosts` configurado con los hosts necesarios.

## Detalle del Playbook

El playbook realiza las siguientes tareas:

1. **Configurar Proxy (si está habilitado)**:
   Si el proxy está habilitado (`proxy.use_proxy: true`), configura las variables de entorno `http_proxy` y `https_proxy`.

2. **Clonar el Repositorio desde el Servidor Local**:
   Clona el repositorio desde el servidor local (`gitea_repo`) si está habilitado (`gitea_repo.use_repo: true`).

3. **Agregar el Repositorio de GitHub Alojado en el VPS**:
   Configura un remoto adicional para el repositorio alojado en el VPS (`github_vps_repo`) si está habilitado (`github_vps_repo.use_repo: true`).

4. **Agregar el Repositorio de GitHub Oficial**:
   Configura un remoto adicional para el repositorio oficial de GitHub (`github_oficial_repo`) si está habilitado (`github_oficial_repo.use_repo: true`).

5. **Commit y Push de Cambios al Repositorio en el VPS**:
   Realiza un commit y push de los cambios al repositorio alojado en el VPS (`github_vps_repo`) si está habilitado (`github_vps_repo.use_repo: true`).

6. **Commit y Push de Cambios al Repositorio Oficial**:
   Realiza un commit y push de los cambios al repositorio oficial de GitHub (`github_oficial_repo`) si está habilitado (`github_oficial_repo.use_repo: true`).

7. **Eliminar la Carpeta Temporal**:
   Elimina la carpeta temporal `/tmp/repo`.

## Para usarlo con Semaphore

```yaml
---


```
