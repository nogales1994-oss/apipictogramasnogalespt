# Pictogramas API

Web estática que publica y documenta una biblioteca de pictogramas PNG para
aplicaciones infantiles, educativas y de accesibilidad. Consumible por
desarrolladores y por agentes de IA.

- **React + Vite**, CSS moderno y sin dependencias visuales pesadas.
- **Sin backend ni base de datos**: archivos JSON y PNG estáticos en `/public`.
- Lista para desplegar en **Netlify** desde GitHub.

## Contenido

- `Inicio` — presentación de la biblioteca y ejemplo de consulta de la API.
- `Biblioteca` — buscador (nombre, categoría, etiquetas), filtros por categoría,
  cuadrícula responsive y modal con el detalle de cada pictograma.
- `Documentación` — cómo consumir `/api/pictograms.json`, ejemplos en
  JavaScript, React y HTML, URLs directas de imagen, categorías y convenciones.
- `Para IAs` — reglas para agentes de código y un prompt reutilizable.

## Estructura

```
.
├── public/
│   ├── api/pictograms.json        # Índice de la API estática
│   ├── openapi.json               # Especificación OpenAPI de la API
│   ├── llms.txt                   # Instrucciones cortas para agentes de IA
│   ├── pictogramas/
│   │   ├── frutas/                # manzana.png, platano.png, fresa.png, naranja.png
│   │   ├── verduras/              # zanahoria.png, esparrago.png
│   │   ├── carnes/
│   │   ├── pescados-y-mariscos/
│   │   ├── lacteos/
│   │   ├── panaderia-y-cereales/
│   │   ├── legumbres/
│   │   ├── bebidas/
│   │   ├── dulces-y-postres/
│   │   ├── platos-preparados/
│   │   └── utensilios-de-cocina/
│   └── favicon.svg
├── src/                          # Código de la web (React + Vite)
├── scripts/generate-pictograms.mjs  # Genera los PNG de ejemplo (500×500)
├── LICENSE                       # Licencia CC0 1.0
├── index.html
├── vite.config.js
├── netlify.toml
└── package.json
```

Los pictogramas son **PNG transparentes de 500×500**. Ejemplos:

- `/public/pictogramas/frutas/manzana.png`
- `/public/pictogramas/verduras/zanahoria.png`

Los nombres de archivo van en **minúsculas, sin tildes, sin espacios** y con
guiones solo cuando es necesario.

## API estática

El índice se sirve en `GET /api/pictograms.json`. Incluye un `baseUrl` con el
dominio de despliegue y URLs de imagen relativas:

```json
{
  "baseUrl": "https://tudominio.netlify.app",
  "items": [
    {
      "id": "manzana",
      "name": "Manzana",
      "category": "frutas",
      "imageUrl": "/pictogramas/frutas/manzana.png",
      "tags": ["fruta", "comida", "roja", "saludable"]
    }
  ]
}
```

Para una app externa o una IA, la URL absoluta de cada imagen es:

```js
const url = `${data.baseUrl}${item.imageUrl}`;
// "https://tudominio.netlify.app/pictogramas/frutas/manzana.png"
```

> Sustituye `baseUrl` por tu dominio real tras el primer despliegue.

La URL directa de cada imagen sigue este patrón:
`/pictogramas/<categoria>/<id>.png`.

La API también está descrita como especificación OpenAPI en
`/openapi.json`, útil para herramientas, clientes generados y agentes de IA.

## Puesta en marcha

Requisitos: Node.js 18 o superior.

```bash
npm install
npm run dev        # desarrollo en http://localhost:5173
npm run build      # build de producción en dist/
npm run preview    # previsualizar el build de producción
```

Generar de nuevo los pictogramas de ejemplo:

```bash
npm run generate:pictograms
```

## Despliegue en Netlify

1. Sube el repositorio a GitHub.
2. En Netlify, crea un nuevo sitio desde el repositorio.
3. Build command: `npm run build` · Publish directory: `dist` (ya configurado
   en `netlify.toml`).
4. Despliega. La redirección SPA de `netlify.toml` permite rutas como
   `/biblioteca`, `/documentacion` o `/para-ias`.

## Licencia

El proyecto y los pictogramas se publican bajo **CC0 1.0 Universal** (dominio
público): uso libre sin atribución, también con fines comerciales. Ver
[`LICENSE`](LICENSE).

Los pictogramas de ejemplo actuales son placeholders generados para esta web;
sustitúyelos por tu biblioteca final manteniendo el mismo formato (PNG 500×500
de fondo transparente) y convención de nombres.