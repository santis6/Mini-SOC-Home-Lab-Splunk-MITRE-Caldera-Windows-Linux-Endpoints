# 02 - Splunk Enterprise (Servidor) — Instalación & configuración básica

> Instrucciones para Ubuntu Server. Ejecuta los comandos como usuario con sudo.

---

## 2.0 Actualizar el sistema y preparar paquetes básicos

```bash
# Actualiza la lista de paquetes y versiones disponibles
sudo apt update
# Actualiza todos los paquetes instalados a sus últimas versiones (no interactivo)
sudo apt upgrade -y
# Instala utilidades necesarias (wget para descargas, net-tools para network info, curl/gnupg en caso)
sudo apt install -y wget net-tools curl unzip apt-transport-https gnupg
```
`apt update`: refresca la cache de paquetes.

`apt upgrade -y`: aplica actualizaciones; -y confirma automáticamente.

`apt install -y ...`: instala herramientas que vas a usar durante la instalación.

📸 [INSERTAR CAPTURA: Terminal mostrando apt update && apt upgrade -y]

## 2.1 Obtener la URL correcta y descargar Splunk (.deb)

### Ir a carpeta temporal
```
cd /tmp
```
# Descargar el paquete .deb de Splunk
```
wget -O splunk-10.0.2-e2d18b4767e9-linux-amd64.deb "https://download.splunk.com/products/splunk/releases/10.0.2/linux/splunk-10.0.2-e2d18b4767e9-linux-amd64.deb"
```

`cd /tmp`: trabajar en /tmp evita ensuciar otras carpetas.

`wget -O splunk.deb 'URL'`: descarga el instalador y lo guarda como splunk.deb.

📸 [INSERTAR CAPTURA: Terminal mostrando la descarga del .deb de Splunk]
