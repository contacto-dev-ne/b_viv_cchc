# Visualizador de Balance de Vivienda - Maule

Este proyecto es una versión autónoma del visualizador de vivienda, diseñada para ser desplegada en **GitHub Pages**.

Los datos se obtienen desde **Excel Online** mediante **Power Automate**.

## Cómo configurar en GitHub:

### 1. Crear el repositorio
Sube todos los archivos de esta carpeta a un nuevo repositorio en GitHub (incluyendo la carpeta `.github`).

### 2. Configurar el Secreto
Para que los datos se conecten de forma segura, ve a tu repositorio en GitHub:
1. Ve a **Settings (Configuración)** > **Secrets and variables** > **Actions**.
2. Haz clic en **New repository secret**.
3. Añade el siguiente secreto:
   - `POWER_AUTOMATE_URL`: La URL completa del trigger HTTP de tu flujo de Power Automate.

### 3. Configurar Power Automate
Tu flujo de Power Automate debe tener la siguiente estructura:
1. **Trigger**: "When a HTTP request is received" (POST)
2. **Acción**: "List rows present in a table" → Excel Online → Tu archivo → Tabla1
3. **Acción**: "Response" → Devolver el body de la acción anterior como JSON

> ⚠️ Asegúrate de que la respuesta de Power Automate incluya los headers CORS:
> - `Access-Control-Allow-Origin: *`
> - `Content-Type: application/json`

### 4. Activar GitHub Pages
Una vez que subas el código, el archivo en `.github/workflows/deploy.yml` creará automáticamente una rama llamada `gh-pages`.
1. Ve a **Settings** > **Pages**.
2. En "Build and deployment", asegúrate de que esté seleccionado "Deploy from a branch".
3. Selecciona la rama `gh-pages` y la carpeta `/(root)`.
4. ¡Guarda y listo! Tu aplicación estará en una URL como `https://tu-usuario.github.io/tu-repositorio/`.

## Estructura del Flujo en Power Automate

```
Trigger HTTP (POST)
    ↓
List rows present in a table (Excel Online)
    - Ubicación: OneDrive / SharePoint
    - Archivo: [tu archivo Excel]
    - Tabla: Tabla1
    ↓
Response (Status 200)
    - Body: @{body('List_rows_present_in_a_table')?['value']}
    - Headers: { "Content-Type": "application/json", "Access-Control-Allow-Origin": "*" }
```

## Columnas esperadas en Excel (Tabla1)
| Columna | Tipo | Descripción |
|---------|------|-------------|
| Año | Número | Año del registro |
| Comuna | Texto | Nombre de la comuna |
| Provincia | Texto | Nombre de la provincia (opcional, se calcula automáticamente) |
| Requerimientos | Número | Cantidad de hogares con requerimiento |
| Sexo | Texto | Sexo del jefe de hogar |
| Zona | Texto | Urbano / Rural |
| Grupo_socioeconomico | Texto | Grupo socioeconómico |
| Componente_deficit | Texto | Componente del déficit habitacional |
| Tipo_hogar | Texto | Tipo de hogar |
| Educacion | Texto | Nivel educacional |

## Notas Técnicas
- El visualizador utiliza React y Recharts vía CDN para no requerir un proceso de construcción complejo.
- La URL de Power Automate se inyecta en el archivo `index.html` durante el despliegue mediante GitHub Actions, por lo que nunca estará visible en tu código fuente principal (`main`).
- El flujo de Power Automate debe responder con los headers CORS adecuados para permitir peticiones desde GitHub Pages.
