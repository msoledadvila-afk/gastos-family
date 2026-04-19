# 💜 Gastos con Amor

Sistema de gestión de finanzas familiares compartidas. Diseñado para hacer seguimiento de cuotas de tarjeta, préstamos, gastos fijos y variables entre dos personas — con lógica real de cierres de tarjeta.

**App desplegada:** [msoledadvila-afk.github.io/Gastos-familiares](https://msoledadvila-afk.github.io/Gastos-familiares/)

---

## Descripción general

La app conecta un frontend web (GitHub Pages) con un backend en Google Apps Script que escribe y lee un Google Sheet. No requiere servidor ni base de datos: todo vive en el Sheet.

Está pensada para un hogar con dos personas (Soledad y Gabriel) pero es fácilmente escalable a más. Cada registro tiene un campo `titular` que permite filtrar y separar los gastos de cada uno.

---

## Archivos del proyecto

### `index.html`
Frontend completo en React (compilado en el browser con Babel). Una sola página con navegación por solapas en la parte inferior.

### `Code.gs`
Backend en Google Apps Script. Expone una API REST simple via `doGet` y `doPost`. Se despliega como Web App desde el editor de Apps Script.

---

## Estructura del Google Sheet

| Hoja | Descripción |
|------|-------------|
| `CUOTAS_ACTIVAS` | Una fila por compra en cuotas. Guarda monto por cuota, cuotas totales y pagadas, tarjeta y titular. |
| `PRESTAMOS` | Préstamos personales y adelantos de sueldo con seguimiento de cuotas. |
| `GASTOS_FIJOS` | Servicios, expensas, colegio y otros gastos recurrentes mensuales. Incluye campo `pagado` (SI/NO) que se resetea cada mes. |
| `GASTOS_MES` | Registro libre de gastos variables del mes (supermercado, salidas, etc.). |
| `CONFIG` | Clave-valor para persistir configuración entre sesiones (ingresos de Sole y Gabi). |

---

## Configuración desde cero

### 1. Google Apps Script

1. Abrí [script.google.com](https://script.google.com) y creá un proyecto nuevo
2. Pegá el contenido de `Code.gs`
3. **Implementar → Nueva implementación → Aplicación web**
   - Ejecutar como: *Yo*
   - Quién tiene acceso: *Cualquier usuario*
4. Copiá la URL generada
5. Para actualizaciones futuras: **Implementar → Administrar implementaciones → ✏️ editar → Nueva versión** (la URL no cambia)

### 2. Inicializar las hojas

Abrí en el navegador:
```
TU_URL_APPS_SCRIPT?action=init
```
Debe responder `{"status":"ok","message":"Hojas creadas/verificadas"}`. Esto crea las 5 hojas automáticamente.

### 3. GitHub Pages

1. Subí `index.html` al repositorio
2. En Settings → Pages, activá GitHub Pages desde la rama `main`
3. En el `index.html`, reemplazá la constante `EP` con tu URL de Apps Script:
```javascript
const EP = "https://script.google.com/macros/s/TU_CODIGO/exec";
```

---

## Cómo usar la app

### Inicio (Dashboard)
- Ingresá los sueldos de Sole y Gabi — se guardan automáticamente en el Sheet y se restauran al reabrir la app
- Muestra el cierre de tarjeta actual con el total separado por titular
- Lista los gastos fijos pendientes de pago como recordatorio
- Muestra los gastos variables del mes separados por persona
- Calcula el balance: Ingresos − Egresos = resultado en verde o rojo

### Cuotas
- Una tarjeta por compra: se ingresan el monto por cuota, cuotas totales y cuántas ya se pagaron
- Cada cuota muestra en qué cierre de tarjeta cae la próxima acreditación
- Badge amarillo `↻ Abr 2026` = entra en el cierre actual
- Badge violeta `→ May 2026` = entra en el siguiente
- Botón "Pagar cuota X" incrementa el contador y actualiza el Sheet
- Filtro por titular (Sole / Gabriel)

### Préstamos
- Seguimiento de préstamos personales y adelantos de sueldo
- Mismo sistema de progreso que cuotas

### Fijos
- Lista de gastos recurrentes mensuales con checkboxes
- Al hacer click en el check, se guarda el estado en el Sheet inmediatamente
- Los pendientes aparecen como recordatorio en el Dashboard

### Variables
- Registro de gastos del mes con fecha, descripción, categoría, titular y medio de pago
- Si se paga con tarjeta de crédito y en cuotas, el gasto se convierte automáticamente en una cuota activa distribuida por período de cierre
- Incluye "VISA PATAGONIA (Gabi)" como medio de pago (asigna automáticamente titular = Gabriel)
- Al seleccionar una fecha muestra en qué resumen de tarjeta va a caer ese gasto

---

## Lógica de cierres de tarjeta

Las tres tarjetas (VISA BNA, VISA PATAGONIA, MC PATAGONIA) comparten el mismo calendario de cierre: **cuarto jueves de cada mes**.

Un gasto o cuota que se acredita **antes o el día del cierre** entra en ese resumen. Lo que se acredita **después del cierre** entra en el siguiente.

Ejemplo: si el cierre de abril es el 23/Apr:
- Compra del 22/Abr → entra en el resumen de Abr, vence en Mayo
- Compra del 24/Abr → entra en el resumen de May, vence en Junio

Los cierres están hardcodeados en `CIERRES` al principio del `index.html` y deben actualizarse anualmente.

**Cierres 2026:**

| Mes | Fecha |
|-----|-------|
| Abril | 23/04 |
| Mayo | 28/05 |
| Junio | 25/06 |
| Julio | 23/07 |
| Agosto | 27/08 |
| Septiembre | 24/09 |
| Octubre | 22/10 |
| Noviembre | 26/11 |
| Diciembre | 24/12 |

---

## Escalar a más personas

Cada registro tiene un campo `titular`. Para agregar una persona:

1. En `DATOS_REALES` del `index.html`, agregar sus cuotas con `titular: "NombrePersona"`
2. En los formularios, agregar la opción en los `<select>` de titular
3. En los medios de pago, agregar su tarjeta si corresponde
4. El Dashboard ya separa automáticamente por titular en los bloques de cierre y variables

---

## API — Endpoints disponibles

### GET
| Acción | Descripción |
|--------|-------------|
| `?action=ping` | Verifica que el script está activo |
| `?action=init` | Crea/verifica las hojas del Sheet |
| `?action=getCuotas` | Lista todas las cuotas |
| `?action=getPrestamos` | Lista todos los préstamos |
| `?action=getGastosFijos` | Lista los gastos fijos |
| `?action=getGastosMes` | Lista los gastos variables |
| `?action=getConfig` | Devuelve la configuración guardada |
| `?action=pagarCuota&id=X` | Incrementa cuotas_pagadas en 1 |
| `?action=pagarPrestamo&id=X` | Incrementa cuotas_pagadas en 1 |
| `?action=togglePagoFijo&id=X` | Alterna pagado SI/NO en un gasto fijo |
| `?action=deleteItem&id=X&sheet=HOJA` | Elimina una fila por id |

### POST (body: `data=JSON`)
| Acción | Descripción |
|--------|-------------|
| `addCuota` | Agrega una cuota activa |
| `addPrestamo` | Agrega un préstamo |
| `addGastoFijo` | Agrega un gasto fijo |
| `addGastoMes` | Agrega un gasto variable |
| `setConfig` | Guarda una clave en CONFIG (`{clave, valor}`) |
| `resetPagosFijos` | Resetea todos los fijos a pagado=NO (inicio de mes) |

---

## Tareas pendientes

- [ ] Trigger automático el 1° de cada mes para resetear `pagado=NO` en todos los gastos fijos (hoy se puede hacer manualmente con `POST action=resetPagosFijos`)
- [ ] Actualizar el array `CIERRES` en enero 2027 con las fechas del nuevo año
- [ ] Exportar resumen mensual a PDF

---

## Stack

- **Frontend:** React 18 + Babel (browser) + DM Mono + Bricolage Grotesque
- **Backend:** Google Apps Script (Web App)
- **Base de datos:** Google Sheets
- **Hosting:** GitHub Pages
