# Laboratorio 3 — Enlaces Internacionales de Internet en Bolivia

**Estudiante:** Univ. Vila Montero Dilan 
**Fecha de consulta:** 13 de noviembre de 2026
**Modalidad:** Investigación de fuentes públicas y mediciones en Ubuntu

## 1. Infraestructura de fibra óptica

Se consultan la [UIT Broadband Maps](https://bbmaps.itu.int/bbmaps/), el [Sistema de Monitoreo de la ATT](https://smi.att.gob.bo/) y el [Submarine Cable Map](https://www.submarinecablemap.com/). Los mapas permiten identificar tramos, ubicaciones y estaciones publicadas, pero no demuestran por sí solos qué ruta física utiliza un ISP boliviano para transportar un flujo específico.

| Infraestructura identificada | Ubicación / extremos publicados | Fuente y fecha |
|---|---|---|
| Fibra terrestre 1 | **La Paz – Patacamaya**, Bolivia; operador Empresa Nacional de Telecomunicaciones S.A.; estado **operacional**; distancia indicada **90,31 km** | https://bbmaps.itu.int/bbmaps/ — captura del 12/11/2026 |
| Fibra terrestre 2 | **Arica (Chile) – Bolivia**; operador Entel; estado **operacional**; distancia indicada **197,91 km** | https://bbmaps.itu.int/bbmaps/ — captura del 12/11/2026 |
| Cable submarino 1 | **Fibra Óptica al Pacífico**; costa del Pacífico; amarre en **Ilo (Perú)** y **Lurín (Perú)**; Entel Bolivia; 1.180 km; RFS septiembre 2020 | https://www.submarinecablemap.com/ — capturas del 12/11/2026 |
| Cable submarino 2 | **South America-1 (SAm-1)**; costas del Pacífico y Atlántico; amarre documentado en **Arica (Chile)** y **Barranquilla (Colombia)**; 25.000 km; RFS marzo 2001 | https://www.submarinecablemap.com/ — capturas del 12/11/2026 |

**Contraste ATT (consulta pública 13/11/2026):** el portal oficial https://smi.att.gob.bo/ publica una sección «Mapas del sector» con la capa «Fibra óptica» y la leyenda «Red troncal de fibra óptica». El mapa se basa en datos reportados por los operadores y la propia ATT advierte que es información referencial, no certificación de disponibilidad en un punto. Esto permite contrastar la existencia de cartografía nacional de troncales con la UIT, **pero no confirmar de forma independiente los extremos y los 90,31 km del tramo La Paz–Patacamaya**. El estado «operacional» y las distancias provienen de las fichas UIT de las capturas. Fuente: https://smi.att.gob.bo/ .

**Evidencias originales revisadas:** capturas de UIT del 12/11/2026 (fichas La Paz–Patacamaya y Arica–Bolivia) y de TeleGeography del 12/11/2026 (fichas Fibra Óptica al Pacífico y SAm-1, incluidas estaciones de amarre).

## 2. ASN y proveedores

Las consultas previas identificaron los siguientes ASN. La verificación en LACNIC RDAP de **ENTEL AS6568** y **AXS AS26210** se realizó mediante capturas proporcionadas durante el trabajo. Los datos dinámicos de vecinos requieren fecha de consulta.

| Red / servicio | ASN identificado | Titular o red asociada | Upstream probable (solo ISP) | Evidencia |
|---|---|---|---|---|
| ENTEL | AS6568 | Empresa Nacional de Telecomunicaciones S.A. | AS12956 — Telefónica Global Solutions; tránsito probable (vecino observado) | https://bgp.he.net/AS6568 |
| AXS | AS26210 | AXS Bolivia S.A. | AS174 — Cogent Communications; tránsito probable (vecino observado) | https://bgp.he.net/AS26210 |
| COMTECO | AS27839 | COMTECO Ltda. | Pendiente | https://bgp.tools/as/27839 |
| COTAS | AS25620 | COTAS Ltda. | Pendiente | https://bgp.tools/as/25620 |
| TIGO Bolivia | AS27882 | TELECEL Bolivia | Pendiente | https://bgp.tools/as/27882 |
| Google / YouTube | AS15169 | Google LLC | — | https://bgp.tools/as/15169 |
| Netflix | AS2906 | Netflix Streaming Services | — | https://bgp.tools/as/2906 |
| Meta / Facebook | AS32934 | Meta Platforms | — | https://bgp.tools/as/32934 |
| TikTok / ByteDance | AS396986; AS138699 (redes relacionadas, por contrastar) | ByteDance / TikTok y redes relacionadas | — | https://bgp.tools/as/396986 ; https://bgp.tools/as/138699 |

**RDAP:** https://rdap-web.lacnic.net/ — ENTEL AS6568 y AXS AS26210, según capturas previas. Un servicio puede usar múltiples ASN o redes de terceros; especialmente TikTok no debe atribuirse a un único ASN.

## 3. Peering, tránsito y BGP

**Consulta pública complementaria:** 13/11/2026. Las tablas de HE BGP identifican vecinos observados; la clasificación comercial se indica como **probable** cuando la fuente no acredita el contrato.

| Relación entre ASN | Clasificación | Evidencia |
|---|---|---|
| ENTEL AS6568 ↔ AS12956 (Telefónica Global Solutions) | **Tránsito probable**, relación BGP observada | https://bgp.he.net/AS6568 |
| AXS AS26210 ↔ AS174 (Cogent Communications) | **Tránsito probable**, relación BGP observada | https://bgp.he.net/AS26210 |
| ENTEL AS6568 ↔ AS27839 (COMTECO) | **Vecindad BGP observada; tipo comercial incierto**. No se acredita como peering bilateral contractual | https://bgp.he.net/AS6568 |

**PeeringDB — ENTEL:** ficha de AS6568 con política general **Selective (selectiva)**; declara presencia operativa en **PIT Bolivia** (IPv4 `45.68.0.1`), **PIT Perú–Lima** y **Equinix São Paulo**. https://www.peeringdb.com/asn/6568 .

**PeeringDB — Google/YouTube:** AS15169 corresponde a Google LLC, declara política de peering **Selective (selectiva)** y servicios de contenido. https://www.peeringdb.com/asn/15169 . Que ambos publiquen políticas de peering **no demuestra que exista una sesión directa entre Google y ENTEL**.

**Interconexión AXS:** HE BGP registra a AXS AS26210 en **PIT Bolivia**, IPv4 `45.68.0.7`, además de vecinos AS12956 y AS174. https://bgp.he.net/AS26210 . La participación en el mismo IXP tampoco demuestra por sí sola una sesión entre dos miembros.

**Prefijo de ENTEL para contrastar en RIPEstat:** `166.114.16.0/20`, mostrado como prefijo de AS6568 en HE BGP: https://bgp.he.net/AS6568 . Consultar en https://stat.ripe.net/ (recurso `166.114.16.0/20`). **AS_PATH: no observable/verificado** en los resultados disponibles; no se inventa la secuencia. La existencia de un prefijo en HE BGP no equivale a una observación del AS_PATH en RIPEstat.

**Interpretación:** una vecindad BGP refleja una relación de enrutamiento observada, pero no prueba contratos comerciales ni la ruta física de los paquetes.

## 4. DNS y latencia

**Entorno:** Ubuntu en VirtualBox. **Fecha de medición:** 13 de noviembre de 2026, 09:31 (-04). **Evidencia:** archivo `resultados_lab3.txt` enviado por el estudiante y captura de consulta adicional del Banco Unión. Se usó `dig +noall +answer`; los TTL son los observados en el momento, no valores permanentes.

| Dominio | A (IPv4) | NS (uno) | MX (uno) | TTL de A (s) |
|---|---|---|---|---:|
| impuestos.bo | Sin respuesta A en las consultas realizadas | Sin respuesta observada | Sin respuesta observada | — |
| umsa.bo | 200.7.161.56 | marte.umsa.bo | 1 aspmx.l.google.com | 2514 |
| entel.bo (ISP) | Sin respuesta A observada | ns2.entelnet.bo | 0 entel-bo.mail.protection.outlook.com | — |
| bancounion.com.bo (empresa privada) | 192.124.249.120 | ns2.bancounion.com.bo | 0 bancounion-com-bo.mail.protection.outlook.com | 17061 |

**TTL adicionales observados:** `umsa.bo` NS 3600 s, MX 2331 s; `entel.bo` NS 11 s, MX 28 s; `bancounion.com.bo` NS 1303 s, MX 600 s. El dominio `impuestos.bo` tampoco devolvió respuesta A al consultar el resolutor `8.8.8.8`, según la prueba reportada; esto **no demuestra que el sitio esté caído**.

**Registros adicionales (UMSA):** AAAA `2801:104:dc:a161::a56` (TTL 60 s); SOA `marte.umsa.bo. admdns.umsa.bo. 2022032362 3600 1800 604800 86400` (TTL 3600 s); TXT SPF `v=spf1 a mx ip4:200.7.160.201 include:spf.mandrillapp.com include:_spf.google.com -all` (TTL 60 s). También aparecieron registros TXT de verificación de dominio.

**Registros adicionales (ENTEL):** SOA `ns.entelnet.bo. root.entel.bo. 2016101657 3600 600 604800 10800` (TTL 60 s); TXT SPF `v=spf1 include:spf.protection.outlook.com -all` (TTL 11 s). Por tanto, **UMSA y ENTEL** cuentan con observaciones SOA y TXT, pero no se obtuvo respuesta AAAA para ENTEL: la consulta fue realizada, sin respuesta registrada. La guía pide consultar los tres tipos para dos dominios, no que necesariamente existan registros.

### Pruebas de latencia

| Dominio | IP resuelta | RTT promedio (ms) | Pérdida (%) | Observación |
|---|---|---:|---:|---|
| impuestos.bo | No resuelta en la consulta | No medible | No medible | No se dispone de resumen de ping; no atribuir caída del servicio |
| umsa.bo | 200.7.161.56 | **22.329** | **0 %** | 5 enviados, 5 recibidos; mínimo 13.725 ms, máximo 42.955 ms |
| entel.bo | No resuelta para ping | No medible | No medible | `ping: entel.bo: No existe ninguna dirección asociada al nombre` |

**Interpretación:** DNS no determina por sí solo en qué país se aloja físicamente un servicio. El RTT mide ida y vuelta; un nombre sin A o un ping sin respuesta no demuestra indisponibilidad del sitio web.

**Fuentes de metodología:** https://dns.google/ ; https://toolbox.googleapps.com/apps/dig/ . **Fuente primaria de resultados:** mediciones propias guardadas en `resultados_lab3.txt` y captura del 13/11/2026.

## 5. CDN y plataformas

La matriz siguiente no afirma instalaciones de caché dentro de operadores bolivianos sin una fuente específica. «No verificable» significa que aún no contamos con evidencia pública suficiente para esa afirmación.

| Servicio | ENTEL: evidencia | AXS: evidencia | ¿Caché dentro del ISP demostrada? |
|---|---|---|---|
| Netflix | No verificable | No verificable | No demostrada |
| Google / YouTube | No verificable | No verificable | No demostrada |
| TikTok | No verificable | No verificable | No demostrada |
| Facebook / Meta | No verificable | No verificable | No demostrada |

**Fuentes consultadas:** https://bgp.tools/ ; https://www.peeringdb.com/asn/6568 ; https://www.peeringdb.com/asn/15169 ; https://www.cdnplanet.com/tools/cdnfinder/ ; https://radar.cloudflare.com/scan . **Resultado:** se verificaron políticas y presencia pública de redes, pero no una relación específica de entrega o caché de las cuatro plataformas dentro de ENTEL y AXS. Se conserva «no verificable» sin confundirlo con ausencia de conexión. Google advierte en PeeringDB que no todo su contenido está disponible en cada punto de presencia o intercambio.

| CDN comercial | ASN / red | Característica pública |
|---|---|---|
| Cloudflare | AS13335 | Red Anycast para entrega y protección de tráfico |
| Fastly | AS54113 | CDN de distribución de contenido y caché en el borde |
| Akamai | AS20940 | Distribución de contenido mediante red de servidores distribuidos |

**Fuentes:** https://bgp.tools/as/13335 ; https://bgp.tools/as/54113 ; https://bgp.tools/as/20940 .

Para demostrar una caché Netflix o Google dentro de un ISP boliviano se necesitarían datos específicos de instalación o confirmación del operador/plataforma, además de evidencias de tráfico y topología. Una IP de CDN no demuestra ubicación física porque la entrega puede depender de Anycast, balanceo, geolocalización inexacta o infraestructura de terceros.

## 6. Observaciones y conclusiones preliminares

1. Los mapas de fibra y cables submarinos describen infraestructura publicada, pero no identifican por sí solos la ruta utilizada por una conexión concreta.
2. Los ASN permiten relacionar operadores y plataformas con redes anunciadas; no representan necesariamente todos los caminos de entrega de sus servicios.
3. Una relación BGP visible no equivale a prueba de contrato comercial ni de presencia física compartida.
4. En las mediciones locales, `umsa.bo` respondió 5 de 5 pings con RTT promedio de 22.329 ms; `entel.bo` no resolvió una dirección para ping. Esto no permite inferir ubicación física ni disponibilidad web.
5. Una relación entre un ISP y una plataforma no confirma que haya una caché CDN dentro de la red del ISP.

## Preparación para la defensa individual (máximo 10 minutos)

- Fibra: mostrar dos tramos terrestres y dos cables submarinos, indicando límites de los mapas.
- ASN/BGP: explicar ENTEL, AXS, RDAP, un upstream probable y un peer.
- DNS/ping: comparar registros y RTT medidos, explicar TTL y pérdida ICMP.
- CDN: presentar matriz de Netflix, Google, TikTok y Meta sin confundir peering con caché local.

## Registro de evidencias y limitaciones

- **Fuentes primarias aportadas:** capturas del 12/11/2026 de mapas UIT, TeleGeography, consultas de redes y registros RDAP; medición local `resultados_lab3.txt` del 13/11/2026; captura adicional de Banco Unión.
- **Limitaciones tras la verificación:** el portal ATT permite un contraste cartográfico general, no una certificación del tramo específico; se identificó una vecindad BGP ENTEL–COMTECO, pero no su tipo comercial; se identificó el prefijo 166.114.16.0/20 de ENTEL, pero no un AS_PATH verificable en RIPEstat; las cachés locales por ISP y plataforma no están demostradas.
- **Sin respuesta DNS:** `impuestos.bo` no devolvió registros en las consultas observadas; el resultado no prueba que el dominio no exista o que su servicio esté caído.
- **Limitación de ping:** el archivo de medición no contiene un resumen ICMP de `impuestos.bo`; se informa como no medible, sin inventar paquetes perdidos.

**Verificación adicional de fuentes (13/11/2026):** https://smi.att.gob.bo/ ; https://bgp.he.net/AS6568 ; https://bgp.he.net/AS26210 ; https://www.peeringdb.com/asn/6568 ; https://www.peeringdb.com/asn/15169 .

**Antes de entregar:** reemplazar el nombre del estudiante en la cabecera y renombrar este archivo con el formato `Laboratorio_Enlaces_Internacionales_Apellido_Nombre.md`. El contenido distingue datos observados, inferencias y límites de la evidencia conforme a la guía.
