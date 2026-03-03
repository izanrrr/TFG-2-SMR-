# Laboratorio de Seguridad: Detección y Respuesta Automatizada (SOAR)

Este proyecto implementa un laboratorio de seguridad básico pero completo, diseñado para detectar actividades maliciosas en una red, centralizar los registros y ejecutar respuestas automáticas ante incidentes, simulando un entorno de un Centro de Operaciones de Seguridad (SOC).

## 🎯 Objetivo

El objetivo principal es construir un sistema que sea capaz de:
1.  **Detectar** ataques de red en tiempo real.
2.  **Centralizar** todas las alertas y eventos en un único punto.
3.  **Responder** de forma automática y aislada para contener la amenaza sin intervención manual.

## 🏗️ Arquitectura del Laboratorio

El laboratorio se compone de tres máquinas virtuales interconectadas en una red interna aislada (`10.0.0.0/8`):

*   **Máquina Atacante (Kali Linux - `10.0.0.1`):** Equipo utilizado para lanzar ataques y generar tráfico malicioso con el fin de probar el sistema.
*   **Máquina Víctima (Debian/Ubuntu - `10.0.0.3`):** El objetivo de los ataques. Monitorizada por un agente que envía sus logs al servidor central.
*   **Servidor de Defensa (Debian/Ubuntu - `10.0.0.2`):** El cerebro de la operación. Aloja las siguientes herramientas:
    *   **Wazuh:** Plataforma SIEM/XDR para la centralización de logs, correlación de eventos y generación de alertas.
    *   **Suricata:** Sistema de Detección de Intrusiones en Red (IDS/NIDS) que analiza el tráfico en busca de firmas de ataque.
    *   **Ansible:** Herramienta de automatización (SOAR) que ejecuta las acciones de respuesta.

## ⚙️ Flujo de Trabajo

El sistema sigue un ciclo de detección y respuesta automatizado:

1.  **Detección:**
    *   **Suricata** monitoriza el tráfico de red entre el atacante y la víctima, generando alertas al detectar patrones maliciosos (ej. escaneos de puertos).
    *   El **Agente Wazuh** en la máquina víctima monitoriza los eventos locales (ej. intentos de login fallidos) y los envía al servidor Wazuh.

2.  **Centralización y Análisis:**
    *   El **Servidor Wazuh** recibe y correlaciona las alertas de Suricata y del agente, presentándolas en una interfaz unificada.

3.  **Respuesta Automatizada (SOAR):**
    *   Al detectarse una alerta crítica, Wazuh activa una respuesta automática a través de **Ansible**.
    *   **Ansible** se conecta a la máquina víctima y ejecuta una acción de contención, como bloquear la dirección IP del atacante en su firewall local.

## 📦 Tecnologías Clave

*   **Virtualización:** VirtualBox
*   **Sistemas Operativos:** Kali Linux, Debian/Ubuntu Server
*   **SIEM/XDR:** Wazuh
*   **IDS/NIDS:** Suricata
*   **Automatización (SOAR):** Ansible 
*   **Análisis de IA (Posible Ampliación):** Script en Python para el análisis contextual de alertas.

## 🚀 Cómo Empezar

1.  Clona este repositorio.
2.  Despliega las máquinas virtuales según la topología de red descrita.
3.  Sigue los guiones de instalación y configuración de cada herramienta (Wazuh, Suricata, Ansible).
4.  Ejecuta los playbooks de Ansible para preparar las máquinas.
5.  Lanza un ataque desde la máquina Kali y observa el ciclo de detección y respuesta automatizado.

## 📚 Estructura del Proyecto
├── ansible/ # Playbooks de Ansible para automatización
│ ├── bloquear_ip.yml # Playbook para bloquear IPs maliciosas
│ └── inventory # Archivo de inventario de Ansible
├── wazuh/ # Configuraciones de Wazuh
│ └── ossec.conf # Archivo de configuración del manager
├── suricata/ # Configuraciones de Suricata
│ └── suricata.yaml # Archivo de configuración principal
└── docs/ # Documentación del proyecto
└── incidentes.md # Análisis de incidentes simulados

## 📄 Licencia
Este proyecto se ha desarrollado como parte de un trabajo académico sobre ciberseguridad.
