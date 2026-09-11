# Impresiones Inspo — versión Cloud

Esta versión guarda tus proyectos:
1. Localmente en cada dispositivo.
2. En Supabase cuando inicias sesión.
3. Sincronizados entre iPhone, PC y cualquier otro dispositivo usando la misma cuenta.

## Archivos importantes

- `index.html` — aplicación
- `config.js` — URL y clave pública de Supabase
- `supabase.sql` — crea la tabla y las reglas privadas de acceso
- `api/makerworld.js` — obtiene datos de MakerWorld
- `manifest.json` + `sw.js` — PWA
- iconos PNG

## 1. Crear Supabase

Crea un proyecto en Supabase.

Después abre:
**SQL Editor > New query**

Copia todo el contenido de `supabase.sql` y ejecútalo.

Esto crea la tabla `projects` y activa RLS para que cada cuenta solo pueda ver sus propios proyectos.

## 2. Activar login por email

En Supabase:
**Authentication > Providers > Email**

Déjalo activado.

Si quieres que la cuenta funcione inmediatamente sin confirmar correo, puedes desactivar temporalmente "Confirm email". Si lo mantienes activado, tendrás que confirmar el primer correo de registro.

## 3. Copiar las claves públicas

En Supabase:
**Project Settings > API**

Copia:
- Project URL
- anon/public key

Abre `config.js` y reemplaza:

```js
SUPABASE_URL: "PEGA_AQUI_TU_SUPABASE_URL",
SUPABASE_ANON_KEY: "PEGA_AQUI_TU_SUPABASE_ANON_KEY"
```

La `anon key` está pensada para usarse en el frontend. La seguridad real la aplican las políticas RLS del archivo SQL.

## 4. Publicar en Vercel

Sube esta carpeta completa a Vercel.

La carpeta `/api` debe permanecer exactamente así porque contiene la función que consulta MakerWorld.

## 5. iPhone

Abre la URL HTTPS de Vercel en Safari:
**Compartir > Añadir a pantalla de inicio**

Después:
- crea tu cuenta desde la app,
- inicia sesión con el mismo email en PC,
- los proyectos aparecerán en ambos dispositivos.

## Cómo funciona la sincronización

- Cada cambio se guarda primero localmente.
- Si tienes internet y sesión iniciada, se sube a Supabase.
- Al abrir otro dispositivo, se descargan y mezclan los proyectos.
- Si pierdes internet, puedes seguir usando la copia local.
- Al volver la conexión, vuelve a sincronizar.
- También existe el botón "Sincronizar ahora".
- Puedes seguir exportando un backup JSON manual.

## Privacidad

La tabla usa `user_id` y Row Level Security.
Un usuario autenticado solo puede leer, crear, editar o eliminar sus propias filas.
