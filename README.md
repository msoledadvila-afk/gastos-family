📊 Finanzas Familiares (Arg 🇦🇷) - Familia

Una aplicación web moderna, rápida y "serverless" diseñada para gestionar, proyectar y controlar los gastos familiares en el contexto económico argentino.
Esta aplicación no requiere servidores complejos ni instalaciones locales; funciona enteramente en el navegador y utiliza Google Sheets como base de datos, lo que permite tener un control total de la información cruda en formato Excel mientras se usa una interfaz visual de primer nivel.
<!-- Reemplazar con una URL real de una captura de la app -->
✨ Características Principales
Cero Instalaciones (Zero Build): Desarrollada en un único archivo index.html usando React, Tailwind y Recharts vía CDN. Lista para alojar gratis en GitHub Pages.
Base de Datos en Google Sheets: Todos los gastos se guardan en tiempo real en una planilla de Google mediante Google Apps Script.
Proyector de Cuotas Automático: Ingresá el monto total de una compra y la cantidad de cuotas. La app calcula la división y genera todos los vencimientos mes a mes automáticamente.
Dashboard Interactivo: Gráficos en tiempo real (gracias a Recharts) para ver distribución de gastos (fijos vs. variables), proyección de vencimientos futuros y responsabilidades.
Gestión de Tarjetas y Responsables: Diferenciación nativa entre los gastos de Gabi y Sole, asignando tarjetas específicas a cada uno (Ej: VISA Patagonia, MC Patagonia, BNA).
Control "Checklist": Marcá gastos como pagados con un solo clic para descontarlos de tu deuda pendiente.
🚀 Tecnologías Utilizadas
Frontend: React 18 (Standalone), Babel, Tailwind CSS (CDN).
Gráficos: Recharts.
Backend & Base de Datos: Google Apps Script + Google Sheets.
Hosting Recomendado: GitHub Pages.

⚙️ Cómo instalar y configurar tu propia versión
Dado que la aplicación funciona en un solo archivo HTML, el despliegue es increíblemente sencillo.

Parte 1: Configurar la Base de Datos (Google Sheets)
Crea un nuevo Google Sheets en tu cuenta de Google Drive.
Nombra la primera pestaña como Gastos.
En la primera fila (A1 hasta K1), escribe exactamente estos encabezados:
ID | Fecha | Descripcion | Monto | Categoria | Tipo | Responsable | Division | Tarjeta | Estado | Timestamp
Ve a Extensiones > Apps Script.
Borra el código que aparece y pega el script de backend (puedes encontrar el código del script en la documentación del proyecto o solicitarlo).
Haz clic en Implementar > Nueva implementación.
Selecciona Aplicación Web.
Ejecutar como: Yo.
Quién tiene acceso: Cualquier persona.
Copia la URL de la Aplicación Web que te generará Google.

Parte 2: Configurar la App (Frontend)
Haz un fork o clona este repositorio.
Abre el archivo index.html.
Busca la constante SCRIPT_URL (alrededor de la línea 20).
Reemplaza el valor por la URL que copiaste de Google Apps Script:
const SCRIPT_URL = "[https://script.google.com/macros/s/TU_CODIGO_AQUI/exec](https://script.google.com/macros/s/TU_CODIGO_AQUI/exec)"; 
Guarda el archivo.

Parte 3: Desplegar gratis en GitHub Pages
Sube tu archivo index.html modificado a tu repositorio de GitHub.
En tu repositorio, ve a Settings (Configuración) > Pages (Páginas).
En Source (Fuente), selecciona la rama main (o master) y guarda.
En unos minutos, GitHub te dará un link público (ej: https://tu-usuario.github.io/finanzas). ¡Ya puedes entrar desde tu celular o PC y empezar a usarla!

💡 Flujo de Trabajo Diario
Carga rápida: Usa el "Gasto Simple" para facturas de luz, supermercado, etc.
Compras Grandes: Usa el "Plan Cuotas" para compras con tarjeta. La app proyectará tu deuda a futuro.
El día de cobro/pago: Ve a la pestaña Lista, filtra por "Solo Pendientes" y ve tildando los círculos a medida que vas pagando desde tu home banking.

🤝 Contribuciones
Este es un proyecto personal, pero las mejoras (como nuevos gráficos, reportes de PDF o exportación a CSV) son bienvenidas. Siéntete libre de hacer un Fork y un Pull Request.
Diseñado con el 🩵 para sobrevivir a la economía argentina.
