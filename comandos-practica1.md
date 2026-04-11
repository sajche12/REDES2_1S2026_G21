# Comandos de Verificación

Este documento contiene los comandos clave utilizados para validar la conectividad, 
segmentación y seguridad de la infraestructura de red interconectada.

## 1. Verificación de Capa 2 (Conmutación y VLANs)

### Estado de las VLANs
Permite verificar que las VLANs (13, 23, 33, 63, 73, 83) estén creadas y activas.

```bash
# Ejecutar en cualquier Switch
show vlan brief

### Estado de los Enlaces Troncales

Crucial para asegurar que las etiquetas de las VLANs viajen a través de la "telaraña" de switches.

```bash
# Ejecutar en Switches de distribución (SW0, SW3, SW11, etc.)
show interfaces trunk

```

* **Qué verificar:** Que los puertos aparezcan en modo `on`, estado `trunking` y que las VLANs permitidas incluyan las del edificio correspondiente.

### Spanning Tree Protocol (STP)

Valida que no existan bucles y que la convergencia sea correcta.

```bash
show spanning-tree
# Verificación específica por segmento
show spanning-tree vlan 13

```

---

## 2. Verificación de Capa 3 (Enrutamiento y Gateway)

### Interfaces y Subinterfaces (Router on a Stick)

Valida que las puertas de enlace de cada edificio estén activas.

```bash
# Ejecutar en Router0 y Router3
show ip interface brief

```

* **Qué verificar:** Que las subinterfaces (ej. `Gig0/0.13`) estén `up/up`.

### Tabla de Enrutamiento y Redistribución

Muestra el "mapa mental" de los routers y la unión de protocolos (EIGRP, RIP, OSPF).

```bash
# Ejecutar en Router1 y Router4 (Routers Traductores)
show ip route

```

* **Clave de éxito:** Buscar rutas con prefijos `D` (EIGRP), `R` (RIP) y `O` (OSPF). Las rutas aprendidas por redistribución aparecerán como `D EX` o `R`.

---

## 3. Verificación de Seguridad y Conectividad

### Port-Security (VLAN Básicos)

Verifica que solo la MAC autorizada tenga acceso al puerto.

```bash
# Ejecutar en switches de acceso (donde esté conectada la PC de Básicos)
show port-security interface fa0/2

```

### Pruebas de Conectividad (Ping)

Validación de extremo a extremo entre edificios.

```bash
# Desde PC de Primaria Izquierda (VLAN 13)
ping 192.178.13.1     # Hacia el Gateway
ping 192.178.63.10    # Hacia Primaria Derecha (Capa 3 completa)

```

---

## 4. Comandos de Administración

Para asegurar que los cambios persistan tras un reinicio.

```bash
# Ejecutar en TODOS los equipos
copy running-config startup-config
# O su versión abreviada
wr
