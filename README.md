# zymplo-multipais-dashboard

Sitio HTML estático que renderiza el `MULTIPAIS-DASHBOARD.md` del monorepo principal · hosteado en Cloudflare Pages como `zymplo-multipais.kuarahytech.com.py`.

## Por qué este repo separado

El monorepo `zymplo-inc/zymplo` requiere aprobación de admin de la org GitHub para conectar Cloudflare Pages. Como workaround, este repo público en mi cuenta personal hostea solo los archivos del dashboard (HTML + .md sync) sin necesidad de aprobación.

**Source of truth**: `MULTIPAIS-DASHBOARD.md` en `zymplo-inc/zymplo`. Este repo es solo un mirror para hosting.

## Sync manual del .md (cada vez que cambia upstream)

```bash
# Desde el monorepo zymplo
cd /home/luzes/zymplo
cp MULTIPAIS-DASHBOARD.md /home/luzes/zymplo-multipais-dashboard/dashboard.md

# Push al repo público
cd /home/luzes/zymplo-multipais-dashboard
git add dashboard.md
git commit -m "sync: update dashboard.md from upstream"
git push
```

Cloudflare Pages auto-deploya en cada push a `main` · 30-60 segundos después la URL está actualizada.

## Setup inicial (one-time · post crear el repo en GitHub)

1. Crear repo público vacío en GitHub: `https://github.com/luzma101190/zymplo-multipais-dashboard` (sin README, sin .gitignore · vacío)
2. Push local:
   ```bash
   cd /home/luzes/zymplo-multipais-dashboard
   git remote add origin git@github.com:luzma101190/zymplo-multipais-dashboard.git
   git branch -M main
   git push -u origin main
   ```
3. Cloudflare Pages:
   - `dash.cloudflare.com` → **Workers & Pages** → **Create application** → **Pages** → **Connect to Git**
   - Seleccionar org `luzma101190` (no te va a pedir aprobación porque es tu cuenta personal)
   - Repo: `zymplo-multipais-dashboard`
   - Settings:
     - **Project name**: `zymplo-multipais` (define `zymplo-multipais.pages.dev`)
     - **Production branch**: `main`
     - **Framework preset**: None
     - **Build command**: (vacío · todo está pre-built)
     - **Build output directory**: `/` (raíz)
     - **Root directory**: (vacío · raíz del repo)
   - **Save and Deploy** → 30-60 seg
4. Custom domain:
   - En el project recién creado: **Custom domains** → **Set up a custom domain**
   - Domain: `zymplo-multipais.kuarahytech.com.py`
   - Cloudflare crea automático el CNAME en la zona `kuarahytech.com.py`
   - Espera 1 min de propagation · listo

## Auto-sync futuro (opcional · cuando tengas tiempo)

Idea: un workflow en `zymplo-inc/zymplo` que en cada push a `develop` que cambie `MULTIPAIS-DASHBOARD.md`, copie el archivo a este repo via PAT. Pendiente · por ahora sync manual con el comando de arriba.

## Estructura

- `index.html` · página HTML que fetcha y renderiza `dashboard.md`
- `dashboard.md` · copia del `MULTIPAIS-DASHBOARD.md` upstream
- `_headers` · config Cloudflare Pages (cache headers, security headers)
