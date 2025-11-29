# 🛡️ Mini SOC Home Lab — Splunk + MITRE Caldera + Windows & Linux Endpoints

Hola, mi nombre es Santiago Daniel Sandili. Bienvenido a mi laboratorio personal de Ciberseguridad, diseñado para practicar Blue Team, análisis de logs, detección de amenazas y automatización de ataques con MITRE CALDERA.  
El objetivo principal es simular un entorno realista con endpoints Windows y Linux que generen logs de actividad maliciosa de forma automática, y luego analizar todo eso dentro de Splunk Enterprise Free.

Este repositorio documenta la arquitectura, instalación, configuración, procedimientos y ataques simulados.

---

## 📌 Índice

- [🎯 Objetivo del Proyecto](#-objetivo-del-proyecto)
- [🌲 Arquitectura del Laboratorio](#-arquitectura-del-laboratorio)
  - [Rama 1 — Linux](#rama-1--linux)
  - [Rama 2 — Windows](#rama-2--windows)
- [🧩 Componentes del Laboratorio](#-componentes-del-laboratorio)
- [🚀 Flujo de Funcionamiento](#-flujo-de-funcionamiento)
- [📦 Requisitos](#-requisitos)
- [📚 Documentación Incluida](#-documentación-incluida)
- [🔐 Licencia](#-licencia)

---

## 🎯 Objetivo del Proyecto

Crear un entorno casero para poner en práctica conocimientos tanto teóricos como prácticos y también:

- Analizar ataques reales generados automáticamente con **MITRE Caldera**  
- Recolectar logs en **Splunk Enterprise Free (500MB/día)**  
- Practicar detección, análisis, dashboards y alertas  
- Simular un flujo completo de un SOC: *ataque → logs → ingestión → detección → respuesta*

Este laboratorio está pensado para estudiar Blue Team, SOC L1/L2, DFIR y MITRE ATT&CK.

---

## 🌲 Arquitectura del Laboratorio

El laboratorio se divide en dos ramas principales. Cada rama tiene un servidor con Splunk + Caldera y un endpoint víctima.

---

### 🚦 **Rama 1 — Linux**

- 🖥️ **Ubuntu Server**
  - Splunk Enterprise Free  
  - MITRE Caldera (ataques automatizados)
- 🧑‍💻 **Endpoint Linux**
  - Ubuntu Desktop
  - Splunk Universal Forwarder
  - Configuración Syslog

---

### 🔵 **Rama 2 — Windows**

- 🖥️ **Ubuntu Server**
  - Splunk Enterprise Free  
  - MITRE Caldera
- 🪟 **Endpoint Windows**
  - Windows 10 LTSC / Windows 11  
  - Splunk Universal Forwarder  
  - Sysmon + config de SwiftOnSecurity  
  - TA de Splunk para Sysmon

---

## 🧩 Componentes del Laboratorio

| Componente | Función |
|-----------|---------|
| **Splunk Enterprise Free** | Recolección, parsing y análisis de logs (500MB/día). |
| **Splunk Universal Forwarder** | Agente ligero para enviar logs a Splunk. |
| **MITRE Caldera** | Generación automática de ataques basados en MITRE ATT&CK. |
| **Sysmon** | Enriquecimiento de logs en Windows. |
| **Ubuntu Server** | Host principal (SIEM + Caldera). |

---

## 🚀 Flujo de Funcionamiento

1. Caldera ejecuta campañas de ataques programadas y automatizadas a los endpoints Linux y Windows.  
2. Los endpoints generan logs.  
3. Los agentes Universal Forwarder los envían al servidor.  
4. Splunk indexa, parsea y almacena.  
5. Se analizan eventos, creamos dashboards, reglas de correlación y detecciones.

---

## 📦 Requisitos

- VirtualBox / VMware
- 12–16 GB de RAM recomendados
- 120 GB de disco mínimo
- Archivos ISO:
  - Ubuntu Server
  - Ubuntu Desktop
  - Windows 10 LTSC (ideal) / Windows 11

---

## 📚 Documentación Incluida

- `Installation/` — Instalación completa del laboratorio  
- `Splunk-Setup/` — Configuración de Splunk  
- `Caldera-Setup/` — Instalación y campañas de MITRE Caldera  
- `Windows-Endpoint/` — Sysmon + agentes  
- `Linux-Endpoint/` — Agents + syslog  
- `Attack-Simulations/` — Pruebas, reportes y campañas  
- `Detection-Rules/` — Consultas, dashboards y reglas

---

## 🔐 Licencia

Este proyecto utiliza la licencia incluida dentro de este repositorio.  
Puedes revisarla en el archivo **LICENSE**.
