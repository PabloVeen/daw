Proyecto centrado en implantar un sistema de detección de intrusos en una red virtual simulada.

Busca identificar amenazas, registrar eventos y responder automáticamente ante actividades sospechosas.

Analiza riesgos de pérdida de datos, vulnerabilidades de red y problemas de seguridad física.

Utiliza Snort o Suricata para monitorizar tráfico y generar alertas en tiempo real.

Incluye scripting en Bash o Python para automatizar respuestas ante incidentes.

El entorno se basa en máquinas virtuales con Ubuntu Server, Kali Linux y herramientas de monitorización.

El proyecto se divide en fases de diseño, despliegue, configuración avanzada y validación final.

Permite cubrir numerosos resultados de aprendizaje relacionados con redes, seguridad, sistemas y automatización.



## PLAN de despliegue

1. Preparación del entorno de virtualización

Se crearán las siguientes máquinas virtuales:

Ubuntu Server → Servidor principal donde se instalará Snort/Suricata.

Kali Linux → Máquina atacante para pruebas de intrusión.

Router y switch simulados → Configurados desde la propia plataforma de virtualización.

Red configurada en modo Red interna para aislar el entorno.

Comandos iniciales en Ubuntu Server:

sudo apt update && sudo apt upgrade -y
sudo apt install net-tools curl wget -y

2. Instalación del Sistema de Detección de Intrusos (Snort o Suricata)
Instalación de Snort:
sudo apt install snort -y


Durante la instalación se indica la interfaz de red a monitorizar (ej: eth0).

Instalación de Suricata:
sudo apt install suricata -y


Comprobar que el servicio queda activo:

sudo systemctl status suricata

3. Configuración del SDI
Determinar la interfaz de red:
ip a


Editar el archivo de configuración (según la herramienta):

Suricata:

sudo nano /etc/suricata/suricata.yaml


Configurar:

Interfaz a monitorizar

Rutas de logs

Reglas activadas

Snort:

sudo nano /etc/snort/snort.conf


Añadir reglas personalizadas:

alert icmp any any -> any any (msg:"Ping detectado"; sid:1000001; rev:1;)


Reiniciar el servicio:

sudo systemctl restart suricata


o

sudo systemctl restart snort

4. Monitorización y registro

Activar la monitorización en tiempo real:

Suricata:

sudo tail -f /var/log/suricata/fast.log


Snort:

sudo snort -A console -q -c /etc/snort/snort.conf -i eth0

5. Pruebas de intrusión con Kali Linux

En la máquina atacantes se realizarán pruebas como:

Escaneo de puertos:
nmap -sS 192.168.1.10

Envío de paquetes ICMP:
ping 192.168.1.10

Generación de tráfico sospechoso:
hping3 -S -p 80 192.168.1.10


El SDI debe generar alertas visibles en los logs.

6. Verificación del sistema y documentación

El despliegue se validará comprobando:

Que el servicio detecta los ataques.

Que los logs se registran correctamente.

Que las reglas personalizadas se activan.

Que la red virtual funciona como se espera.
