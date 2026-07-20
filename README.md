# ⚡ FastCooking - Recetas Rápidas con Gemini

Una Progressive Web App (PWA) minimalista, responsiva y diseñada específicamente para usarse en el celular mientras cocinas en el hogar. Al grano, sin introducciones largas y con los tiempos exactos.

---

## 🍳 Propósito y Características

¿Te ha pasado que quieres cocinar viendo un video de YouTube, pero tienes que estar retrocediendo, buscando los ingredientes en la descripción o tocando la pantalla con las manos sucias porque el celular se suspendió?

**FastCooking** soluciona esto:
- **Buscador Inteligente:** Escribe lo que quieres cocinar y la app buscará en YouTube el video correcto (priorizando las recetas del chef chileno Álvaro Barrientos).
- **Ingredientes Interactivos:** Una lista simple con casillas de verificación para ir tachando los ingredientes a medida que los preparas (*Mise en place*).
- **Tips con Timestamps:** Consejos clave ordenados cronológicamente. Cada tip tiene un enlace directo que abre el video de YouTube en el **segundo exacto** de la recomendación.
- **Modo "Pantalla On":** Un botón para evitar que la pantalla de tu celular se apague automáticamente mientras tienes las manos con harina o agua.
- **Tus Favoritos:** Guarda tus recetas más exitosas en el inicio para acceder a ellas con un solo toque.

---

## 🛠️ Detalles Técnicos

La aplicación está diseñada bajo el principio de simplicidad y portabilidad absoluta. Es un sitio estático autocontenido que se ejecuta en su totalidad en el cliente.

### Arquitectura y Tecnologías:
- **Core:** Archivo único estático `index.html` para máxima velocidad de carga.
- **Estilos:** Tailwind CSS CDN con fuentes de Google (*Plus Jakarta Sans* y *Outfit*).
- **Integración IA (Gemini API):**
  - Conexión cliente-servidor directa a los modelos de Google AI Studio (`v1beta`).
  - **Autodetección Dinámica de Modelos:** Consulta el endpoint `GET /models` con la clave de API para detectar y utilizar automáticamente el modelo Flash activo en tu cuenta (ej. `gemini-3.5-flash` o `gemini-3.0-flash`), evitando fallas por descontinuación de modelos anteriores.
  - **Google Search Grounding:** Envía la solicitud con la herramienta `google_search` activa para que Gemini rastree en tiempo real los enlaces watch directos y los contenidos del video en YouTube.
  - **Estrategia Autocurativa (Auto-healing):** Si la clave de API tiene restricciones para usar herramientas de búsqueda en Google (común en planes gratuitos), la app atrapa el error y reintenta de inmediato la consulta de forma directa sin herramientas.
  - **Parseo Robusto:** Extracción estructurada del JSON mediante expresiones regulares (`Regex`) aislando cualquier formato Markdown.
- **Soporte PWA:**
  - `manifest.json`: Permite agregar la aplicación a la pantalla de inicio del celular.
  - `sw.js` (Service Worker): Implementa una estrategia de almacenamiento en caché **Network-First** para asegurar que los cambios de desarrollo se reflejen de inmediato al estar online, pero mantenga el soporte offline al cocinar sin internet.
- **Kitchen Utilities:**
  - **Wake Lock API:** Mantiene el estado activo de la pantalla del dispositivo de forma nativa.
  - **localStorage:** Almacenamiento local para la API Key de Gemini y la lista de recetas favoritas.

---

## 🚀 Instalación y Despliegue en GitHub Pages

1. **Clona el proyecto:**
   ```bash
   git clone https://github.com/ricardoalfaro/fast-cooking.git
   cd fast-cooking
   ```
2. **Súbelo a tu hosting estático favorito** o activa **GitHub Pages** desde la sección *Settings > Pages* de tu repositorio seleccionando la rama `main`.
3. Abre el sitio en tu celular, pulsa en el menú del navegador "Agregar a la pantalla de inicio" para instalarla como PWA.
4. Obtén tu llave de API gratuita en [Google AI Studio](https://aistudio.google.com/), configúrala en el icono de llave (`🔑`) y ¡listo para cocinar!
