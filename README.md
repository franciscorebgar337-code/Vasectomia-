# Vasectomia-
Jornada de vasectomías.

## Contenido

- `vasectomia_registro-1.html` — formulario de registro de jornada de vasectomía sin bisturí (IMSS-BIENESTAR). Captura datos del paciente y envía la información por WhatsApp.
- `inventario_medicamentos.html` — sistema de **entradas y salidas de medicamentos** con catálogo base del Cuadro Básico IMSS-BIENESTAR / Secretaría de Salud.

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
