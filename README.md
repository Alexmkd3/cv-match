# CV Match App — Setup completo

## Lo que vas a tener al final
Una URL pública tipo `https://tu-usuario.github.io/cv-match/` con el app funcionando.

---

## PARTE 1 — Cloudflare Worker (proxy de la API key)

### 1. Crea cuenta en Cloudflare
Ve a https://cloudflare.com → Sign Up (gratis).

### 2. Activa Workers
Dashboard → Workers & Pages → Get started.

### 3. Crea el Worker
- Clic en **Create** → **Create Worker**
- Dale un nombre: `cv-match-proxy`
- Clic en **Deploy** (con el código de ejemplo que aparece)
- Luego clic en **Edit code**
- Borra todo el código que aparece
- Pega el contenido completo de `worker.js` de este proyecto
- Clic en **Deploy**

### 4. Agrega tu API key como Secret
- En el Worker → Settings → Variables → **Add variable**
- Tipo: **Secret**
- Nombre: `ANTHROPIC_API_KEY`
- Valor: tu API key de Anthropic (la encuentras en https://console.anthropic.com)
- Clic en **Deploy**

### 5. Copia la URL de tu Worker
Se ve así: `https://cv-match-proxy.TU-SUBDOMINIO.workers.dev`
Guárdala — la necesitas en el paso siguiente.

---

## PARTE 2 — GitHub Pages

### 1. Crea un repositorio en GitHub
- Ve a https://github.com → New repository
- Nombre: `cv-match`
- Público
- Clic en **Create repository**

### 2. Sube el archivo index.html
- En el repo recién creado → **Add file** → **Upload files**
- Sube `index.html`
- Commit: "Initial commit"

### 3. Activa GitHub Pages
- Settings → Pages → Source: **Deploy from a branch**
- Branch: `main` / `root`
- Save

Tu URL quedará: `https://TU-USUARIO.github.io/cv-match/`
(puede tardar 1-2 minutos en activarse)

---

## PARTE 3 — Conectar todo

Abre `index.html` y busca esta línea (cerca del final del script):

```js
const WORKER_URL = 'https://TU_WORKER.TU_SUBDOMINIO.workers.dev';
```

Reemplaza con la URL real de tu Worker del Paso 5 anterior.

También actualiza los links del CTA:
```html
<a href="https://calendly.com/TU_USUARIO" ...>
<a href="https://www.linkedin.com/in/TU_PERFIL" ...>
```

Luego vuelve a subir el `index.html` actualizado a GitHub.

---

## PARTE 4 — (Opcional pero recomendado) Restringir el Worker a tu dominio

En `worker.js`, cambia:
```js
const ALLOWED_ORIGIN = '*';
```
Por:
```js
const ALLOWED_ORIGIN = 'https://TU-USUARIO.github.io';
```

Esto evita que alguien use tu Worker desde otro sitio.

---

## Costos estimados

| Servicio | Costo |
|---|---|
| GitHub Pages | Gratis |
| Cloudflare Workers | Gratis hasta 100,000 requests/día |
| Anthropic API | ~$0.003 por análisis (Claude Sonnet) |

Con 50 análisis/día → menos de $5 USD/mes en API.

---

## ¿Problemas?

- **El app no carga:** verifica que GitHub Pages esté activado y espera 2 min
- **Error al analizar:** verifica que la URL del Worker esté bien escrita en index.html
- **Error 401 en el Worker:** la API key no está bien configurada como Secret
