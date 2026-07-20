# 📋 Backlog de Mejoras - FastCooking

Este documento registra y organiza las ideas de mejora, nuevas características y refactorizaciones técnicas planificadas para **FastCooking**. Las tareas están ordenadas y evaluadas según su **Prioridad** y **Dificultad Técnica**.

---

## 🚦 Tabla de Clasificación de Tareas

| Característica / Tarea | Prioridad | Dificultad Técnica | Estado | Descripción / Notas |
| :--- | :---: | :---: | :---: | :--- |
| **Bucle de Recarga de Service Worker** | 🔥 Alta | 🟢 Baja | ⏳ Pendiente | Evitar recargas infinitas o innecesarias al actualizar la API Key. |
| **Cómputo de Porciones Dinámico** | 🔥 Alta | 🟡 Media | ⏳ Pendiente | Botón `+/-` para ajustar las porciones (ej: de 4 a 2 o 6 personas) y recalcular automáticamente las cantidades de los ingredientes. |
| **Temporizador Interno (Timer)** | 🟡 Media | 🟡 Media | ⏳ Pendiente | Reloj/alarma flotante integrado en la app para programar alertas de cocción (ej: los "25 minutos en olla a presión"). |
| **Búsqueda Offline / Caché Local** | 🟡 Media | 🟡 Media | ⏳ Pendiente | Si el usuario está offline o no tiene API Key, buscar coincidencias dentro del historial de recetas locales guardadas. |
| **Compartir Receta (WhatsApp/Copy)** | 🟢 Baja | 🟢 Baja | ⏳ Pendiente | Botón para copiar ingredientes o enviar el enlace directo de la receta con un toque. |
| **Modularización de Archivos** | 🟢 Baja | 🟢 Baja | ⏳ Pendiente | Separar el JavaScript de `index.html` en un archivo `app.js` dedicado para facilitar mantenimiento y orden en el repositorio. |
| **Contador de Usos (Frecuencia)** | 🟢 Baja | 🟢 Baja | ⏳ Pendiente | Registrar cuántas veces se ha cocinado una receta favorita y ordenarlas por frecuencia en la pantalla de inicio. |

---

## 🔍 Detalle de Propuestas y Criterios Técnicos

### 1. Cómputo de Porciones Dinámico
* **Propósito:** Adaptar la receta a la cantidad de comensales reales.
* **Detalle Técnico:** 
  - La receta devuelta por Gemini debe ser parseada para extraer números y unidades (ej. `500g` ➔ `125g por porción`).
  - Implementar funciones JS con expresiones regulares para multiplicar/dividir cantidades de ingredientes dinámicamente en el DOM al presionar los controles de porciones.

### 2. Temporizador Flotante de Cocina
* **Propósito:** Ayudar a controlar los tiempos de cocción sin salir de la aplicación.
* **Detalle Técnico:**
  - Crear un widget de cuenta regresiva en la interfaz que use `Notification API` y alertas sonoras nativas del navegador.
  - Habilitar que al tocar un tip que mencione tiempo (ej. "Tiempo: 25 minutos"), se configure automáticamente una alarma con esa duración.

### 3. Modo Desconectado Robusto (Búsqueda Offline)
* **Propósito:** Permitir cocinar recetas previamente consultadas incluso si estás en un lugar sin cobertura o internet.
* **Detalle Técnico:**
  - Modificar el buscador en `index.html` para buscar coincidencias parciales dentro de la colección `fastcooking_custom_recipes` almacenada en `localStorage` antes de lanzar error de conexión.

### 4. Modularización de Código
* **Propósito:** Mantener la PWA fácil de depurar mediante Pull Requests (PRs).
* **Detalle Técnico:**
  - Extraer estilos personalizados a un archivo `style.css`.
  - Migrar la base de datos base y la lógica a un archivo `app.js`.
  - Configurar las referencias relativas correspondientes en `sw.js` para asegurar que todo se siga cacheando de forma correcta.
