

---
layout: page
title: Drones
permalink: /drones/
---


# Dronea2: Control de Drones y Seguridad Cibernética

## Índice
- [Introducción](#introducción)
- [Tecnologías Utilizadas](#tecnologías-utilizadas)
- [Ataques y Contramedidas](#ataques-y-contramedidas)
- [Cómo Empezar](#cómo-empezar)
- [Funcionamiento](#funcionamiento)
- [Casos de Uso](#casos-de-uso)
- [Conclusión](#conclusión)
- [Licencia](#licencia)

## Introducción
`Dronea2` es un proyecto diseñado para ofrecer un control más seguro y versátil de drones, específicamente del DJI Tello. El proyecto integra funcionalidades para el control de drones, detección facial mediante OpenCV, y herramientas de seguridad cibernética para detectar y mitigar ataques específicos contra drones.

## Tecnologías Utilizadas

### OpenCV
Se utiliza para la detección facial y seguimiento en tiempo real.

### Pygame
Brinda una interfaz gráfica intuitiva para el control del dron.

### Redes y Seguridad
Para la administración de la red se utilizan las bibliotecas `Netifaces` y `Nmap`.

## Ataques y Contramedidas

### Ataques Conocidos Contra Drones DJI

- **Desautenticación**: Desconectar el dron de la red.
- **Jamming**: Interferencia en la comunicación entre el dron y el controlador.
- **Sniffing de Video**: Captura no autorizada del video transmitido.
- **MITM (Man-in-the-Middle)**: Interceptación y manipulación de la comunicación.

### Estrategias de Mitigación

- **Detección de Desautenticación**: Alarma si se detecta un intento de desconexión.
- **Anti-Jamming**: Cambio de canal para evitar interferencias.
- **Cifrado de Video**: Protege contra la captura no autorizada del video.
- **Certificados de Seguridad**: Previene ataques MITM mediante la autenticación mutua.

## Cómo Empezar

```bash
# Clonar el repositorio
git clone https://github.com/tu_usuario/Dronea2.git

# Instalar dependencias
pip install -r requirements.txt

# Ejecutar el programa
python main.py
