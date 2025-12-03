# 04 - MITRE Caldera — Despliegue en Docker (Servidor de ataques)

> MITRE Caldera es una plataforma para ejecutar simulaciones de adversarios (ATAQUES automatizados) basados en MITRE ATT&CK.

> Es fundamental para tu Mini SOC porque genera actividad realista en los endpoints Linux y Windows para luego analizarla en Splunk.

### Vamos a desplegar Caldera MITRE en Docker, para evitar problemas de compatibilidad y conflictos con dependencias.

> Beneficios: aislamiento, reproducibilidad y fácil limpieza si algo falla.

## 5.0 Requisitos previos (en el Ubuntu Server donde vas a correr Docker)
- Ubuntu Server con al menos 2–4 GB RAM disponible para el contenedor (más si vas a atacar muchas máquinas).  


<img width="647" height="63" alt="free memory" src="https://github.com/user-attachments/assets/8912739a-0962-4901-878c-39e8899d67c0" />



---

## 5.1 Instalar Docker Engine (comandos + explicación)


# Actualizar lista de paquetes
```
sudo apt update
```
# Instalar paquetes que permiten usar repos apt sobre HTTPS
```
sudo apt install -y ca-certificates curl gnupg lsb-release
```
# Añadir la clave GPG oficial de Docker
```
sudo mkdir -p /etc/apt/keyrings
```
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```
# Añadir el repo estable de Docker
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
# Actualizar e instalar docker engine y docker-compose plugin
```
sudo apt update
```
```
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

`apt install ca-certificates ...`: prepara el sistema para repositorios HTTPS.

`gpg --dearmor`: guarda la clave de Docker para verificar paquetes.

`docker-ce docker-compose-plugin`: instala Docker y el plugin docker compose.

<img width="395" height="34" alt="docker version" src="https://github.com/user-attachments/assets/174cca83-fb9b-44e1-95a1-0c3f21e012c5" />



---

Verificar Docker funciona:
```
sudo docker run --rm hello-world
```
Esto descarga y ejecuta un contenedor de prueba; confirma que Docker está operativo.

<img width="636" height="450" alt="docker hello world" src="https://github.com/user-attachments/assets/d4b7ed2d-c1cc-4428-9ebf-6268a05d1636" />



---

## 5.2 Clonar Caldera 4.2.0 (fuente estable para el contenedor)
```
cd /opt
```
```
sudo git clone --branch 4.2.0 https://github.com/mitre/caldera.git caldera-4.2
```
```
sudo chown -R $USER:$USER caldera-4.2.0
```
```
cd caldera-4.2
```
### Clonamos la rama 4.2 (evitamos la v5 que requiere build frontend).

`chown` para que tu usuario pueda construir la imagen sin sudo todo el tiempo.

<img width="1003" height="87" alt="archivos caldera4 2" src="https://github.com/user-attachments/assets/f894fad7-cb1a-4dc7-b697-d79ebc87f396" />


---

## 5.3 Crear Dockerfile para Caldera 4.2
> NOTA: Al clonar el repo encontramos un Dockerfile, no borrarlo, crear uno nuevo con otro nombre, ie: Dockerfile.lab
### Crea el archivo Dockerfile dentro de /opt/caldera-4.2 con este contenido:
```
#Dockerfile para Caldera 4.2 (base Python slim)
FROM python:3.10-slim

#Variables de entorno (evitan prompts)
ENV PYTHONUNBUFFERED=1
WORKDIR /opt/caldera

#Dependencias del sistema necesarias
RUN apt-get update && apt-get install -y python3-dev git build-essential && rm -rf /var/lib/apt/lists/*

#Copiar el código (si construís desde local) o clonar en el build
COPY . /opt/caldera

#Crear y activar venv, instalar dependencias
RUN python3 -m venv /opt/venv && \
    /opt/venv/bin/pip install --upgrade pip setuptools wheel && \
    /opt/venv/bin/pip install -r /opt/caldera/requirements.txt

#Exponer puerto HTTP de Caldera
EXPOSE 8888

#Comando por defecto para iniciar Caldera (modo inseguro para lab)
CMD ["/opt/venv/bin/python", "/opt/caldera/server.py", "--insecure"]

```
`python:3.10-slim`: imagen base ligera.

`COPY . /opt/caldera`: copia tu repo (con la rama 4.2) al container.

Se crea un venv y se instalan requerimientos dentro del contenedor.

`CMD` arranca Caldera en modo --insecure (solo para laboratorio).

<img width="1280" height="800" alt="dockerfile lab" src="https://github.com/user-attachments/assets/f0a97066-c228-4eec-b237-4af340125ee1" />


---

## 5.4 Crear docker-compose.lab.yml (opcional pero recomendado)

### Archivo docker-compose.lab.yml en /opt/caldera-4.2:
```
version: "3.8"
services:
  caldera:
    build:
      context: .
      dockerfile: dockerfile.lab
    container_name: caldera4
    restart: unless-stopped
    ports:
      - "8888:8888"   # exponer UI de Caldera
    volumes:
      - ./data:/opt/caldera/data  # persistir datos de Caldera
    environment:
      - PYTHONUNBUFFERED=1
```

`build: .` construye la imagen desde el Dockerfile local.

Mapea puerto 8888 del container al host.

volumes guarda data para persistencia de campañas/agentes.

<img width="1268" height="223" alt="docker compose" src="https://github.com/user-attachments/assets/c47af2ec-b88a-46ba-8e7c-121b67566acf" />



---

## 5.5 Construir la imagen y levantar Caldera

## Desde /opt/caldera-4.2:

### Construir la imagen (puede tardar unos minutos)
```
sudo docker compose -f docker-compose.lab.yml --progress=plain build caldera
```
### Levantar el servicio en background
```
sudo docker compose -f docker-compose.lab.yml up -d
```

`docker compose build`: compila la imagen Docker con Caldera y sus dependencias.

`docker compose up -d`: levanta el servicio en segundo plano.



📸 [INSERTAR CAPTURA: salida de docker compose build con final "Successfully built" o similar]



Verificar que el container corre:



sudo docker ps --filter "name=caldera4"
📸 [INSERTAR CAPTURA: salida de docker ps mostrando caldera4 con STATUS Up]

5.6 Acceder a la UI de Caldera
Abrí en tu navegador (desde máquina host o donde tengas acceso a la IP del servidor):

cpp
Copy code
http://<IP_DEL_SERVIDOR>:8888
Credenciales por defecto (Caldera 4.2):

Usuario: red

Contraseña: admin

📸 [INSERTAR CAPTURA: Caldera UI en el navegador mostrando login o dashboard]

5.7 Logs y troubleshooting (comandos útiles)
Ver logs del contenedor:

bash
Copy code
sudo docker logs -f caldera4
-f sigue los logs en tiempo real.

Entrar al shell del contenedor (por si querés debug):

bash
Copy code
sudo docker exec -it caldera4 /bin/bash
# una vez dentro podés activar el venv y correr comandos:
source /opt/venv/bin/activate
python server.py --insecure
Parar / iniciar / destruir:

bash
Copy code
# Parar
sudo docker compose stop

# Iniciar
sudo docker compose start

# Bajar y eliminar containers (pero conservar imagen y volumen)
sudo docker compose down

# Borrar imagen construida (si querés limpiar)
sudo docker image rm caldera4
📸 [INSERTAR CAPTURA: salida de docker logs -f caldera4 mostrando "Caldera running on http://0.0.0.0:8888"]

5.8 Conectar Caldera a la red de laboratorio (Host-Only / Bridge)
Si querés que Caldera sea accesible sólo en la red interna (Host-Only), cambiá ports en docker-compose.yml por network_mode: "host" solo si el servidor corre directamente en la VM y no conflictúa con otros servicios.

Ejemplo alternativo (usar host networking):

yaml
Copy code
services:
  caldera:
    build: .
    network_mode: "host"
    ...
network_mode: host hace que el contenedor use la red del host; http://<host_ip>:8888 funcionará sin mapeo de puertos.

Usar con precaución (no exponer al internet).

5.9 Integración con Splunk (generar logs accionables)
Ejecutá campañas contra tus endpoints desde Caldera UI.

Los endpoints (UF + Sysmon) generarán logs que Splunk indexará.

Si Caldera y Splunk corren en la misma VM, no hay conflicto de puertos (Splunk 8000/9997, Caldera 8888).

Recomendación: probar una campaña pequeña y validar en Splunk con:

spl
Copy code
index=linux_logs OR index=windows_logs earliest=-15m
📸 [INSERTAR CAPTURA: Caldera ejecutando campaign + Splunk search mostrando eventos]

5.10 Limpieza / Recuperar espacio
Si querés parar y eliminar todo rápidamente:

bash
Copy code
sudo docker compose down --volumes --rmi local
--volumes elimina volúmenes persistentes de datos.

--rmi local elimina las imágenes construidas localmente.
