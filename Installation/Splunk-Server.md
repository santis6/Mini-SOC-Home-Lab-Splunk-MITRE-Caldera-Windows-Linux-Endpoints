# 02 - Splunk Enterprise (Servidor) — Instalación & configuración básica

> Instrucciones para el despliegue correcto del Ubuntu Server y la instalación de Splunk.

> Para evitar errores recomiendo el uso de `sudo` en todo los comandos que se apliquen en este apartado.

> En caso de errores, freeze de pantalla u otros problemas, consultar el apartado "Troubleshooting".
  


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

`apt upgrade -y`: aplica actualizaciones; `-y` confirma automáticamente.

`apt install -y ...`: instala herramientas que vas a usar durante la instalación.

<img width="733" height="190" alt="sudo apt upgrade update" src="https://github.com/user-attachments/assets/fa7457f8-c17a-4c31-bb02-eca52293e3d2" />

---

### 2.1 Obtener la URL correcta y descargar Splunk (.deb)
> En este apartado ingresar a la página web de Splunk, realizar el registro y descargar la última versión disponible.
#### Ir a carpeta temporal
```
cd /tmp
```
#### Descargar el paquete .deb de Splunk
```
wget -O splunk-10.0.2-e2d18b4767e9-linux-amd64.deb "https://download.splunk.com/products/splunk/releases/10.0.2/linux/splunk-10.0.2-e2d18b4767e9-linux-amd64.deb"
```

`cd /tmp`: trabajar en /tmp evita ensuciar otras carpetas.

`wget -O splunk.deb 'URL'`: descarga el instalador y lo guarda como splunk.deb.

<img width="1280" height="185" alt="descargando splunk" src="https://github.com/user-attachments/assets/398f8e28-b5e4-4ac6-8882-1a0bd2fe7104" />

---

### 2.2 Instalar Splunk (.deb) y resolver dependencias
#### Instala el paquete .deb con dpkg (puede mostrar advertencias sobre dependencias)
```
sudo dpkg -i /tmp/splunk-10.0.2-e2d18b4767e9-linux-amd64.deb
```
#### Si dpkg reporta dependencias faltantes, arreglalas con apt
```
sudo apt --fix-broken install -y
```

`dpkg -i`: instala paquetes .deb; no resuelve dependencias automáticamente.

`apt --fix-broken install -y`: descarga e instala dependencias faltantes y completa la instalación.

<img width="726" height="195" alt="instalacion splunk" src="https://github.com/user-attachments/assets/160cf8cc-709b-4f79-a9c1-dac994f14cac" />

---

### 2.3 Iniciar Splunk por primera vez y aceptar la licencia
#### Inicia splunk por primera vez y acepta la licencia interactiva (si no hay UI)

```
sudo /opt/splunk/bin/splunk start --accept-license
```

Esto arranca splunkd, inicializa la instalación y suele pedir que crees o confirmes credenciales (puede completarse en la web UI).

>Si prefieres no ver prompts, iniciar con --accept-license evita el prompt de licencia.

<img width="1280" height="800" alt="iniciando splunk" src="https://github.com/user-attachments/assets/bbb31efd-7a8e-4856-b030-1e3551cd791b" />

---

### 2.4 Habilitar Splunk para que arranque automáticamente con el sistema

#### Habilita el servicio de Splunk para que se inicie automáticamente en cada arranque.

``` 
sudo /opt/splunk/bin/splunk enable boot-start
```

<img width="577" height="68" alt="enable boot start" src="https://github.com/user-attachments/assets/6c613fad-eab0-48c1-abe3-1b57d09976f5" />


---

### 2.5 Abrir puertos necesarios en el firewall (UFW)
#### Permitir acceso web UI de Splunk (puerto 8000)
```
sudo ufw allow 8000/tcp
```
#### Permitir puerto de recepción de forwarders (9997)
```
sudo ufw allow 9997/tcp
```
#### (Opcional) Ver estado de UFW y reglas
```
sudo ufw status verbose
```

`ufw allow 8000/tcp`: permite que accedas a la interfaz web.

`ufw allow 9997/tcp`: permite que Universal Forwarders envíen datos.

`ufw status verbose`: muestra reglas activas.

<img width="417" height="96" alt="reglas firewall" src="https://github.com/user-attachments/assets/9cf8dc1f-b780-44e6-9f83-8a747807c3fd" />

---


### 2.6 Verificar puertos escuchando y obtener IP del servidor
### Ver IP de la interfaz (útil para abrir en el navegador desde tu host)
```
ip -4 addr show 
```
#### Verificar que splunk esté escuchando en los puertos habituales (8000, 8089, 9997)
```
sudo ss -tulpen | grep -E '8000|8089|9997' || sudo netstat -tulpen | grep -E '8000|8089|9997'
```

`ip -4 addr show ...`: imprime las IP IPv4 de la VM.

`ss -tulpen | grep ...`: muestra sockets TCP/UDP escuchando y filtra los puertos de Splunk; netstat es alternativa.

<img width="1280" height="226" alt="ip y puertos escuchando" src="https://github.com/user-attachments/assets/553ed7d4-1229-46cb-878f-bbace1e26d4b" />

---

### 2.7 Acceder a la Web UI de Splunk

#### Abre en tu navegador desde el host:

http://<IP_DEL_SPLUNK>:8000


#### Login inicial: admin + contraseña que estableciste (o te la pide la primera vez).

Si la conexión es rechazada verificá la configuración de red de tu VM. Lo recomendable es *Host-Only Adapter*.
Sino revisá `sudo journalctl -u splunk` o `/opt/splunk/var/log/splunk/splunkd.log`.


<img width="1359" height="767" alt="interfaz web splunkl" src="https://github.com/user-attachments/assets/664d3fd9-69e6-4712-80a0-8b19edfd55a8" />


---

### 2.8 Comandos útiles de control y logs
#### Estado del servicio Splunk (comando propio)
```
sudo /opt/splunk/bin/splunk status
```
#### Reiniciar Splunk
```
sudo /opt/splunk/bin/splunk restart
```
#### Ver logs principales de splunkd (últimas 200 líneas)
```
sudo tail -n 200 /opt/splunk/var/log/splunk/splunkd.log
```

`splunk status`: estado del servicio Splunk CLI.

`splunk restart`: reinicia splunkd.

`tail ... splunkd.log`: revisar errores o avisos de arranque.

<img width="490" height="72" alt="splunk status" src="https://github.com/user-attachments/assets/2c6f1cde-c86c-4bcf-9272-66c875327c24" />


---
