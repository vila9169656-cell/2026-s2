# INFORME DE LABORATORIO 1

## 1. Datos generales

**Estudiante:** Dilan  
**Fecha:** 02/10/2026  
**Distribución utilizada:** Ubuntu 24.04.4 LTS  
**Entorno:** Máquina virtual en VirtualBox  
**Kernel:** 7.0.0-34-generic  
**Arquitectura:** x86_64  

## 2. Objetivo

Construir y analizar una topología de red virtual en Linux utilizando network namespaces y un par de interfaces veth, verificando la conectividad entre dos hosts virtuales, el funcionamiento de ARP e ICMP y el comportamiento de la red ante una falla controlada.

## 3. Preparación del entorno

Se trabajó en Ubuntu utilizando las herramientas de red disponibles en Linux. Se crearon dos espacios de nombres de red denominados `hostA` y `hostB`, permitiendo simular dos hosts independientes dentro de una misma máquina.

## 4. Creación de la topología

Se crearon los namespaces:

- `hostA`
- `hostB`

Posteriormente se creó un par de interfaces virtuales veth:

- `vethA`
- `vethB`

La interfaz `vethA` fue asignada a `hostA` y `vethB` a `hostB`.

La configuración utilizada fue:

- hostA - vethA: `10.10.1.1/30`
- hostB - vethB: `10.10.1.2/30`

Después se habilitaron las interfaces y las interfaces de loopback correspondientes.

## 5. Verificación de conectividad

Se realizaron pruebas de conectividad mediante `ping` en ambos sentidos.

Desde `hostA` hacia `hostB` se obtuvo respuesta satisfactoria, con 4 paquetes transmitidos, 4 recibidos y 0 % de pérdida.

También se verificó la comunicación desde `hostB` hacia `hostA`, obteniendo nuevamente conectividad correcta.

Estos resultados demostraron que el enlace virtual entre ambos namespaces se encontraba correctamente configurado.

## 6. Análisis de ARP e ICMP

Se utilizó `tcpdump` para observar el tráfico generado entre los hosts virtuales.

Durante la captura se observaron mensajes ICMP Echo Request y Echo Reply correspondientes a las pruebas de ping. También se observó tráfico ARP utilizado para resolver las direcciones IP a direcciones MAC.

La captura finalizó con:

- 12 paquetes capturados.
- 12 paquetes recibidos por el filtro.
- 0 paquetes descartados por el kernel.

Esto permitió comprobar de manera práctica el funcionamiento de ARP e ICMP dentro de la topología.

## 7. Falla controlada y diagnóstico

Para analizar el comportamiento de la red ante una falla, se deshabilitó intencionalmente la interfaz `vethB` de `hostB`.

### Predicción

Al deshabilitar `vethB`, se esperaba perder la comunicación entre `hostA` y `hostB`.

### Síntoma observado

Al realizar nuevamente el ping desde `hostA`, apareció el mensaje `Destination Host Unreachable`.

El resultado fue:

- 4 paquetes transmitidos.
- 0 paquetes recibidos.
- 100 % de pérdida.

### Diagnóstico y causa

Se verificó el estado de las interfaces y se comprobó que `vethB` se encontraba en estado DOWN. La pérdida de conectividad fue causada por la desactivación de esta interfaz.

### Corrección

Se volvió a habilitar `vethB`.

### Prueba de recuperación

Después de habilitar nuevamente la interfaz se repitió el ping. Se obtuvieron 4 paquetes transmitidos, 4 recibidos y 0 % de pérdida, confirmando la recuperación de la comunicación.

## 8. Uso de OpenCode

Como herramienta de apoyo se instaló y utilizó OpenCode versión `1.18.34`.

Se realizó una interacción relacionada con el diagnóstico y recuperación de la conectividad cuando la interfaz `vethB` se encontraba en estado DOWN.

La propuesta obtenida fue contrastada con comandos reales de Linux y posteriormente se verificó el funcionamiento mediante una nueva prueba de conectividad.

La herramienta fue utilizada únicamente como apoyo para el análisis, verificando los resultados directamente en el entorno de laboratorio.

## 9. Conclusiones

La práctica permitió comprender el funcionamiento de los network namespaces y las interfaces veth para construir una red virtual dentro de Linux. Se logró establecer correctamente la comunicación entre dos hosts virtuales y observar mediante tcpdump el intercambio de paquetes ARP e ICMP.

La falla controlada permitió comprobar la importancia del estado de las interfaces durante el diagnóstico de problemas de conectividad. Finalmente, al habilitar nuevamente la interfaz afectada, la comunicación fue recuperada correctamente.

El laboratorio permitió relacionar los conceptos teóricos de direccionamiento IP, ARP, ICMP y diagnóstico de redes con su funcionamiento práctico en Linux.

