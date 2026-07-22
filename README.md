# FOOAX · Verificación Domiciliaria del Equipo

App para registrar la verificación domiciliaria de Gerentes de Zona, Promotoras de Crédito, Supervisores/Coordinadores y personal de oficina, siguiendo el formato de la página 6 de la política.

## 1. Crear la tabla en Supabase

En tu proyecto de Supabase (`wbkznjbccjivlsveqqwp`), abre el **SQL Editor** y corre esto:

```sql
create table if not exists verificaciones_domicilio_equipo (
  id uuid default gen_random_uuid() primary key,
  folio text,
  nombre_colaborador text,
  puesto text,
  domicilio_contrato text,
  cp text,
  municipio text,
  colonia text,
  telefono text,
  nombre_supervisor text,
  fecha_visita date,
  direccion_coincide boolean,
  direccion_real text,
  tipo_vivienda text,
  habita_ahi boolean,
  observacion_habita text,
  estado_vivienda text,
  accesibilidad text,
  tipo_comprobante text,
  fecha_comprobante date,
  comprobante_a_nombre_de text,
  ref1_nombre text,
  ref1_relacion text,
  ref1_confirma boolean,
  ref2_nombre text,
  ref2_relacion text,
  ref2_confirma boolean,
  foto_consentimiento boolean,
  foto_base64 text,
  foto_folio text,
  observaciones_adicionales text,
  resultado text,
  firma_colaborador text,
  firma_supervisor text,
  gps_lat numeric,
  gps_lng numeric,
  created_at timestamp default now()
);

alter table verificaciones_domicilio_equipo disable row level security;
grant all on verificaciones_domicilio_equipo to anon;
```

Esto sigue el mismo patrón que ya usas en las otras tablas (`supervisiones`, `clientas`, etc.): RLS desactivado + GRANT a `anon`, para evitar los errores 401.

**Nota sobre `foto_base64`:** la foto se guarda como texto base64 directo en la tabla para simplicidad (igual que el resto del ecosistema). Si el volumen de fotos crece mucho, en algún momento puede convenir moverlas a un bucket de Supabase Storage en vez de la tabla — pero para empezar y probar, esto funciona bien.

## 2. Supabase ya configurado

El `index.html` ya trae pegados la **Project URL** y la llave **anon** de tu proyecto `wbkznjbccjivlsveqqwp` — no necesitas tocar nada aquí. Sólo asegúrate de haber corrido el SQL del paso 1 en ese mismo proyecto.

## 3. Subir a GitHub y desplegar en Vercel

Mismo flujo que ya conoces:
1. Crea un repo nuevo en GitHub (sugerido: `fooax-verificacion-domicilio-equipo`).
2. Sube estos 5 archivos: `index.html`, `manifest.json`, `sw.js`, `icons/icon-192.png`, `icons/icon-512.png`, `icons/icon-maskable-512.png`.
3. Conecta el repo a Vercel como proyecto estático (no necesita build command).

## Qué hace la app

- **Formulario en 7 pasos**, siguiendo exactamente el protocolo del documento: datos del colaborador → verificación física → comprobante de domicilio → 2 referencias del entorno → evidencia/observaciones → resultado → firmas.
- **Firma digital** (colaborador y supervisor) con el dedo, directo en pantalla.
- **Foto exterior** vía cámara del celular, con checkbox de consentimiento explícito (nunca interior, según la política).
- **GPS opcional** de la visita.
- **Guardado en Supabase**, con respaldo local (`localStorage`) si no hay conexión — se sincroniza solo cuando vuelve el internet.
- **Historial** con las últimas 50 verificaciones, incluyendo las que están pendientes de sincronizar.
- Es una **PWA instalable** (ícono en pantalla de inicio, funciona sin conexión para llenar formularios).

## Notas de diseño

Paleta jade/marigold inspirada en la identidad FOOAX ("Creciendo juntas"), con numeración de pasos porque el protocolo real es literalmente secuencial (los 7 pasos del documento). Pensado para llenarse en el celular, en la puerta de la casa, en los 20-30 minutos que dura la visita.
