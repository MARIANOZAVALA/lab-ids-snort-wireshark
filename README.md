# lab-ids-snort-wireshark
Laboratorio de detección de intrusiones con Snort e IDS
# 🛡️ Laboratorio de Ciberseguridad: IDS y Análisis de Tráfico
**Enfoque: Blue Team | Monitoreo SOC | Detección de Intrusiones**

## 🎯 Objetivo
Desarrollar un entorno controlado para el monitoreo, detección y mitigación de amenazas de red. Este laboratorio se centra en la configuración de un Sistema de Detección de Intrusiones (IDS) para identificar patrones de ataque comunes y optimizar la respuesta ante incidentes mediante la afinación (*tuning*) de firmas.

## 🛠️ Stack Tecnológico
- **Sistema Operativo:** Kali Linux (Entorno de pruebas)
- **Detección de Intrusiones:** [Snort](https://www.snort.org/)
- **Análisis de Paquetes:** Wireshark
- **Generación de Tráfico/Ataque:** Nmap, hping3, curl
- **Virtualización:** Hyper-V

## 🚀 Desafíos y Detecciones Implementadas

### 1. Detección de Escaneos Evasivos (Xmas Scan)
- **Escenario:** Identificación de paquetes con banderas TCP inusuales (`FIN`, `PSH`, `URG`) utilizados para evadir firewalls tradicionales.
- **Regla Snort:** `alert tcp any any -> any any (msg:"Alerta: Escaneo Navideno (Xmas) detectado"; flags:FPU; sid:1000003;)`

### 2. Mitigación de Fuerza Bruta y DoS (hping3)
- **Escenario:** Monitoreo de inundaciones de paquetes SYN (SYN Flood) mediante el uso de filtros de detección matemáticos.
- **Optimización:** Implementación de `detection_filter` para alertar únicamente tras superar un umbral de 10 intentos en 5 segundos, reduciendo el ruido en el SOC.
- **Regla Snort:** `alert tcp any any -> any 22 (msg:"Alerta CRITICA: Fuerza Bruta SSH"; flags:S; detection_filter:track by_src, count 10, seconds 5; sid:1000004;)`

### 3. Cacería de Amenazas tras Proxy (X-Forwarded-For)
- **Escenario:** Análisis de cabeceras HTTP para identificar la IP real de un atacante cuando el tráfico es canalizado a través de un servidor proxy o balanceador de carga.
- **Lógica:** Inspección del campo `X-Forwarded-For` en el payload de paquetes del puerto 80.

### 4. Tuning y Reducción de Falsos Positivos
- **Metodología:** Aplicación de técnicas de supresión y comentarios de reglas redundantes para evitar la "fatiga de alertas" y asegurar que el analista se enfoque en incidentes críticos.

## 🧠 Conceptos Aplicados
- **IDS vs IPS:** Diferenciación entre el monitoreo pasivo (alertas) y la protección activa (bloqueo de paquetes).
- **Análisis Forense:** Inspección del *TCP Three-Way Handshake* para validar conexiones legítimas vs. intentos de escaneo.
- **Arquitectura de Red:** Segmentación y visibilidad en interfaces de bucle (*loopback*) vs. interfaces físicas.

## 📸 Evidencia Técnica
![Alerta Snort](screenshots/snort-alerta.png)
*Captura de pantalla: Snort detectando ráfaga de ataques hping3 en tiempo real.*

![Wireshark](screenshots/wireshark.png)
*Análisis en Wireshark: Inspección de banderas FPU en un paquete Xmas Scan.*
