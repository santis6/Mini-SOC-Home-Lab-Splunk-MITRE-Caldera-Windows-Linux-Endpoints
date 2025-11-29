# 🛠️ Instalación - Mini SOC (Splunk + MITRE Caldera + Endpoints Linux & Windows)

---

# 1 - Resumen y requisitos

**Objetivo:** montar dos ramas del lab (Linux focus y Windows focus). Cada rama tiene un Ubuntu Server que corre Splunk Enterprise Free y Caldera, y un endpoint (Linux o Windows) que envía logs al Splunk. Caldera lanza ataques automatizados contra los endpoints para generar eventos.

**Requisitos mínimos (recomendado para todo el lab corriendo simultáneamente):**
- Host: 16 GB RAM (12 GB mínimo si no corres ambas ramas simultáneamente)  
- CPU: 6 cores (o 4 si vas a usar menos VMs)  
- Disco: 120 GB (SSD recomendado)  
- Virtualizador: VirtualBox o VMware Workstation/Player  
- ISOs: Ubuntu Server LTS (22.04 o 24.04), Ubuntu Desktop (opcional para endpoint Linux) y Windows 10/11 LTSC image

**Notas:**
- Splunk Free: límite de indexación **500 MB/día**. Monitorea tu volumen, pero en el caso de este laboratorio debería ser más que suficiente el limite de la licencia.

---
