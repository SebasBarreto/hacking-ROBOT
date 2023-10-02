Informe de Explotación de Vulnerabilidades: Reto Monkey - Semana 4
Introducción

Este informe documenta el análisis, explotación y escalación de privilegios en la máquina MONKEY, que forma parte del reto de Ethical Hacking. El objetivo del ejercicio fue la obtención de dos banderas ocultas en el sistema, lo que permitió demostrar competencias en reconocimiento de red, análisis de vulnerabilidades y explotación manual de servicios en un entorno controlado. El proceso detallado cubre desde la fase inicial de descubrimiento hasta la obtención de las banderas, incluyendo la explotación de vulnerabilidades y la escalación de privilegios.
Paso 1: Reconocimiento

La fase de reconocimiento fue crucial para obtener la información necesaria sobre la máquina objetivo.

    Identificación de la máquina objetivo:
        Herramientas utilizadas:
            ifconfig: Para obtener la dirección IP de la máquina atacante (Kali).
            arp-scan -l: Para identificar dispositivos en la red y sus direcciones MAC.
        Resultado: Se identificó la dirección IP de la máquina objetivo como 192.168.189.135. Se estimó que el sistema operativo es de tipo Linux/Unix.

    Escaneo de puertos:
        Herramienta utilizada: nmap
        Comando:

        nmap -sV -sVC 192.168.189.135

        Puertos abiertos identificados:
            21/tcp (FTP): Servidor FTP, potencialmente con acceso a archivos.
            22/tcp (SSH): Servicio SSH, posible acceso remoto con credenciales válidas.
            80/tcp (HTTP): Servidor web, posible puerta de entrada a través de vulnerabilidades en la aplicación web.

Paso 2: Análisis de Vulnerabilidades

En esta fase se realizó la enumeración de servicios y se evaluaron posibles vulnerabilidades.

    Enumeración de servicios:
        Se utilizó nmap con opciones avanzadas para identificar versiones de los servicios y detectar posibles vulnerabilidades.
        Resultado: No se encontraron exploits automatizados relevantes para las versiones de los servicios disponibles.

    Exploración web:
        Herramienta utilizada: Dirbuster
        Se realizó un escaneo de directorios web para identificar rutas ocultas en el servidor HTTP.
        Resultado: Se identificaron directorios adicionales como phpmyadmin y monkey, lo cual sugiere que el servidor podría tener aplicaciones web con posibles vulnerabilidades de configuración.

Paso 3: Explotación

En esta fase se explotan las vulnerabilidades detectadas para obtener acceso no autorizado al sistema.

    Acceso inicial (FTP):
        Se utilizó FTP con credenciales predeterminadas para obtener acceso inicial al sistema.
        Se descubrió un archivo con una contraseña cifrada, que posteriormente se descifró como junior01, lo que permitió avanzar en la explotación.

    Ataque de fuerza bruta (Web):
        Herramienta utilizada: Burp Suite con la opción Cluster Bomb.
        Se ejecutó un ataque de fuerza bruta en el panel de administración web (ip/monkey/index.php).
        Resultado: Se logró obtener acceso al panel de administración, lo que proporcionó una puerta de entrada adicional para la explotación.

    Ejecución de shell reverso:
        Técnica utilizada: Aprovechamiento de una funcionalidad de "subir imagen" en el panel de administración web.
        Se subió un archivo malicioso que permitió ejecutar un shell reverso, obteniendo acceso al sistema a través del servidor HTTP.

Paso 4: Escalación de Privilegios

Una vez obtenido el acceso inicial con privilegios limitados, se procedió a la escalación de privilegios.

    Acceso SSH:
        Con las credenciales obtenidas en la fase anterior (hackermentor:M1_P4ssw0rd_segur@), se logró acceder al servidor mediante SSH.
        Resultado: Se obtuvo acceso con privilegios limitados al sistema.

    Modificación de script para escalación:
        Durante la revisión del sistema, se identificó un script ejecutado cada 60 segundos por el usuario root.
        Se modificó el script para incluir comandos que permitieran escalar privilegios, logrando finalmente acceso completo al sistema y obteniendo la segunda bandera.

Paso 5: Obtención de las Banderas

Para completar el reto, se realizaron las siguientes acciones:

    Comando utilizado para localizar las banderas:
        Se ejecutó el siguiente comando para buscar las banderas en el sistema:

        find / -name bandera*.txt 2>/dev/null

    Ubicación y contenido de las banderas:
        Bandera 1: Ubicada en ~/bandera1.txt con el contenido: 47ee0702e489445bae251df46bc88b73.
        Bandera 2: Ubicada en /root/bandera2.txt con el contenido: d844ce556f834568a3ffe8c219d73368.

Herramientas Utilizadas

Las herramientas clave empleadas en este ejercicio fueron las siguientes:

    Reconocimiento y escaneo de red: ifconfig, arp-scan, nmap.
    Exploración web: Dirbuster, Burp Suite.
    Explotación: crackmapexec, ssh.

Conclusiones

    Éxitos:
        Se lograron identificar y explotar varias vulnerabilidades en los servicios web y de red de la máquina objetivo.
        La escalación de privilegios fue exitosa a través de la modificación de un script ejecutado por el usuario root, lo que permitió obtener acceso completo al sistema.

    Lecciones Aprendidas:
        La importancia de proteger servicios predeterminados como FTP y las páginas administrativas (e.g., phpMyAdmin) para prevenir accesos no autorizados.
        La necesidad de monitorear y auditar scripts automatizados y procesos que se ejecutan con privilegios elevados, ya que pueden ser utilizados para escalar privilegios.

Recomendaciones

    Mitigación de Vulnerabilidades:
        FTP y PHPMyAdmin deben ser actualizados, protegidos mediante autenticación fuerte o deshabilitados si no son necesarios.
        Es crucial eliminar o cambiar las credenciales predeterminadas y proteger las configuraciones de acceso a servicios críticos.

    Prácticas de Seguridad:
        Implementar monitoreo constante de scripts y procesos críticos que se ejecutan con privilegios elevados.
        Realizar auditorías periódicas y pruebas de penetración para detectar configuraciones inseguras y posibles vectores de ataque.

Análisis Final

Este informe demuestra un enfoque sistemático y detallado para la explotación de una máquina virtual en un entorno de prueba de penetración. El uso de herramientas conocidas como nmap, Burp Suite y Dirbuster ha sido clave para la identificación de vulnerabilidades. Además, la ejecución de un shell reverso a través de una carga maliciosa y la posterior escalación de privilegios mediante la modificación de un script evidencian un conocimiento profundo de las técnicas de explotación y escalación de privilegios.

Este tipo de informe puede servir como base para auditorías de seguridad reales en entornos de producción, destacando áreas críticas donde los sistemas pueden ser susceptibles a ataques si no se implementan las medidas de mitigación adecuadas.