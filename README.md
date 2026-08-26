# Fiado — Cuaderno digital del tendero

PWA offline para registrar fiados, abonos y cobrar por WhatsApp.
Los datos se guardan **solo en el dispositivo** (IndexedDB `tendero_fiado`, v1).

---

## Novedad de esta versión: Copia de seguridad completa

Ruta: **⚙︎ Ajustes → 📤 Copia de seguridad (Excel / WhatsApp)**

### 1 · Excel
- Archivo `Fiado-<Tienda>-AAAA-MM-DD.xls` con **3 hojas**:
  - **Resumen** — tienda, tendero, fecha del respaldo, total por cobrar, fiado histórico, abonado histórico, conteos.
  - **Clientes** — nombre, teléfono, saldo actual, total fiado, total abonado, días de la deuda más vieja, estado, n° de movimientos, cliente desde, autorización de datos. Fila de TOTAL al final.
  - **Movimientos** — fecha, hora, cliente, teléfono, tipo, descripción, columna *Fiado (+)* y columna *Abono (−)* (para sumar directo en Excel) e ID. Fila de TOTALES al final.
- Formato SpreadsheetML 2003. Si Excel de escritorio pregunta por el formato, responder **Sí**.
- **CSV** de respaldo (`;` como separador + BOM UTF-8) por si el celular no abre el `.xls`. Incluye los dos bloques (Clientes y Movimientos) en un solo archivo.

### 2 · WhatsApp
- **Mandar el Excel por WhatsApp** — usa `navigator.share({files})`; en Android/iOS abre la hoja de compartir con WhatsApp, correo, Drive, etc. En escritorio cae a descarga normal.
- **Mandar el reporte escrito** — texto formateado con negrillas de WhatsApp: resumen, quiénes deben (ordenados de mayor a menor con días de mora), quiénes están al día y el detalle movimiento por movimiento.
  - Si el reporte supera ~1600 caracteres, se envía el resumen por `wa.me` y el detalle completo queda **copiado al portapapeles** para pegarlo en un segundo mensaje (los enlaces `wa.me` muy largos se cortan).
- **Copiar el reporte completo** — al portapapeles, para pegar donde sea.

### 3 · Respaldo restaurable
- **Bajar respaldo (.json)** — el único formato que devuelve los datos a la app.
- **♻️ Restaurar desde un respaldo** — lee un `.json` de esta misma app y hace `put()` por `id`:
  - **No borra nada.** Los registros existentes se conservan; los del archivo se agregan y los que tengan el mismo `id` se actualizan.
  - Pide confirmación mostrando cuántos clientes y movimientos entrarían.
  - La configuración (tienda/tendero) solo se restaura si la app todavía está en valores por defecto.

### Habeas Data (Ley 1581 de 2012)
- Casilla **"Incluir los teléfonos completos"**, apagada por defecto en toda la copia.
- Apagada, los teléfonos salen enmascarados (`••• ••• 67`) en Excel, CSV y WhatsApp.
- El `.json` siempre lleva los datos completos porque su función es restaurar; es un archivo local del tendero.

---

## Lo que NO se tocó

- Esquema de IndexedDB: `DB_NAME='tendero_fiado'`, `DB_VER=1`, stores `clientes`, `movimientos`, `config`. **Sin migraciones**, así que los datos ya guardados siguen intactos al reemplazar el archivo.
- Funciones de registro de fiados, abonos, clientes, recordatorio de WhatsApp, política de datos y borrado.
- `exportar()` se conserva con el mismo nombre; solo ahora usa la hoja de compartir en móvil antes de caer a descarga.

**Importante:** el respaldo depende del origen (dominio) desde donde se abra la app. Si se reemplaza `index.html` en la misma URL, los datos siguen ahí. Si se cambia de dominio, hay que exportar el `.json` en el dominio viejo y restaurarlo en el nuevo.

---

## Despliegue

Archivo único, sin dependencias ni build. Subir `index.html` a Netlify, GitHub Pages o servirlo con cualquier estático.
Bloque Open Graph / Twitter Card completo (9 etiquetas, `og:image` absoluta 1200×630).

---

Desarrollada por **Vibras Positivas HM** — Derechos de Autor Reservados
