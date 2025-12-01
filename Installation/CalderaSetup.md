# 04 - MITRE Caldera — Instalación en Ubuntu Server (Servidor de ataques)

> MITRE Caldera es una plataforma para ejecutar simulaciones de adversarios (ATAQUES automatizados) basados en MITRE ATT&CK.

> Es fundamental para tu Mini SOC porque genera actividad realista en los endpoints Linux y Windows para luego analizarla en Splunk.


## 4.2 Instalar dependencias necesarias

### Caldera requiere Python 3.8+ y otras utilidades.
```
sudo apt install -y python3 python3-venv python3-pip git
```

#### Instalamos Python 3, pip, virtualenv y Git para clonar el repositorio de Caldera.

<img width="1279" height="575" alt="instalacion pyth3 venv git pip" src="https://github.com/user-attachments/assets/3ca35cc5-4402-4e51-aaa8-c2c071e48789" />



--- 


## 4.3 Crear directorio para Caldera
```
mkdir ~/caldera
```
```
cd ~/caldera
```
#### Creamos un directorio para caldera, y nos movemos al mismo.

---

## 4.4 Clonar MITRE Caldera desde GitHub

```
git clone https://github.com/mitre/caldera.git
```


#### Con esto descargamos el código fuente oficial de Caldera en el servidor.

<img width="674" height="127" alt="git clone caldera" src="https://github.com/user-attachments/assets/8d7f94e0-2450-42db-84f1-4f4aca814718" />

---



## 4.5 Crear entorno virtual de Python
```
python3 -m venv venv
```

#### Creamos un entorno virtual aislado para evitar conflictos con otras aplicaciones del sistema.

---

## 4.6 Activar el entorno virtual
```
source venv/bin/activate
```

#### Activa el entorno virtual para instalar dependencias ahí dentro.

## 4.7 Instalar las dependencias de Caldera
```
pip3 install -r requirements.txt
```

#### Instala todas las librerías necesarias para que Caldera funcione.


<img width="867" height="314" alt="install requirements" src="https://github.com/user-attachments/assets/42568f3e-9404-4c74-826e-b3b90bf0ff0c" />

---

# 4.8 Ejecutar Caldera por primera vez

```
python3 server.py --insecure
```


Iniciamos el servidor Caldera.

--insecure: desactiva HTTPS para simplificar el laboratorio (solo recomendado en ambientes internos).

4.9 Acceder a la interfaz web

Una vez iniciado, verás algo así en la terminal:

Caldera running on http://0.0.0.0:8888
Default credentials: red / admin


📌 Acceder desde tu navegador:

http://IP_DEL_SERVIDOR:8888


Credenciales por defecto:

Usuario: red

Contraseña: admin

📸 Inserta una captura aquí
Captura recomendada:
Pantalla de login de Caldera en el navegador.

4.10 Crear un servicio systemd para que Caldera arranque fácil (opcional pero recomendado)

👇 Si querés iniciar Caldera sin escribir comandos largos.

Crear archivo de servicio:

sudo nano /etc/systemd/system/caldera.service


Pegar dentro:

[Unit]
Description=MITRE Caldera Server
After=network.target

[Service]
User=$USER
WorkingDirectory=/home/tu_usuario/caldera/caldera
ExecStart=/home/tu_usuario/caldera/caldera/venv/bin/python3 server.py --insecure
Restart=always

[Install]
WantedBy=multi-user.target


(Reemplazar tu_usuario por tu usuario real de Ubuntu.)

Activar el servicio:
sudo systemctl daemon-reload
sudo systemctl enable caldera
sudo systemctl start caldera


¿Qué hace?

daemon-reload: recarga servicios.

enable: arranca con el sistema.

start: inicia Caldera ahora mismo.

4.12 Verificar estatus del servicio
sudo systemctl status caldera


¿Qué hace?

Muestra si Caldera está corriendo sin errores.

📸 Inserta una captura aquí
Captura recomendada: salida OK del status (Active: running).

4.13 Confirmar conectividad con endpoints
Desde un endpoint Linux/Windows:
ping <IP_DEL_SERVIDOR_CALDERA>


¿Qué hace?

Comprueba que el endpoint puede alcanzar al servidor Caldera para ejecutar agentes.
