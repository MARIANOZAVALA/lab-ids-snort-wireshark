# 🕵️‍♂️ Semana 3: Intercepción de Tráfico Inseguro y Análisis Forense (HTTP)

## 🎯 Objetivo
Auditar el tráfico de red de aplicaciones web vulnerables para demostrar los riesgos críticos asociados al uso de protocolos sin cifrado (HTTP) frente a la intercepción de credenciales.

## 🛠️ Herramientas Utilizadas
* **Ofensiva / Sensor:** Wireshark (Captura de paquetes en modo promiscuo).
* **Víctima:** Servidor web vulnerable (DVWA en Metasploitable 2).

## 🚀 Acciones Técnicas Realizadas
1. **Filtro de Telemetría:** Se configuró Wireshark para aislar peticiones de envío de formularios web (`http.request.method == "POST"`).
2. **Generación de Tráfico:** Se emuló un inicio de sesión administrativo estándar hacia el portal web de la máquina víctima a través del puerto 80.
3. **Análisis de Capa de Aplicación:** Se inspeccionó el payload del paquete TCP interceptado, decodificando el `HTML Form URL Encoded`.
4. **Validación del Riesgo:** Se comprobó la exposición de variables críticas, logrando extraer el nombre de usuario (`admin`) y la contraseña (`password`) en texto plano absoluto.

## 📸 Evidencia Forense
*(Captura del paquete POST revelando credenciales sin cifrado en Wireshark)*
![Credenciales Interceptadas](<img width="788" height="675" alt="wireshark_http_creds" src="https://github.com/user-attachments/assets/3a922207-69a5-4048-9b8d-a18896cbf4af" />.png)
