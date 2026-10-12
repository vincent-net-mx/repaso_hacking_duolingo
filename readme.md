# Guía de Estudio Integral: Hacking Ético y Fundamentos de Seguridad

---

## Módulo 0: Fundamentos y Conceptos Básicos

### 0.1 La Tríada CIA (y extensiones)
El pilar central de la seguridad de la información está compuesto por tres principios rectores, a menudo extendidos a cinco:

| Pilar | Definición | Amenaza / Ataque común | Mecanismo de Control |
| :--- | :--- | :--- | :--- |
| **Confidencialidad** | Garantizar que la información solo sea accesible por entidades autorizadas. | Eavesdropping, Man-in-the-Middle (MitM), fugas de datos. | Cifrado (AES, RSA), control de acceso (ACL, RBAC). |
| **Integridad** | Asegurar que los datos no sean modificados, alterados o eliminados sin autorización. | Alteración de paquetes, inyección SQL, tampering de archivos. | Funciones Hash (SHA-256), firmas digitales, checksums. |
| **Disponibilidad** | Garantizar que los sistemas, redes y datos estén accesibles cuando los usuarios legítimos los requieran. | Ataques DoS/DDoS, ransomware, fallos de infraestructura. | Redundancia, balanceadores de carga, planes de DRP/BCP. |
| **Autenticidad** | Verificación certera de la identidad del remitente o emisor. | Suplantación de identidad (Spoofing), phishing. | MFA, certificados X.509, Kerberos. |
| **No Repudio** | Imposibilidad de que una entidad niegue haber realizado una acción o transmitido un mensaje. | Desconocimiento de transacciones o firmas. | Firmas digitales criptográficas, bitácoras de auditoría firmadas. |

---

### 0.2 Las 7 Capas de la Ciberseguridad (Defensa en Profundidad)
El principio de **Defensa en Profundidad** (*Defense in Depth*) establece que ningún control individual es infalible; por ello, la seguridad se implementa en capas superpuestas:

```
[ Capa Humana ]
     └── [ Perimetral ]
          └── [ Red (Network) ]
               └── [ Dispositivos / Endpoint ]
                    └── [ Aplicación ]
                         └── [ Datos ]
                              └── [ Misión Crítica ]
```

1. **Capa Humana:** Políticas corporativas, concientización, capacitación contra ingeniería social y gestión de roles.
2. **Capa Perimetral:** Frontera externa de la organización con Internet. Firewalls de borde, routers de frontera, protección anti-DDoS y WAF.
3. **Capa de Red (*Network*):** Segmentación interna, VLANs, switches administrables, IDS/IPS de red (NIDS), VPNs internas y NAC (*Network Access Control*).
4. **Capa de Endpoint:** Protección de hosts finales (servidores, estaciones de trabajo, móviles). Soluciones EDR/XDR, parches de SO y hardening.
5. **Capa de Aplicación:** Prácticas de desarrollo seguro (SDLC), validación de entradas, sanitización y control de sesiones.
6. **Capa de Datos:** Cifrado en reposo (*Data at Rest*) y en tránsito (*Data in Transit*), políticas de DLP (*Data Loss Prevention*), respaldos y gestión de accesos a bases de datos.
7. **Capa de Misión Crítica:** Activos esenciales del negocio (procesos clave, propiedad intelectual, infraestructura crítica de la organización).

---

### 0.3 Taxonomía del Malware
Software malicioso diseñado con fines perjudiciales:

* **Troyano (*Trojan*):** Programa aparentemente legítimo u ofensivo que oculta una carga útil maliciosa en segundo plano. No se autorreplica.
* **Gusano (*Worm*):** Malware con capacidad de autorreplicación que se propaga automáticamente por la red aprovechando vulnerabilidades sin interacción del usuario.
* **Adware:** Software que despliega publicidad no deseada de forma invasiva, con frecuencia acoplado a trackers de comportamiento.
* **Spyware / Keylogger:** Diseñado para recolectar información sensible (pulsaciones de teclado, credenciales, historial) sin consentimiento.
* **Rootkit:** Conjunto de herramientas orientadas a ocultar la presencia de procesos, puertos o archivos maliciosos, operando habitualmente en espacio de kernel (Ring 0) o con altos privilegios.
* **Ransomware:** Cifra los archivos del sistema víctima mediante algoritmos criptográficos robustos (p. ej., híbrido RSA + AES) y exige un rescate monetario a cambio de la llave de descifrado.
* **Botnet:** Red de equipos comprometidos (zombies) controlados remotamente por un servidor de Comando y Control (C2), comúnmente utilizados para ataques DDoS masivos.

---

### 0.4 Clasificación de Hackers

* **White Hat (Sombrero Blanco):** Profesionales de seguridad y pentesters que operan bajo autorización expresa, contratos de confidencialidad y apego legal para hallar fallas y remediarlas.
* **Grey Hat (Sombrero Gris):** Actúan sin autorización explícita para descubrir vulnerabilidades, pero generalmente sin dolo destructivo; suelen divulgar la falla a la víctima o al público posterior al hallazgo.
* **Black Hat (Sombrero Negro):** Actores maliciosos que operan al margen de la ley con fines de lucro, espionaje, extorsión o sabotaje.
* **Otros perfiles relevantes:**
  * *Script Kiddies:* Atacantes con bajo nivel técnico que emplean herramientas y scripts automatizados de terceros.
  * *Hacktivistas:* Atacantes motivados por ideologías políticas, sociales o religiosas.
  * *Nation-State / APT (Advanced Persistent Threats):* Grupos con recursos masivos respaldados por estados-nación orientados al espionaje y ciberguerra prolongada.

---

## Módulo 1: Frameworks y Metodologías de Seguridad

### 1.1 Cyber Kill Chain (Lockheed Martin)
Modelo secuencial que describe las fases de un ciberataque dirigido:

```
[Reconocimiento] ➔ [Armamento] ➔ [Entrega] ➔ [Explotación] ➔ [Instalación] ➔ [C2] ➔ [Acciones sobre Objetivos]
```

1. **Reconnaissance (Reconocimiento):** Búsqueda pasiva y activa de información sobre el objetivo (OSINT, escaneos).
2. **Weaponization (Armamento):** Creación del binario o documento malicioso acoplando el exploit con un payload/backdoor.
3. **Delivery (Entrega):** Transmisión del artefacto a la víctima (correo de phishing, USB, enlace malicioso).
4. **Exploitation (Explotación):** Ejecución del exploit al aprovechar una vulnerabilidad de software o humana.
5. **Installation (Instalación):** Persistencia en el host (creación de servicios maliciosos, tareas programadas, rootkits).
6. **Command & Control (C2):** Establecimiento de un canal bidireccional de comunicación con el atacante.
7. **Actions on Objectives:** Consecución de los fines del atacante (exfiltración de datos, destrucción, cifrado o pivoteo lateral).

---

### 1.2 MITRE ATT&CK
Base de conocimiento global sobre tácticas, técnicas y procedimientos (TTPs) de adversarios en entornos reales.
* **Táctica:** El *objetivo técnico* del atacante (el **¿Por qué?**), ej.: *Initial Access*, *Privilege Escalation*, *Lateral Movement*.
* **Técnica:** El método específico empleado para alcanzar dicho objetivo (el **¿Cómo?**), ej.: *Phishing (T1566)*, *Pass the Hash (T1550.002)*.
* **Procedimiento:** La implementación técnica concreta o herramientas usadas por un grupo particular (ej., grupo APT29 usando PowerShell con ciertos argumentos).

---

### 1.3 NIST Cybersecurity Framework (CSF)
Marco estructurado alrededor de funciones clave para la gestión continua del riesgo de ciberseguridad:
* **Identify (Identificar):** Gestión de activos, entorno empresarial, gobernanza y evaluación de riesgos.
* **Protect (Proteger):** Control de acceso, concientización, seguridad de datos, mantenimiento defensivo.
* **Detect (Detectar):** Monitoreo continuo de eventos de seguridad y detección de anomalías.
* **Respond (Responder):** Planes de respuesta a incidentes, mitigación, análisis forense y comunicaciones.
* **Recover (Recuperar):** Restauración de operaciones, resiliencia y lecciones aprendidas.

---

### 1.4 OWASP (Open Web Application Security Project)
Comunidad dedicada a mejorar la seguridad en aplicaciones web.
* **OWASP Top 10:** Lista periódica de los riesgos de seguridad más críticos para aplicaciones web (e.g., Inyecciones, Autenticación rota, Fallas criptográficas, Configuraciones de seguridad incorrectas, Control de acceso defectuoso).
* **WSTG (Web Security Testing Guide):** Metodología estandarizada de pruebas técnicas para auditar la seguridad web.

---

### 1.5 PTES (Penetration Testing Execution Standard)
Estándar compuesto por 7 fases que estructuran una prueba de penetración profesional:
1. **Pre-engagement Interactions:** Delimitación de alcance (Scope), Reglas de Compromiso (RoE), NDA y acuerdos legales.
2. **Intelligence Gathering (OSINT):** Recolección de información sobre la red, dominios, infraestructura y empleados.
3. **Threat Modeling:** Definición y perfilado de vectores de ataque más probables.
4. **Vulnerability Analysis:** Identificación activa y pasiva de vulnerabilidades y configuraciones deficientes.
5. **Exploitation:** Ejecución controlada de exploits para comprobar la viabilidad del compromiso.
6. **Post-Exploitation:** Mantener persistencia, escalada de privilegios, recolección de evidencia y pivoteo (*Pivoting*).
7. **Reporting:** Redacción del informe ejecutivo (para directivos) y técnico (con pasos de remediación para ingenieros).

---

## Módulo 2: Redes Aplicadas al Hacking Ético

### 2.1 Modelos de Red: OSI vs. TCP/IP

| Capa OSI | Nombre | PDU | Funcionalidad Clave | Capa TCP/IP | Protocolos Típicos |
| :---: | :--- | :--- | :--- | :---: | :--- |
| **7** | Aplicación | Datos | Interfaz directa con aplicaciones de usuario. | Aplicación | HTTP, SSH, DNS, FTP, SMTP |
| **6** | Presentación | Datos | Formato, compresión y cifrado de datos. | Aplicación | SSL/TLS, JPEG, ASCII |
| **5** | Sesión | Datos | Gestión y control del diálogo/sesiones. | Aplicación | RPC, NetBIOS, Sockets |
| **4** | Transporte | Segmento / Datagrama | Entrega extremo a extremo, control de flujo. | Transporte | TCP, UDP |
| **3** | Red | Paquete | Enrutamiento lógico y direccionamiento global. | Internet | IP (IPv4/IPv6), ICMP, ARP |
| **2** | Enlace de Datos | Trama (*Frame*) | Direccionamiento físico, control de acceso al medio. | Acceso a Red | Ethernet, Wi-Fi (802.11), MAC |
| **1** | Física | Bits | Transmisión de señales eléctricas/ópticas. | Acceso a Red | Cables, conectores, voltajes |

---

### 2.2 Protocolos de Transporte: TCP vs. UDP

```
           TCP 3-Way Handshake
Cliente                         Servidor
   |             SYN               |
   | ----------------------------> |
   |           SYN / ACK           |
   | <---------------------------- |
   |             ACK               |
   | ----------------------------> |
[Conexión establecida e intercambio de datos]
```

* **TCP (Transmission Control Protocol):**
  * **Orientado a conexión:** Requiere el *Three-Way Handshake* (SYN $\rightarrow$ SYN-ACK $\rightarrow$ ACK).
  * **Confiable:** Mecanismo de confirmación de paquetes (ACK), retransmisión por pérdida y control de congestión.
  * **Flags TCP relevantes para escaneo de puertos:**
    * `SYN`: Inicia conexión (usado en escaneo semiabierto / Stealth Scan: `-sS` en Nmap).
    * `ACK`: Confirma recepción (usado en `-sA` para mapear reglas de firewall).
    * `FIN`: Cierre ordenado de conexión.
    * `RST`: Reinicio inmediato de conexión rechazada.
    * `PSH`: Solicita envío inmediato de buffer sin esperar llenarlo.
    * `URG`: Datos urgentes prioritarios.
    * *Escaneo Xmas:* Envía flags `FIN, PSH, URG` activas simultáneamente.

* **UDP (User Datagram Protocol):**
  * **No orientado a conexión:** No negocia sesión; envía datagramas directamente (*best-effort*).
  * **Rápido y ligero:** Sin sobrecarga de control; no garantiza orden ni entrega.
  * Casos de uso: DNS, streaming, voz sobre IP (VoIP), DHCP.

---

### 2.3 Direccionamiento y Tipos de Direcciones

#### Direcciones IP Públicas vs. Privadas (RFC 1918)
Las IP privadas no son enrutables en el Internet público y requieren NAT (*Network Address Translation*):

| Clase | Rango Privado (RFC 1918) | Máscara por Defecto |
| :---: | :--- | :--- |
| **Clase A** | `10.0.0.0` a `10.255.255.255` | `/8` (`255.0.0.0`) |
| **Clase B** | `172.16.0.0` a `172.31.255.255` | `/12` (`255.240.0.0`) |
| **Clase C** | `192.168.0.0` a `192.168.255.255` | `/16` (`255.255.0.0`) |

#### Direcciones Reservadas Especiales
* **Loopback:** `127.0.0.0/8` (comúnmente `127.0.0.1`, `localhost`): Dirección interna del propio host.
* **APIPA (Automatic Private IP Addressing):** `169.254.0.0/16`: Asignada automáticamente cuando el cliente no recibe respuesta de un servidor DHCP.
* **Multicast:** `224.0.0.0` a `239.255.255.255` (Clase D).
* **Broadcast:** `255.255.255.255` (difusión en red local).

#### Direcciones MAC (Capa 2)
* Identificador físico único de 48 bits (6 bytes) en formato hexadecimal: `AA:BB:CC:DD:EE:FF`.
  * Primeros 24 bits: **OUI** (*Organizationally Unique Identifier*), identifica al fabricante de la tarjeta de red.
  * Últimos 24 bits: **NIC**, número de serie específico del dispositivo.
* **Vectores de ataque asociados:** ARP Spoofing/Poisoning, suplantación de MAC (*MAC Spoofing*), saturación de tabla CAM (*MAC Flooding*).

---

### 2.4 Puertos, Sockets y Servicios Críticos

* **Socket:** Combinación única de una Dirección IP y un Puerto (ejemplo: `192.168.1.100:443`). Identifica unívocamente un proceso en comunicación de red.
* **Rango de Puertos (0 a 65535):**
  * *Well-Known Ports:* `0 - 1023` (reservados para servicios del sistema y protocolos estándar).
  * *Registered Ports:* `1024 - 49151` (asignados por IANA para aplicaciones específicas).
  * *Dynamic / Private / Ephemeral Ports:* `49152 - 65535` (utilizados temporalmente por clientes para originar conexiones salientes).

#### Tabla de Puertos y Servicios Esenciales para Auditorías

| Servicio | Puerto | Transporte | Mecanismo de Seguridad / Vector de Auditoría |
| :--- | :---: | :---: | :--- |
| **FTP** | 20 / 21 | TCP | Tráfico en texto plano. Credenciales vulnerables a sniffing. Variante segura: **FTPS** o **SFTP** (vía SSH). |
| **SSH** | 22 | TCP | Canal seguro cifrado por defecto. Ataques de fuerza bruta, llaves privadas expuestas. |
| **Telnet** | 23 | TCP | Comunicación en texto claro sin cifrado. Obsoleto y crítico si se detecta. |
| **SMTP** | 25 / 587 | TCP | Envío de correos. Puerto 25 (relays abiertos, texto claro), 587 (autenticado/TLS). |
| **DNS** | 53 | UDP / TCP | Resolución de nombres. UDP para consultas habituales; TCP para transferencias de zona (`AXFR`). |
| **DHCP** | 67 (Srv) / 68 (Cli) | UDP | Asignación dinámica de IP. Vulnerable a ataques de Starvation y Rogue DHCP Servers. |
| **HTTP** | 80 | TCP | Tráfico web sin cifrar. Susceptible a interceptación y MitM. |
| **POP3** | 110 / 995 | TCP | Descarga de correos. 110 (texto claro), 995 (POP3S cifrado sobre TLS). |
| **NTP** | 123 | UDP | Sincronización de reloj. Empleado en ataques de amplificación DDoS (Monlist). |
| **IMAP** | 143 / 993 | TCP | Gestión de buzones de correo. 143 (claro), 993 (IMAPS con TLS). |
| **SNMP** | 161 / 162 | UDP | Monitoreo de red. SNMP v1 y v2c transmiten *Community Strings* en texto plano (ej. `public`, `private`). |
| **HTTPS** | 443 | TCP | HTTP sobre TLS. Inspección de certificados, cifrados débiles (SSLv3, TLS 1.0/1.1 deprecados). |
| **SMB** | 445 | TCP | Compartición de archivos en Windows. Historial de exploits críticos (EternalBlue/MS17-010, Null Sessions). |
| **RDP** | 3389 | TCP | Escritorio remoto de Microsoft. Foco común de ataques de fuerza bruta y exploits como BlueKeep (CVE-2019-0708). |

---

### 2.5 Dispositivos de Red y Seguridad

#### Dispositivos de Red
* **Switch (Capa 2 / Capa 3):** Reenvía tramas analizando la tabla CAM (relación MAC-puerto físico). Divide dominios de colisión, pero mantiene un solo dominio de broadcast a menos que se implementen VLANs.
* **Router (Capa 3):** Enruta paquetes entre distintas subredes analizando direcciones IP lógicas. Divide dominios de colisión y dominios de broadcast.

#### Dispositivos de Seguridad
* **Firewall:**
  * *Stateless (Sin estado):* Filtra paquetes individualmente con base en reglas estáticas (IP origen/destino, puertos) sin recordar el estado de la conexión previa.
  * *Stateful (Con estado):* Monitorea activamente el estado de la conexión en una tabla de estados; aprueba automáticamente paquetes que correspondan a flujos legítimos ya establecidos.
  * *WAF (Web Application Firewall):* Dispositivo o software de Capa 7 que inspecciona tráfico HTTP/HTTPS buscando patrones maliciosos específicos (XSS, SQLi, CSRF).
* **IDS vs. IPS:**
  * **IDS (Intrusion Detection System):** Dispositivo pasivo (conectado típicamente a un puerto SPAN/Mirror). Analiza el tráfico, detecta actividad sospechosa y genera alertas sin bloquear activamente el tráfico.
  * **IPS (Intrusion Prevention System):** Dispositivo activo colocado en línea (*in-line*). Puede bloquear de forma inmediata paquetes o conexiones anómalas en tiempo real.
  * *Detección por Firmas:* Compara el tráfico contra patrones conocidos de ataques previos. Ineficaz ante ataques de día cero (*Zero-Day*).
  * *Detección por Anomalías / Comportamiento:* Establece una línea base de tráfico habitual y genera alertas ante desviaciones estadísticas. Puede originar falsos positivos.

---

## Módulo 3: Sistemas Operativos

### 3.1 Conceptos Fundamentales
* **Sistema Operativo:** Software base que administra los recursos de hardware (CPU, memoria, almacenamiento, periféricos) y provee servicios estandarizados para la ejecución de aplicaciones.

### 3.2 Programa vs. Proceso
* **Programa:** Conjunto pasivo de instrucciones y datos almacenado como archivo ejecutable en disco (ej. `/bin/bash` o `exploit.exe`).
* **Proceso:** Instancia activa de un programa cargado en memoria RAM en ejecución. Posee su propio:
  * Espacio de direcciones de memoria virtual (Stack, Heap, Data, Text).
  * Identificador único (**PID** - *Process ID*).
  * Descriptores de archivo abiertos, variables de entorno y contexto de privilegios (UID/GID).
* **Hilos (*Threads*):** Unidades mínimas de ejecución dentro de un mismo proceso que comparten el mismo espacio de memoria, pero tienen su propia pila y registros.

### 3.3 El Kernel y sus Espacios
El Kernel es el núcleo del sistema operativo responsable de mediar entre el software y el hardware:
* **Kernel Space (Espacio de Kernel / Ring 0):** Modo con privilegios totales sobre la CPU y la memoria. Aquí residen el núcleo, los controladores de dispositivos y el planificador de procesos.
* **User Space (Espacio de Usuario / Ring 3):** Modo con privilegios restringidos donde corren las aplicaciones de usuario y servicios comunes. Para acceder a recursos de hardware, el proceso debe solicitar una llamada al sistema (**System Call** o *Syscall*), provocando una transición de contexto controlada.

---

## Módulo 4: Linux para Seguridad Ofensiva

### 4.1 Jerarquía del Filesystem (FHS)
En Linux todo se estructura a partir del directorio raíz `/`:

* `/home`: Directorios personales de los usuarios regulares (`/home/usuario`).
* `/root`: Directorio personal del superusuario `root`.
* `/bin`: Binarios esenciales de comandos ejecutables del sistema requeridos para todos los usuarios.
* `/sbin`: Binarios de administración del sistema requeridos por el administrador (`root`).
* `/usr`: Binarios, librerías y documentación secundarios de nivel de usuario (`/usr/bin`, `/usr/share`).
* `/etc`: Archivos de configuración de servicios y del sistema (objetivo frecuente de auditorías).
* `/var`: Archivos de datos variables en tiempo de ejecución: bitácoras (`/var/log`), bases de datos y correos en cola.
* `/tmp`: Archivos temporales; por estándar tiene permisos de escritura universal (`1777`), punto clave para volcar payloads.
* `/boot`: Archivos de arranque del sistema, kernel de Linux e imagen initramfs.
* `/lib` / `/lib64`: Librerías compartidas esenciales requeridas por los binarios del sistema.
* `/dev`: Nodos de dispositivos representados como archivos (`/dev/sda`, `/dev/null`, `/dev/urandom`).
* `/proc`: Sistema de archivos virtual que expone en memoria el estado del kernel y los procesos en ejecución (`/proc/<PID>`).

---

### 4.2 Variables de Entorno
Valores dinámicos disponibles para los procesos que definen el comportamiento de su entorno de trabajo:
* `$PATH`: Lista de directorios separados por dos puntos (`:`) donde la shell busca los binarios cuando se ejecuta un comando sin ruta absoluta.
  * *Riesgo en Pentesting (PATH Hijacking):* Si un binario con permisos SUID ejecuta un comando relativo (ej. `system("service status")`) y un atacante puede alterar `$PATH`, se puede anteponer un ejecutable malicioso para lograr escalada de privilegios a root.
* `$USER` / `$HOME` / `$SHELL`: Indican el usuario actual, directorio personal y la shell predeterminada.
* Comandos clave:
  * `env` o `printenv`: Muestra todas las variables de entorno activas.
  * `export VARIABLE=valor`: Define o exporta una nueva variable hacia subprocesos.

---

### 4.3 Comandos Esenciales de Linux

#### 1. Navegación e Inspección
```bash
pwd                     # Muestra la ruta de trabajo actual (Print Working Directory)
ls -la                  # Lista todos los archivos, incluidos ocultos (.), con permisos y detalles
cd /ruta/directorio     # Cambia al directorio especificado
cd ..                   # Sube un nivel en la jerarquía de directorios
cd ~                    # Lleva al directorio personal del usuario
```

#### 2. Lectura y Manipulación de Archivos
```bash
cat archivo.txt         # Concatena y muestra el contenido completo
less archivo.txt        # Paginador interactivo para leer archivos extensos
head -n 20 archivo.txt  # Muestra las primeras 20 líneas
tail -n 20 -f log.txt   # Muestra las últimas 20 líneas y sigue actualizaciones en vivo (-f)
file binario            # Determina el tipo real de archivo examinando su cabecera (magic bytes)
strings binario         # Extrae cadenas de caracteres imprimibles de archivos binarios
cp origen.txt destino   # Copia archivos
mv viejo.txt nuevo.txt  # Mueve o renombra archivos
rm -rf carpeta/         # Eliminación forzada y recursiva de directorios y archivos
```

#### 3. Búsqueda y Filtrado (Cruciales en Auditorías)
```bash
# Buscar archivos por nombre:
find / -name "*.conf" 2>/dev/null

# Buscar binarios con bit SUID activo (vital en Privilege Escalation):
find / -perm -4000 -type f -exec ls -la {} + 2>/dev/null

# Filtrar patrones de texto con grep:
grep -ri "password" /var/www/html/ 2>/dev/null

# Combinación con tuberías (pipes) para extraer datos de usuarios:
cat /etc/passwd | cut -d: -f1,7 | grep "/bin/bash"
```

---

### 4.4 Sistema de Permisos y Control de Acceso

#### Clases de Usuarios
Cada archivo y directorio en Linux asigna permisos a tres niveles:
1. **User / Owner ($u$):** El usuario propietario del archivo.
2. **Group ($g$):** El grupo de usuarios asignado al archivo.
3. **Others ($o$):** Cualquier otro usuario del sistema.

#### Permisos Básicos y Notación Octal

| Permiso | Simbólico | Valor Octal | Efecto en Archivo | Efecto en Directorio |
| :---: | :---: | :---: | :--- | :--- |
| **Lectura** | `r` | $4$ | Ver el contenido del archivo. | Listar los archivos del directorio (`ls`). |
| **Escritura** | `w` | $2$ | Modificar o truncar el archivo. | Crear, renombrar o borrar archivos en el directorio. |
| **Ejecución** | `x` | $1$ | Ejecutar el archivo como programa. | Ingresar y atravesar el directorio (`cd`). |

* La suma de los tres valores define la máscara de permisos:
  * `rwx` = $4 + 2 + 1 = 7$
  * `rw-` = $4 + 2 + 0 = 6$
  * `r-x` = $4 + 0 + 1 = 5$
  * `r--` = $4 + 0 + 0 = 4$
  * `---` = $0 + 0 + 0 = 0$

* *Ejemplo:* Permisos `-rwxr-xr--` equivalen a:
  * Propietario: `rwx` = $7$
  * Grupo: `r-x` = $5$
  * Otros: `r--` = $4$
  * Notación: `chmod 754 archivo`

#### Permisos Especiales (SUID, SGID, Sticky Bit)

```
Valor especial: [ Especial ][ Owner ][ Group ][ Others ]
                  4/2/1       7/6/5    7/6/5    7/6/5
```

1. **SUID (Set User ID - Valor octal 4000):**
   * El archivo se ejecuta con los privilegios del **propietario del archivo**, no del usuario que lo invoca.
   * Representación: `rwsr-xr-x` (si tiene permiso `x`) o `rwSr-xr-x` (si carecía de `x`).
   * Ejemplo legítimo: `/bin/passwd` (requiere escribir en `/etc/shadow` como root).
2. **SGID (Set Group ID - Valor octal 2000):**
   * En archivos: se ejecuta con los privilegios del **grupo propietario**.
   * En directorios: los archivos creados dentro heredan automáticamente el grupo del directorio padre.
   * Representación: `rwxr-sr-x`.
3. **Sticky Bit (Valor octal 1000):**
   * Aplicado a directorios compartidos (ej. `/tmp`). Permite que cualquiera cree archivos, pero **únicamente el propietario del archivo (o root) puede eliminarlo o renombrarlo**.
   * Representación: `rwxrwxrwt`.

#### Archivos Críticos de Identidad en Linux
* `/etc/passwd`: Archivo legible por todos los usuarios. Estructura:
  ```text
  usuario:x:1001:1001:Nombre:/home/usuario:/bin/bash
  ```
  *(El campo `x` indica que la contraseña cifrada reside en `/etc/shadow`)*.
* `/etc/shadow`: Archivo restringido (solo legible por `root`) que contiene los hashes de las contraseñas, sal (*salt*) y políticas de caducidad.
* `/etc/sudoers`: Define qué usuarios o grupos pueden ejecutar comandos con privilegios elevados vía `sudo` (se audita con `sudo -l`).

---

## Módulo 5: Tabla Resumen Rápida para el Examen

| Concepto | Punto de Evaluación Frecuente |
| :--- | :--- |
| **CIA vs AAA** | CIA = Confidencialidad, Integridad, Disponibilidad. AAA = Autenticación, Autorización, Auditoría (Accounting). |
| **Kill Chain vs ATT&CK** | Kill Chain es lineal/secuencial; MITRE ATT&CK es una matriz no lineal de TTPs reales. |
| **Escaneo SYN de Nmap** | No completa el 3-way handshake (envía RST tras recibir SYN-ACK); evade registros simples de aplicación. |
| **