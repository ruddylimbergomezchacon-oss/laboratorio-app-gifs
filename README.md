# App de GIFs — Laboratorio React

Aplicación desarrollada con React + Vite, siguiendo como referencia visual la captura proporcionada.

## Funcionalidades
- Búsqueda de GIFs por palabra o frase.
- Categorías iniciales: Dragon Ball, Valorant y One Punch Man.
- Resultados en tarjetas con imagen y título.
- Vista ampliada al seleccionar una tarjeta.
- Botón para cargar más resultados.
- Mensajes de carga, errores y búsqueda sin resultados.
- Diseño adaptable a computadora y celular.

## 1. Requisitos
- Node.js 18 o superior.
- Una API Key de GIPHY.

## 2. Obtener la API Key
1. Crear una cuenta en https://developers.giphy.com/
2. Crear una aplicación desde el panel de desarrolladores.
3. Copiar la API Key.

## 3. Ejecutar el proyecto
En la carpeta del proyecto, abrir una terminal y ejecutar:

```bash
npm install
```

Crear un archivo llamado `.env.local` en la raíz (junto a package.json) con este contenido:

```env
VITE_GIPHY_API_KEY=PEGA_AQUI_TU_API_KEY
```

Reemplazar el texto de ejemplo por la clave real. Después ejecutar:

```bash
npm run dev
```

Abrir la dirección local que indique Vite (normalmente http://localhost:5173).

## 4. Generar versión de producción
```bash
npm run build
npm run preview
```
La carpeta lista para publicar se genera en `dist`.

## 5. Publicar en GitHub Pages
1. Crear un repositorio público en GitHub, por ejemplo `laboratorio-app-gifs`.
2. Subir todos los archivos del proyecto excepto `node_modules`, `dist` y `.env.local`.
3. En GitHub abrir **Settings → Pages**.
4. En **Build and deployment**, elegir **GitHub Actions**.
5. Crear el archivo `.github/workflows/deploy.yml` con el flujo de publicación (ver más abajo).
6. En GitHub, abrir **Settings → Secrets and variables → Actions → New repository secret**.
7. Nombre: `VITE_GIPHY_API_KEY`; valor: la API Key de GIPHY.
8. Guardar. GitHub Actions construirá y publicará el sitio. La URL aparecerá en Settings → Pages.

## 6. Publicar en Netlify
1. Entrar a https://app.netlify.com/ e iniciar sesión.
2. Elegir **Add new site → Import an existing project**.
3. Conectar GitHub y seleccionar el repositorio.
4. Build command: `npm run build`.
5. Publish directory: `dist`.
6. En **Site configuration → Environment variables**, agregar `VITE_GIPHY_API_KEY` con la API Key.
7. Volver a desplegar el sitio. Netlify mostrará la URL pública.

## Flujo de GitHub Actions
Crear `.github/workflows/deploy.yml`:

```yaml
name: Deploy React App to GitHub Pages
on:
  push:
    branches: [main]
  workflow_dispatch:
permissions:
  contents: read
  pages: write
  id-token: write
concurrency:
  group: pages
  cancel-in-progress: true
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm
      - run: npm install
      - run: npm run build
        env:
          VITE_GIPHY_API_KEY: ${{ secrets.VITE_GIPHY_API_KEY }}
      - uses: actions/upload-pages-artifact@v3
        with:
          path: dist
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
```

## Referencias
- Documentación oficial de búsqueda GIPHY: https://developers.giphy.com/docs/api/endpoint/search/
- Documentación general de GIPHY: https://developers.giphy.com/docs/api/

## Importante
La API Key es necesaria para que las búsquedas funcionen. No subir `.env.local` a GitHub. En despliegues se configura la variable de entorno en GitHub Actions o Netlify.
