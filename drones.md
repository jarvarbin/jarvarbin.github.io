# Dronea2
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
