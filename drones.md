# Dronea2: Drone Control and Cybersecurity

```
,------------------------------------------.
`-----------------------------------------. |
,-----. ,-----.  ,----. ,-.  ,-.,------.  | | ,-. ,-. ,----. ,-----.  ,----.
`----. |`----. |`----. |`--` | |`------'  `-' `-' | |'----. |`----. |'------`
,-.  | |,----' |,-.  | |,---.| |,------.  ,-. ,-. | |,----' |,----' |.-----.
| |  | || ,-. < | |  | || |\ ` || ,----'  | | | | | || ,--. || ,-. <  `---. |
| `--' || |  | || `--' || | \  || `----.  | `-' `-' || |  | || |  | |.----' |
`-----' `-'  `-' `----' `-'  | |`------'  `----^----'`-'  `-'`-'  `-' `----'
                             | `--------------------------------------------.
                              `---------------------------------------------'
```
## Descripción

Este proyecto en Python utiliza múltiples bibliotecas para llevar a cabo la interacción y control de drones. Las funciones incluyen detección de redes WiFi, detección facial, transmisión de video, y acciones más invasivas como la interrupción de señal. Adicionalmente, el proyecto cuenta con funcionalidades especializadas:

- **Interrupción de Control del Dron**: Esta función utiliza la herramienta `hping3` para inundar la red del dron, resultando en la pérdida de control por parte del piloto.
  
- **Ataque de Desautenticación WiFi**: Utiliza la herramienta `aireplay-ng` para desautenticar todos los dispositivos conectados a un punto de acceso WiFi específico.

- **Transmisión de Video del Dron Tello**: Conecta al dron Tello y muestra su transmisión de video en tiempo real utilizando `ffplay`.

Estas nuevas capacidades incrementan significativamente el alcance y la flexibilidad del proyecto, permitiendo una variedad más amplia de pruebas e investigaciones.

### Importancia de los Drones en la Guerra Mosaico y Multidominio

La guerra mosaico y multidominio es un concepto moderno de operaciones militares que involucra la coordinación de diferentes plataformas y dominios (tierra, mar, aire, ciberespacio, y espacio exterior) para lograr una ventaja táctica. Los drones han surgido como un componente crucial en este paradigma.

Primero, los drones ofrecen una flexibilidad sin precedentes. Pueden ser desplegados rápidamente para labores de reconocimiento, ataque, o incluso para crear una red de sensores que amplíe la consciencia situacional del campo de batalla. Este último aspecto es particularmente relevante en la guerra mosaico, donde pequeñas "piezas" operan en conjunto para formar un "mosaico" táctico más grande y más efectivo.

Segundo, en un ambiente multidominio, los drones pueden operar tanto en el ámbito aéreo como en el ciberespacio, efectuando tareas que van desde el mero espionaje hasta la interrupción de redes enemigas. Su capacidad para integrar múltiples funciones y sincronizarse con otros activos en distintos dominios los convierte en una herramienta de multiplicación de fuerzas.

Tercero, la autonomía de los drones permite su uso en operaciones prolongadas y en terrenos hostiles, donde el riesgo para las tropas humanas sería elevado. Esto es especialmente útil en escenarios donde la negación de acceso y el control de áreas son críticos.

Por último, los drones pueden ser más económicos y rápidos de producir que otros sistemas militares, lo que permite a las fuerzas armadas adaptarse más rápidamente a nuevas amenazas y escenarios. Estas ventajas hacen de los drones un elemento vital en la evolución hacia formas de conflicto más adaptativas y dinámicas, como la guerra mosaico y multidominio.

### Resumen de Seguridad en Drones: Caso de Estudio con DJI

Los drones han capturado la imaginación de personas de todas las edades, ofreciendo una mezcla única de tecnología y entretenimiento. Sin embargo, también presentan riesgos de ciberseguridad que a menudo son pasados por alto. 

#### Importancia de la Seguridad
Dada la creciente popularidad de drones como el Tello de DJI, asequible y fácil de usar, es imperativo entender los riesgos de ciberseguridad asociados. Estos dispositivos utilizan WiFi para la comunicación entre el dron y el piloto, lo que podría abrir la puerta a varios tipos de ataques si no se toman medidas de seguridad adecuadas.

#### Configuración y Comunicación
El dron Tello se conecta a un smartphone a través de una aplicación. Inicialmente, crea un punto de acceso sin contraseña para que el teléfono se conecte. La aplicación permite comandos de vuelo, transmisión de video y almacenamiento de fotos. 

- **Dirección IP del Dron**: 192.168.10.2
- **Dirección IP del Piloto**: 192.168.10.2
- **Puertos Utilizados**: 
    - 8889/tcp para comandos
    - 8890/tcp para datos de estado
    - 11111/udp para transmisión de video

&nbsp;   

## Vulnerabilidades Identificadas


```
|     Amenaza                             |     Riesgo    |     CVSS                                   |
|-----------------------------------------|---------------|--------------------------------------------|
|     1. Denegación de servicio           |     7.7       |     AV:L/AC:L/PR:N/UI:N/S:U/C:N/I:H/A:H    |
|     2. Suplantación de identidad ARP    |     7.1       |     AV:L/AC:L/PR:N/UI:N/S:C/C:H/I:N/A:N    |
|     3. Interceptar video                |     7.7       |     AV:L/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:H    |
|     4. Inyección de instrucciones       |     7.8       |     AV:L/AC:H/PR:N/UI:N/S:C/C:N/I:H/A:H    |

```


## Ataques Demostrados

&nbsp;   

### Objetivo 1: Denegación de Servicio
- **Metodología**: Uso de `hping3` para enviar paquetes TCP de sincronización.
- **Resultado**: Pérdida total de control sobre el dron y la transmisión de video.

&nbsp;   

### Objetivo 2: Suplantación de ARP
- **Descripción**: Identificar la posibilidad de un ataque Man-in-the-Middle (MiTM) mediante la explotación de la confianza en el protocolo ARP.
- **Herramientas**: `arpspoof`, `dsniff`
- **Metodología**:
  1. **Activar Reenvío de Paquetes**:  
     - Comando: `sysctl -w net.ipv4.ip_forward=1`
  2. **Habilitar Monitoreo de Paquetes en la Interfaz wlanX**:  
     - Comando: `arpspoof -i wlanX -t 192.168.10.2 192.168.10.1`
  3. **Configurar Reenvío de Paquetes del Dron al Controlador**:  
     - Comando: `arpspoof -i wlanX -t 192.168.10.1 192.168.10.2`
- **Resultado**: Posibilidad de interceptar o manipular el tráfico de red entre el dron y su controlador, exponiendo la falta de medidas de seguridad contra ataques de envenenamiento ARP.

&nbsp;   

### Objetivo 3: Intercepción de Video
- **Metodología**: Uso de la herramienta `FFmpeg` para capturar el tráfico de video UDP.
- **Resultado**: Éxito en la intercepción del video, aunque con alto riesgo para el atacante.

&nbsp;   

### Objetivo 4: Inyección de Comandos
- **Metodología**: Uso del SDK de Tello para enviar instrucciones maliciosas.
- **Resultado**: Pérdida total del control sobre el dron y posibilidad de realizar acciones peligrosas.

&nbsp;   

### Conclusión
Los drones, como el DJI Tello, pueden ser vulnerables a varios tipos de ataques cibernéticos que pueden tener graves consecuencias. Es crucial abordar estas vulnerabilidades para garantizar una operación segura. Además, los fabricantes deben incorporar medidas de seguridad más robustas en el diseño de estos dispositivos.


# Propuesta Framework

## Requisitos

- Python 3.x
- `colorama`
- `pyfiglet`
- `wifi`
- `subprocess`
- `netifaces`
- `nmap`
- `threading`
- `time`
- `djitellopy`
- `cv2`
- `pygame`
- `numpy`
- `concurrent.futures`
- `signal`
- `imutils`
- `torch`
- `torchvision`
- `imageio`
- `PIL`
- `pyimagesearch`

## Instalación de dependencias

```  bash
pip install colorama pyfiglet wifi netifaces nmap pygame numpy opencv-python djitellopy torch torchvision imageio Pillow pyimagesearch
```  

## Estructura del Código

### Importaciones

- Se importan todas las bibliotecas requeridas.

### Configuraciones iniciales

- Se obtienen las interfaces de red disponibles y se selecciona una.
- Se inicializa Colorama para colorear la salida de la terminal.
- Se muestra un banner de bienvenida utilizando ASCII art.

### Funciones de Deteción de Rostros

- Utiliza OpenCV para detectar rostros en un marco de video.

### Clase `FrontEnd`

- Se encarga de la interfaz gráfica utilizando Pygame.
  
### Funciones de Redes

- `buscar_redes`: Busca redes WiFi disponibles.
- `imprimir_redes`: Imprime las redes WiFi encontradas.
- `seleccionar_red`: Permite seleccionar una red WiFi.

### Manejo de Señales

- `signal_handler`: Maneja la interrupción para cerrar las ventanas de OpenCV.

## Características Principales

1. Identificación automática de interfaces de red.
2. Detección de redes Wi-Fi disponibles.
3. Control de dron Tello mediante teclado.
4. Visualización de transmisión de video del dron.
5. Detección de rostros en tiempo real.
6. **Interrupción de Control del Dron**: Utiliza `hping3` para enviar una ráfaga de paquetes, impidiendo la operación normal del dron.
7. **Ataque de Desautenticación WiFi**: Emplea `aireplay-ng` para desautenticar dispositivos en una red WiFi específica.
8. **Transmisión de Video en Tiempo Real del Dron Tello**: Utiliza `ffplay` para mostrar la transmisión de video en vivo del dron Tello.


---

## Detalle del Código

### Importación de Bibliotecas

Se importan todas las bibliotecas necesarias al inicio del script.

### Selección de Tarjeta de Red

El código escanea automáticamente todas las interfaces de red y selecciona la que comienza con "w".

```  python
interfaces = ni.interfaces()
...
print("Tarjeta de red seleccionada:", nets)
```  

### Inicialización de la Interfaz Gráfica

Se utiliza `pygame` para crear una ventana que muestra la transmisión de video del dron.

```  python
class FrontEnd(object):
    def __init__(self):
        pygame.init()
        ...
```  

### Control de Dron

Se utilizan eventos de teclado para controlar el dron.

```  python
def keydown(self, key):
    ...
def keyup(self, key):
    ...
```  

### Detección de Rostros

Se utiliza la biblioteca OpenCV para detectar rostros en la transmisión de video.

```  python
def draw_faces(frame):
    ...
```  

### Manejo de Redes Wi-Fi

El código también puede buscar redes Wi-Fi disponibles y conectarse a ellas.

```  python
def buscar_redes(nombre):
    ...
def imprimir_redes(redes):
    ...
def seleccionar_red(redes):
    ...
```  

### Menú Principal

- Provee opciones para distintas funcionalidades como detección de drones, interrupción de señal, transmisión de video, etc.

### Nuevas Funciones (Actualización)

- **Opción '1'**: Se enfoca en interrumpir el control normal del dron mediante el uso del comando `hping3` para inundar la red del dron con paquetes. Esta acción provoca que el piloto pierda el control total del dron, y no se ejecutará ningún protocolo de aterrizaje de emergencia. Debes estar conectado a la red del dron para que esta función funcione correctamente.

- **Opción '2'**: Utiliza la herramienta `aireplay-ng` para realizar un ataque de desautenticación contra un punto de acceso WiFi específico. Esto resulta en la desconexión de todos los dispositivos asociados con ese punto de acceso. Se requiere la dirección BSSID del punto de acceso que deseas atacar.

- **Opción '3'**: Se conecta al dron Tello, inicia la transmisión de video y la muestra a través de la herramienta `ffplay`. El dron Tello debe estar en un estado que permita la conexión y esta función captura el video en tiempo real del dron.

- `Opción 333`: Utiliza ImageIO para capturar el flujo de video del drone.
  
- `Opción 10`: Usa OpenCV para mostrar el flujo de video y realiza detección de rostros en tiempo real.
  
- `Opción 4`: Captura una foto del flujo de video del drone.

- `Opción 9`: Captura una foto, detecta rostros y guarda cada rostro en archivos separados.

- **Opción '5'**: Utiliza la librería pyimagesearch para aplicar el algoritmo PID (Proporcional-Integral-Derivativo) al objeto (cara) que está siendo rastreado. Muestra el error en la ubicación de la cara respecto al centro del frame, y aplica una actualización basada en el control PID para corregir el error. Se utilizan funciones para controlar el cierre seguro del programa.

- **Opción '0'**: Se enfoca en encontrar redes Wi-Fi específicas, conectar a una red seleccionada y luego usar nmap para escanear la red para encontrar otros dispositivos. Aquí, no hay código relacionado con la captura de imágenes o el rastreo de objetos.

- **Opción '44'**: Utiliza una red neuronal preentrenada (fasterrcnn_resnet50_fpn) para la detección de objetos. Se ha implementado con PyTorch y se usa para identificar objetos en cada frame capturado del feed de video. Hay un condicional para verificar si CUDA está disponible y, de ser así, la computación se realizará en la GPU.



## Requisitos

Antena wifi usb
Instalar dependencias

## Advertencia

El uso inadecuado de este software puede ser ilegal. Utiliza este código solo en redes y dispositivos para los cuales tienes permiso.

## Contribuciones

Las contribuciones son bienvenidas. Por favor, abre un issue o realiza un pull request para cualquier mejora.

## Licencia

Este proyecto está bajo la licencia MIT.
