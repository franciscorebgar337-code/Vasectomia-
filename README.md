# Vasectomia-
Jornada de vasectomías.

## Contenido

- `vasectomia_registro-1.html` — formulario de registro de jornada de vasectomía sin bisturí (IMSS-BIENESTAR). Captura datos del paciente y envía la información por WhatsApp.
- `inventario_medicamentos.html` — sistema de **entradas y salidas de medicamentos** con catálogo base del Cuadro Básico IMSS-BIENESTAR / Secretaría de Salud.
- `mensajeria_mesh.html` — **Malla**: mensajería cifrada de extremo a extremo que viaja **sin Internet ni red celular** (mesh híbrida). Útil para brigadas, contingencias y zonas sin cobertura.

## Malla — mensajería mesh sin Internet (`mensajeria_mesh.html`)

App de un solo archivo, 100% en el navegador, **sin servidores, sin cuentas y sin dependencias externas** (todo el núcleo es offline). Los datos se guardan solo en el dispositivo (`localStorage`).

### Qué funciona de verdad hoy en un navegador (gratis, sin operador)

- **Identidad criptográfica** (no un número de teléfono): par de llaves ECDH + ECDSA P-256 generadas con WebCrypto. Tu "contacto" es un código compartible (texto, archivo, QR).
- **Cifrado de extremo a extremo**: AES-256-GCM con clave derivada por ECDH + HKDF; cada mensaje va **firmado** (ECDSA) y se verifica el remitente.
- **Malla con saltos**: cada mensaje lleva TTL, se **deduplica** y se reenvía por todos los enlaces (flooding controlado).
- **Almacenar y reenviar** (*store-and-forward*): si no hay ruta, el mensaje espera en cola y se entrega cuando aparece un enlace.
- **P2P por Wi-Fi/hotspot local** con WebRTC **sin servidor de señalización** (se intercambia un código una sola vez). Funciona en la misma red local sin Internet.
- **Enlace entre pestañas/ventanas** del mismo navegador (`BroadcastChannel`) para probar el ruteo.
- **Relevo físico (*sneakernet*)**: exporta la cola ya cifrada a archivo/QR, alguien la transporta y la importa donde sí hay alcance. Quien la lleva **no puede leerla**.
- **Acuses de entrega** (✓✓) que regresan por la malla.
- **QR sin librerías**: generador de QR propio (modo byte, ECC L, v1–10) verificado decodificando con jsQR; escáner con `BarcodeDetector` nativo.

### Qué requiere app nativa o hardware (documentado dentro de la app)

- **Bluetooth Mesh** (estilo Bridgefy/Briar) y **Wi-Fi Direct/Aware**: el navegador no puede anunciar/escanear BLE en segundo plano de forma fiable → requiere app nativa Android/iOS.
- **LoRa (SX1262/1276) / Meshtastic** y **APRS/DMR/Packet**: necesitan radio física (y, en radioafición, licencia).

El protocolo de paquetes está diseñado con **transportes intercambiables**, de modo que una versión nativa pueda enchufar BLE mesh o un puente a Meshtastic/LoRa reutilizando la misma capa de identidad + cifrado + ruteo.

### Cómo probarlo en 1 minuto

1. Abre `mensajeria_mesh.html` en **dos pestañas** (o dos teléfonos en el mismo Wi-Fi/hotspot).
2. En **Contactos** pon tu nombre y copia tu código; agréguense mutuamente.
3. Entre pestañas ya están enlazadas; entre teléfonos crea/acepta una invitación en **Enlaces**.
4. Escríbanse desde **Chats**. El contador "mensajes ruteados" muestra los saltos.

> Demostración educativa de mensajería descentralizada. Para uso crítico, audita la criptografía antes de depender de ella.

## Módulo de inventario de medicamentos

Archivo independiente (`inventario_medicamentos.html`) que funciona 100% en el navegador (sin servidor). Guarda los datos en `localStorage` del equipo y permite transferirlos a otros equipos vía Excel o JSON.

### Funciones

- **Catálogo base** precargado con ~100 claves del Cuadro Básico (analgésicos, AINE, antibióticos, anestésicos locales, corticoides, cardiovasculares, antidiabéticos, SNC, soluciones parenterales, antisépticos, vitaminas, anticonceptivos). Editable y ampliable.
- **Registro de entradas y salidas**, de forma **manual** o con **código de barras** (cámara del dispositivo vía `html5-qrcode`, o lector físico USB/Bluetooth a través del input dedicado).
- **Stock por lote y caducidad** con **semáforo**:
  - Verde: caduca en ≥12 meses.
  - Amarillo: 6–11 meses.
  - Rojo: 0–5 meses; aparece un aviso "≤3 m" cuando falta 3 meses o menos.
  - Negro: caducado.
  - Umbrales configurables desde la pestaña **Ajustes**.
- **Peso por unidad** (tableta, cápsula, ml, etc.) más peso del empaque y **peso total por lote** (gramos y kilogramos). Totales generales en la vista de Stock.
- **Historial** cronológico de todos los movimientos con filtros y exportación a Excel.
- **Import/Export** en Excel (`.xlsx`, multi-hoja: `Catalogo`, `Lotes`, `Movimientos`, `Config`) y JSON (paquete completo con versión y timestamp). Soporta **Reemplazar** o **Mezclar** al importar (upsert por clave/id).

### Uso

1. Abrir `inventario_medicamentos.html` en un navegador moderno (Chrome, Firefox, Edge; móvil o PC).
2. En **Ajustes**, escribir el nombre del equipo/unidad y el usuario por defecto.
3. Registrar entradas desde **Entrada** (clave autocompletada desde el catálogo, lote, caducidad y cantidad).
4. Registrar salidas desde **Salida** (sugiere FEFO; bloquea stock negativo).
5. Consultar **Stock** y **Historial**.
6. Exportar a otros equipos desde **Respaldo**.

### Dependencias (CDN)

- [SheetJS (xlsx)](https://cdn.jsdelivr.net/npm/xlsx@0.18.5/dist/xlsx.full.min.js) para Excel.
- [html5-qrcode](https://unpkg.com/html5-qrcode@2.3.8/html5-qrcode.min.js) para escáner por cámara.

### Fuentes del catálogo

- IMSS — Cuadros Básicos: <https://www.imss.gob.mx/profesionales-salud/cuadros-basicos/medicamentos>
- Compendio Nacional de Insumos para la Salud 2025: <https://www.gob.mx/csg/articulos/medicamentos-compendio-nacional-de-insumos-para-la-salud-2025>
