# FOOAX · Verificación Domiciliaria del Equipo
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
