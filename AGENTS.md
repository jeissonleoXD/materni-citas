# AGENTS.md — MaterniCitas

Plataforma web para gestionar citas de control prenatal de madres gestantes (Colombia).
Proyecto académico SENA.
Prototipos de referencia: GA1-220501092-AA4-EV01. Requisitos: GA1-220501092-AA3-EV01.

## Cómo trabajar (léelo siempre)
- Haz UN cambio pequeño a la vez: un componente o una pantalla por petición.
- Antes de crear algo nuevo, revisa si ya existe un componente parecido en `src/components`.
- No inventes librerías, props ni APIs. Si no estás seguro, dilo y pregunta.
- No cambies archivos que no te pidieron. Muestra el cambio en un diff.
- Responde en español. Código y nombres de variables en inglés; textos de interfaz en español.
- Después de cada cambio, indica cómo probarlo (comando o pasos).

## Stack
- Vite + React + TypeScript + React Router
- Tailwind CSS + shadcn/ui + lucide-react
- React Hook Form + Zod (validaciones)
- TanStack Query (datos)
- Framer Motion (animación mínima)
- Recharts (gráficas del embarazo)
- Supabase (Auth, Postgres con RLS, Edge Functions)
- Pruebas: Vitest + Testing Library; Playwright para flujos completos

## Estructura
```
src/
  components/ui/     # shadcn (no editar a mano sin necesidad)
  components/        # componentes propios de MaterniCitas
  pages/             # una carpeta por pantalla (RF-01 ... RF-08)
  lib/               # supabase.ts, utils, schemas Zod
  hooks/
  types/
```

## Requisitos funcionales (id → pantalla)
- RF-01 Registro de usuaria → crear cuenta; confirmación por correo
- RF-02 Inicio de sesión → login, error con contador de intentos, bloqueo temporal de 15 min, recuperar contraseña
- RF-03 Datos del embarazo → FUM, EPS, IPS; cálculo de semanas y trimestre
- RF-04 Calendario de controles
- RF-05 Recordatorios automáticos (48 h y 2 h antes, SMS y correo)
- RF-06 Información educativa por etapa del embarazo
- RF-07 Gestión de citas: agendar, reprogramar, cancelar
- RF-08 Histórico de citas y controles realizados

RF-04, RF-05 y RF-07 viven en una sola pantalla ("Calendario y gestión de citas").

## Requisitos no funcionales (reglas de calidad)
- Usabilidad: lenguaje claro y simple, navegación sencilla, para personas con poca alfabetización digital.
- Seguridad: datos de salud protegidos según la **Ley 1581 de 2012** (usa siempre esta referencia, no "Habeas Data médico"). Cifrado y control de acceso.
- Rendimiento: acciones frecuentes (ver calendario, agendar) deben sentirse ágiles.
- Compatibilidad: probar en móvil, tablet y computador. Diseñar primero para móvil.
- Escalabilidad: componentes reutilizables, sin lógica duplicada.

## Diseño visual
Usa los tokens, no colores sueltos. Valores aproximados tomados de los prototipos; ajústalos si el diseño final es otro.
- Fondo: rosa muy claro `#FCEFF3`
- Superficie (tarjetas): blanco `#FFFFFF`
- Primario (botones, acentos): vino/marrón rojizo `#8B4535`
- Texto principal: marrón oscuro `#3B2420`
- Texto secundario: `#7A6560`
- Estados: éxito verde suave, error rojo suave, advertencia ámbar (siempre con ícono o texto, nunca solo color)

Tipografía (confirmar con el diseño): títulos en serif elegante (tipo Playfair Display), texto en sans-serif legible.
- Texto base mínimo 16 px; interlineado cómodo; líneas de menos de 80 caracteres.
- Títulos en mayúscula solo la primera letra (sentence case). No uses MAYÚSCULAS para etiquetas.

Principios:
- Estilo cálido, tranquilo y confiable; es salud materna, no una app de moda.
- Una sola cosa memorable por pantalla; el resto, sobrio.
- Evita el look genérico de IA: tarjetas idénticas con la misma sombra, degradados decorativos, numeración 01/02/03 si no es una secuencia real, una palabra resaltada en cada título.
- Movimiento solo cuando responde a una acción (abrir, confirmar, cancelar). Respeta `prefers-reduced-motion`.
- Bordes redondeados coherentes: más grandes en tarjetas y botones, más pequeños en campos.

## Textos de interfaz
- Voz activa y verbos claros: "Agendar control", "Guardar cambios", "Cancelar cita".
- El mismo nombre para la misma acción en todo el flujo (el botón dice "Reprogramar" y el aviso dice "Cita reprogramada").
- Errores: explican qué pasó y cómo arreglarlo, sin culpar a la usuaria.
- Pantallas vacías: invitan a actuar ("Aún no tienes controles. Agenda el primero").
- Lenguaje de Colombia, trato de "tú", sin tecnicismos.

## Accesibilidad (obligatorio en cada pantalla)
- Todo campo con `<label>` visible; errores asociados al campo (`aria-describedby`).
- Foco visible y navegación completa con teclado.
- Contraste suficiente en texto y botones.
- Áreas táctiles de al menos 44 px.
- Estados de carga, vacío y error en toda pantalla con datos.

## Seguridad y datos
- Nunca pongas claves o tokens en el código. Usa variables de entorno (`VITE_...` solo para valores públicos).
- La clave `service_role` de Supabase jamás va en el frontend.
- Toda tabla con datos de usuarias tiene RLS: cada usuaria solo ve sus propios datos.
- Valida con Zod en el formulario y de nuevo en el servidor.
- No registres en consola datos personales ni de salud.
- Cuenta inactiva hasta confirmar el correo. Bloqueo de 15 minutos tras 5 intentos fallidos (se implementa con una tabla de intentos).
- Consentimiento de tratamiento de datos obligatorio antes de crear la cuenta.

## Contenido médico
- El contenido educativo (RF-06) debe basarse en fuentes como Minsalud u OMS e indicar la fuente.
- No inventes recomendaciones médicas, dosis ni diagnósticos. Si falta información, deja un marcador `TODO: validar con fuente oficial`.
- Los textos de la aplicación son informativos; no reemplazan la consulta médica.

## Datos de ejemplo
- Genera datos ficticios en español colombiano (nombres, EPS, IPS, ciudades). Nunca datos reales.
- Mantén coherencia: la fecha de última menstruación debe producir las semanas de gestación que se muestran en pantalla, usando la fecha actual.
- Año actual en el pie de página (no "2024").

## Pruebas
- Cada caso de prueba del documento AA4 debe tener un test automatizado cuando sea posible.
- Nombre del test: `RF-0X: <qué se comprueba>`.
- Antes de dar una tarea por terminada: `npm run lint`, `npm run test` y `npm run build` sin errores.

## Despliegue
- Frontend estático desplegado desde GitHub (Cloudflare Pages o Vercel; pendiente de decidir).
- React Router necesita redirigir todas las rutas a `index.html`.
- Variables de entorno configuradas en el panel del servicio, nunca en el repositorio.

## Definición de terminado (por pantalla)
1. Se ve bien en móvil y en computador.
2. Cumple los criterios de aceptación del requisito.
3. Tiene estados de carga, vacío y error.
4. Pasa accesibilidad básica (teclado, etiquetas, contraste).
5. Tiene sus tests y no rompe lint ni build.
