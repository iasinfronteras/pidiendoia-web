# CLAUDE.md — Instrucciones para Claude Code
# pidiendoia-web (pidiendoia.com)

---

## Estado actual del sistema

```text
Baseline activo: CI/CD operativo
HEAD main:       e3dbc82
Rama activa:     ci/add-production-workflow
Framework:       Astro 7.x + Tailwind 4
Desplegado en:   pidiendoia.com
CI/CD:           GitHub Actions → Tailscale → Hetzner VPS
```

### Estructura del proyecto
```text
src/
├── components/   — componentes Astro reutilizables
├── layouts/      — layouts base
├── pages/        — rutas del sitio (file-based routing)
└── styles/       — estilos globales
public/           — assets estáticos
astro.config.mjs  — configuración Astro
```

### Pipeline CI/CD
```text
git push → GitHub Actions → build Astro → SSH vía Tailscale → VPS Hetzner
Stack: /srv/stack-edge (Nginx) sirve dist/ generado por astro build
```

---

## Reglas específicas para Claude Code

### Stack y convenciones
- Astro 7.x — file-based routing en `src/pages/`.
- Tailwind 4 — sistema de diseño Q-04 establecido.
- TypeScript habilitado — no usar `any` sin justificación.
- Node.js >= 22.12.0 requerido.

### Antes de cualquier commit
```bash
npm run build
npx astro check
```
Ambos deben pasar sin errores. No hacer commit si alguno falla.

### CI/CD — reglas críticas
- No modificar `.github/workflows/` sin instrucción explícita.
- El despliegue ocurre automáticamente en push a `main` — verificar build local antes.
- SSH al VPS va exclusivamente vía Tailscale — nunca TCP/22 público.
- Los archivos de producción viven en `/srv/stack-edge/` en el VPS — no en este repositorio.

### Lo que Claude Code NO debe hacer
- Modificar `astro.config.mjs` sin instrucción explícita.
- Cambiar la versión de Node.js en `engines` sin coordinación.
- Agregar dependencias sin justificación técnica clara.
- Hacer commit con errores de build o type check.
- Tocar archivos de infraestructura (`/srv/`, Docker, Nginx) desde este repositorio.
- Hacer push directo a `main` sin verificar build.

---

*CLAUDE.md actualizado — 07/SEP/2026*
