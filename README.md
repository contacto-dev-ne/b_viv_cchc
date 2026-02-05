# Visualizador de Balance de Vivienda - Maule

Este proyecto es una versión autónoma del visualizador de vivienda, diseñada para ser desplegada en **GitHub Pages**.

## Cómo configurar en GitHub:

### 1. Crear el repositorio
Sube todos los archivos de esta carpeta a un nuevo repositorio en GitHub (incluyendo la carpeta `.github`).

### 2. Configurar los Secretos
Para que los datos se conecten de forma segura, ve a tu repositorio en GitHub:
1. Ve a **Settings (Configuración)** > **Secrets and variables** > **Actions**.
2. Haz clic en **New repository secret**.
3. Añade los siguientes dos secretos:
   - `VITE_GOOGLE_API_KEY`: Pon aquí tu clave API de Google Cloud.
   - `VITE_HOUSING_SHEET_ID`: Pon aquí el ID de tu Google Sheet (lo encuentras en la URL de tu hoja).

### 3. Activar GitHub Pages
Una vez que subas el código, el archivo en `.github/workflows/deploy.yml` creará automáticamente una rama llamada `gh-pages`.
1. Ve a **Settings** > **Pages**.
2. En "Build and deployment", asegúrate de que esté seleccionado "Deploy from a branch".
3. Selecciona la rama `gh-pages` y la carpeta `/(root)`.
4. ¡Guarda y listo! Tu aplicación estará en una URL como `https://tu-usuario.github.io/tu-repositorio/`.

## Notas Técnicas
- El visualizador utiliza React y Recharts vía CDN para no requerir un proceso de construcción complejo.
- Los secretos se inyectan en el archivo `index.html` durante el despliegue mediante GitHub Actions, por lo que nunca estarán visibles en tu código fuente principal (`main`).
