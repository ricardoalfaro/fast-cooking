# 📋 Backlog de Mejoras - FastCooking

Este documento registra y organiza las ideas de mejora, nuevas características y refactorizaciones técnicas planificadas para **FastCooking**. Las tareas están ordenadas y evaluadas según su **Prioridad** y **Dificultad Técnica**.

---

## 🚦 Tabla de Clasificación de Tareas

| Característica / Tarea | Prioridad | Dificultad Técnica | Estado | Descripción / Notas |
| :--- | :---: | :---: | :---: | :--- |
| **Bucle de Recarga de Service Worker** | 🔥 Alta | 🟢 Baja | ⏳ Pendiente | Evitar recargas infinitas o innecesarias al actualizar la API Key. |
| **Búsqueda Histórica y Ciclo de 6 Meses** | 🔥 Alta | 🟡 Media | ⏳ Pendiente | Guardar todas las búsquedas en `localStorage` y priorizarlas al buscar. Auto-limpiar recetas no consultadas en 6 meses. Solo los favoritos persisten por siempre. |
| **Cómputo de Porciones Dinámico** | 🔥 Alta | 🟡 Media | ⏳ Pendiente | Botón `+/-` para ajustar las porciones (ej: de 4 a 2 o 6 personas) y recalcular automáticamente las cantidades de los ingredientes. |
| **Temporizador Interno (Timer)** | 🟡 Media | 🟡 Media | ⏳ Pendiente | Reloj/alarma flotante integrado en la app para programar alertas de cocción (ej: los "25 minutos en olla a presión"). |
| **Rediseño de la Interfaz (UI/UX)** | 🟡 Media | 🟡 Media | ⏳ Pendiente | Pulir el diseño visual, espaciados y jerarquía tipográfica para que sea más premium y fácil de leer mientras se cocina. |
| **Modo Oscuro y Automático (System)** | 🟡 Media | 🟢 Baja | ⏳ Pendiente | Soporte para tema oscuro basado en la preferencia del sistema operativo del celular. |
| **Miniatura / Portada de YouTube** | 🟢 Baja | 🟢 Baja | ⏳ Pendiente | Cargar la miniatura real del video de YouTube (`https://img.youtube.com/vi/ID/0.jpg`) como portada de la receta. |
| **Enlaces a Recetas de Alternativas** | 🟢 Baja | 🟢 Baja | ⏳ Pendiente | Extraer y adjuntar los enlaces de YouTube o web para los YouTubers sugeridos en la sección secundaria. |
| **Compartir Receta (WhatsApp/Copy)** | 🟢 Baja | 🟢 Baja | ⏳ Pendiente | Botón para copiar ingredientes o enviar el enlace directo de la receta con un toque. |
| **Modularización de Archivos** | 🟢 Baja | 🟢 Baja | ⏳ Pendiente | Separar el JavaScript de `index.html` en un archivo `app.js` dedicado para facilitar mantenimiento y orden en el repositorio. |
| **Contador de Usos (Frecuencia)** | 🟢 Baja | 🟢 Baja | ⏳ Pendiente | Registrar cuántas veces se ha cocinado una receta favorita y ordenarlas por frecuencia en la pantalla de inicio. |

---

## 🔍 Detalle de Propuestas y Criterios Técnicos

### 1. Búsqueda Histórica y Ciclo de Vida de 6 Meses
* **Propósito:** Evitar llamadas a la API de Gemini para recetas ya consultadas previamente, reduciendo el consumo de tokens y acelerando el tiempo de respuesta.
* **Detalle Técnico:**
  - Al buscar una receta, primero se revisa el `localStorage` (haciendo búsqueda difusa).
  - Cada receta guardada en el `localStorage` debe incluir una propiedad `lastConsulted` con la marca de tiempo (timestamp).
  - Al cargar la aplicación, se corre una rutina de limpieza: cualquier receta con `lastConsulted` mayor a 6 meses de antigüedad se elimina de la caché local, a menos que esté marcada en el listado de favoritos (`isFavorite`), los cuales persisten para siempre.

### 2. Cómputo de Porciones Dinámico
* **Propósito:** Adaptar la receta a la cantidad de comensales reales.
* **Detalle Técnico:** 
  - La receta devuelta por Gemini debe ser parseada para extraer números y unidades (ej. `500g` ➔ `125g por porción`).
  - Implementar funciones JS con expresiones regulares para multiplicar/dividir cantidades de ingredientes dinámicamente en el DOM al presionar los controles de porciones.

### 3. Temporizador Flotante de Cocina
* **Propósito:** Ayudar a controlar los tiempos de cocción sin salir de la aplicación.
* **Detalle Técnico:**
  - Crear un widget de cuenta regresiva en la interfaz que use `Notification API` y alertas sonoras nativas del navegador.
  - Habilitar que al tocar un tip que mencione tiempo (ej. "Tiempo: 25 minutos"), se configure automáticamente una alarma con esa duración.

### 4. Modo Oscuro y Automático
* **Propósito:** Mejorar la visibilidad de la pantalla de noche o en entornos con iluminación cambiante en la cocina.
* **Detalle Técnico:**
  - Implementar clases de Tailwind (`dark:...`) y escuchar la preferencia del sistema a través de `window.matchMedia('(prefers-color-scheme: dark)')` para cambio automático.

### 5. Miniatura de YouTube como Portada
* **Propósito:** Hacer la ficha de la receta visualmente más atractiva y fácil de reconocer.
* **Detalle Técnico:**
  - Extraer el ID del video desde la `baseUrl` del JSON (usando regex para pescar el parámetro `v=`).
  - Cargar el asset de miniatura directamente desde los servidores de Google utilizando la URL: `https://img.youtube.com/vi/[VIDEO_ID]/mqdefault.jpg`.

### 6. Enlaces a Recetas de Alternativas
* **Propósito:** Que el usuario pueda ver las preparaciones secundarias de los otros YouTubers si no le convence el método principal.
* **Detalle Técnico:**
  - Ajustar el prompt de Gemini para solicitar las URLs de los videos específicos de las alternativas de YouTubers propuestas y renderizarlas como botones discretos en cada tarjeta de alternativa.
