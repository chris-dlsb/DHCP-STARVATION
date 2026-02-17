# PoC: Ataque de Agotamiento DHCP (DHCP Starvation) con Scapy

![Status](https://img.shields.io/badge/Estado-Finalizado-green)
![Python](https://img.shields.io/badge/Python-3.x-blue)
![Scapy](https://img.shields.io/badge/Library-Scapy-yellow)

##  Descripción Técnica
Este repositorio contiene una Prueba de Concepto (PoC) desarrollada en Python utilizando el framework **Scapy**. 
**Objetivo:** Ejecutar una Denegación de Servicio (DoS) mediante la inundación de solicitudes DHCP DISCOVER con direcciones MAC falsificadas aleatorias. El objetivo es agotar el pool de direcciones IP disponibles en el servidor DHCP legítimo, impidiendo que nuevos clientes se conecten a la red.
El script demuestra vulnerabilidades críticas en la Capa 2 (Enlace de Datos) del modelo OSI, permitiendo auditar la seguridad de la infraestructura de red conmutada.

## Topología y Escenario

El entorno de pruebas fue desplegado utilizando **GNS3** con emulación de hardware Cisco (IOU) y máquinas virtuales atacantes.

| Dispositivo | Rol | IP / Interfaz | Detalles |
| :--- | :--- | :--- | :--- |
| **Kali Linux** | Atacante | `10.14.14.66` / `eth0` | Origen de la inyección de paquetes. |
| **Cisco IOU L3** | Gateway (Víctima) | `10.14.14.1` / `e0/0` | Router/Switch de borde. |
| **Cisco IOU L2** | Switch de Acceso | N/A (Capa 2) | Dispositivo donde se inyecta tráfico. |
| **VLAN** | Segmento | VLAN 1 (Nativa) | Red `10.14.14.0/24`. |


### Diagrama Lógico
*(Aquí debes insertar tu captura de pantalla de GNS3 con la topología)*
`![Topologia](./evidencias/topologia.png)`

##  Requisitos y Dependencias

Para ejecutar esta herramienta se requiere:
* **Sistema Operativo:** Linux (Kali Linux, Parrot OS, Ubuntu).
* **Python:** Versión 3.8 o superior.
* **Permisos:** Acceso **Root** (sudo) es mandatorio para la manipulación de sockets raw.
* **Librerías:**
    ```bash
    pip install scapy
    ```

##  Instalación y Uso

1.  **Clonar el repositorio:**
    ```bash
    git clone [https://github.com/tu-usuario/nombre-repo.git](https://github.com/tu-usuario/nombre-repo.git)
    cd nombre-repo
    ```

2.  **Ejecutar el script:**
    ```bash
    sudo python3 nombre_script.py
    ```

### Parámetros Configurados
* **Interfaz:** `eth0` (Hardcoded o por argumento, según tu script).
* **Target:** Broadcast `ff:ff:ff:ff:ff:ff` o Multicast STP `01:80:c2:00:00:00`.

## 📸 Evidencia de Funcionamiento (PoC)

**1. Ejecución del Ataque:**
*(Pega aquí captura de tu terminal Kali corriendo el script)*
`![Ejecucion](./evidencias/terminal.png)`

**2. Análisis de Tráfico (Wireshark):**
*(Pega aquí captura de Wireshark mostrando los paquetes maliciosos)*
`![Wireshark](./evidencias/wireshark.png)`

**3. Impacto en la Víctima:**
*(Pega aquí captura de la consola del Switch/Router mostrando el daño)*
`![Impacto](./evidencias/impacto.png)`

## 🛡️ Medidas de Mitigación

Para proteger la infraestructura contra este vector de ataque, se recomienda implementar:

[MITIGACIONES ESPECÍFICAS]:

Port Security: Configurar seguridad de puertos en el Switch para limitar el número máximo de direcciones MAC permitidas por interfaz (ej. switchport port-security maximum 5).

DHCP Snooping: Habilitar DHCP Snooping globalmente y configurar límites de tasa (rate-limit) en puertos no confiables para descartar ráfagas excesivas de solicitudes DHCP.
---
*Descargo de Responsabilidad: Este software fue creado únicamente con fines académicos para la asignatura de Ciberseguridad del ITLA. El autor no se hace responsable del mal uso de esta herramienta.*

**Autor:** Cristopher De Los Santos  
**Matrícula:** 2024-1414
