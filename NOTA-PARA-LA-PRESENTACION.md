# Aula Común — nota para tu exposición

## Qué es la app
Red social universitaria ficticia con login, registro, novedades,
reportes de accidentes, comunicados y cursos/talleres. Front-end puro:
usa `localStorage` del navegador para simular una base de datos (no hay
servidor real detrás).

## La vulnerabilidad que se demuestra
**Categoría OWASP:** A01:2021 — Broken Access Control (Control de acceso roto).

**Cómo se ve en la demo:**
1. `robots.txt` lista `/panel-admin/` en `Disallow`.
2. Esa ruta no está enlazada desde ningún botón visible del sitio.
3. Pero al visitar `/panel-admin/` directamente, la página carga sin pedir
   ninguna credencial de administrador.

**El punto para la clase:** ocultar una ruta (no enlazarla, listarla en
`robots.txt`) es "seguridad por oscuridad". No es control de acceso real,
porque:
- `robots.txt` es un archivo público, pensado para buscadores, no para
  restringir personas.
- Cualquier escáner automático de rutas (gobuster, dirbuster, etc.) prueba
  rutas comunes igual, con o sin `robots.txt`.
- Si la ruta se descubre (por `robots.txt`, por error, por fuerza bruta de
  rutas), no hay nada del lado del servidor que verifique que quien entra
  es realmente un administrador autenticado.

## Cómo se corrige en un sistema real
- Verificación de sesión y rol **del lado del servidor** en cada solicitud
  a rutas administrativas (no solo ocultar el enlace en el front-end).
- Responder `401 No autorizado` / `403 Prohibido` si la sesión no tiene el
  rol correcto — nunca simplemente "no publicar el link".
- No depender de `robots.txt` para nada relacionado a seguridad; su único
  propósito es indicarle a buscadores qué no indexar.

## Límite importante
Esta demo se construyó para practicar y presentar en un entorno propio y
controlado (tu propio proyecto de clase). El mismo patrón —imitar un sitio
real (por ejemplo un banco) para capturar credenciales de usuarios
reales— es phishing, y por eso se evitó desde el diseño: la app es
ficticia, sin marca real, sin captura de datos ajenos y sin conexión a
ningún backend fuera de tu propio navegador.
