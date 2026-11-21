# MaterniCitas

Plataforma web para gestionar las citas de control prenatal de madres gestantes en Colombia.

Proyecto académico del SENA — Tecnólogo en Análisis y Desarrollo de Software (ficha 3545712).

> **Estado:** en desarrollo. Ya existe la base técnica (Vite, React, Tailwind y shadcn/ui). Las funcionalidades se van construyendo requisito por requisito.

## Equipo
- Jeisson Leonardo Barajas Valero
- John Alejandro Bonilla Cárdenas
- Karen Yulieth Toloza Castro
- Nelcy Alexandra Chaves Chaves

Instructor: Nelson Armando Serrano Hincapié

## Qué hace la plataforma
Estos son los requisitos funcionales que vamos a construir (vienen del documento de requisitos GA1-220501092-AA3-EV01 y de los prototipos GA1-220501092-AA4-EV01):

| ID | Requisito | Carpeta donde vive |
|---|---|---|
| RF-01 | Registro de usuaria | `src/features/auth` |
| RF-02 | Inicio de sesión | `src/features/auth` |
| RF-03 | Registro de datos del embarazo | `src/features/pregnancy` |
| RF-04 | Calendario de controles | `src/features/appointments` |
| RF-05 | Recordatorios de citas | `src/features/appointments` (pantalla) y `supabase/functions` (envío) |
| RF-06 | Información educativa | `src/features/education` |
| RF-07 | Gestión de citas (agendar, reprogramar, cancelar) | `src/features/appointments` |
| RF-08 | Histórico de citas | `src/features/appointments` |

## Tecnologías
- **Vite + React + TypeScript**: base de la aplicación.
- **Tailwind CSS + shadcn/ui**: estilos y componentes (botones, campos, tarjetas).
- **Supabase** (planeado): autenticación, base de datos y recordatorios.
- **Vitest y Playwright** (planeados): pruebas.

## Cómo ejecutarlo en tu computador
Necesitas **Node.js 20 o superior** y **Git**.

```bash
git clone <URL-del-repositorio>
cd materni-citas
npm install
npm run dev
```

Abre la dirección que aparece en la terminal (normalmente `http://localhost:5173`).

### Comandos disponibles
| Comando | Para qué sirve |
|---|---|
| `npm run dev` | Inicia la aplicación en modo desarrollo |
| `npm run build` | Genera la versión de producción |
| `npm run lint` | Revisa errores y estilo del código |
| `npm run preview` | Muestra la versión generada con `build` |

### Variables de entorno
Crea un archivo `.env` en la raíz del proyecto con estas variables:

​```
VITE_SUPABASE_URL=
VITE_SUPABASE_ANON_KEY=
​```

Pide los valores al equipo por un canal privado. **Nunca subas el archivo `.env` al repositorio.**

## Cómo está organizado el proyecto

```
materni-citas/
├── .github/            # Automatizaciones y plantillas de GitHub
├── docs/               # Documentación (requisitos, prototipos, trazabilidad)
├── supabase/
│   ├── migrations/     # Cambios de la base de datos
│   └── functions/      # Funciones del servidor (recordatorios)
├── tests/
│   └── e2e/            # Pruebas de flujos completos
├── public/             # Archivos estáticos (íconos, imágenes)
├── src/
│   ├── app/            # Configuración general: rutas, proveedores, layout principal
│   ├── features/       # Cada funcionalidad en su propia carpeta
│   │   ├── auth/           # Registro e inicio de sesión
│   │   ├── pregnancy/      # Datos del embarazo
│   │   ├── appointments/   # Calendario, citas, recordatorios, histórico
│   │   └── education/      # Información educativa
│   ├── components/
│   │   ├── ui/         # Componentes de shadcn/ui (no mover ni renombrar)
│   │   └── layout/     # Encabezado, pie de página, contenedores
│   ├── hooks/          # Hooks reutilizables en varias funcionalidades
│   ├── lib/            # Utilidades: cliente de Supabase, funciones auxiliares
│   ├── types/          # Tipos de TypeScript compartidos
│   ├── test/           # Configuración de las pruebas
│   ├── index.css       # Estilos globales y colores del proyecto
│   └── main.tsx        # Punto de entrada
├── AGENTS.md           # Instrucciones para las herramientas de IA
├── CONTRIBUTING.md     # Cómo trabajar en equipo (léelo antes de tu primer cambio)
└── README.md           # Este archivo
```

### Cómo se organiza cada funcionalidad
Todas las carpetas de `src/features/` siguen la misma plantilla:

```
features/auth/
├── components/    # Piezas de pantalla (por ejemplo RegisterForm)
├── pages/         # Pantallas completas (por ejemplo RegisterPage)
├── hooks/         # Lógica reutilizable (por ejemplo useRegister)
├── schemas.ts     # Validaciones de formularios
├── api.ts         # Comunicación con Supabase
├── types.ts       # Tipos de datos de esa funcionalidad
└── index.ts       # Lo único que otras partes pueden importar
```

Solo se crean los archivos que realmente se necesitan. Si una funcionalidad no tiene `api.ts` todavía, no se crea vacío.

### Reglas de orden
1. **Una funcionalidad no usa archivos internos de otra.** Si algo se necesita en varias, se mueve a `components/`, `hooks/` o `lib/`.
2. **Antes de crear un componente, busca si ya existe uno parecido.**
3. **No muevas `src/components/ui` ni `src/lib/utils.ts`.** shadcn/ui los necesita en ese lugar.
4. **Los nombres siguen un patrón:** componentes en `PascalCase.tsx`, hooks como `useAlgo.ts`, el resto en `camelCase.ts`.
5. **Los textos de la interfaz van en español; el código y los nombres de variables, en inglés.**

## Diseño
Los colores y las tipografías salen de los prototipos y están definidos en `src/index.css`. No pongas colores sueltos en los componentes: usa las variables del proyecto (`bg-primary`, `text-foreground`, etc.).

Principios: lenguaje claro y sencillo, textos grandes y legibles, buen contraste, y que funcione bien en el celular, porque es donde más lo van a usar las gestantes.

## Seguridad y datos personales
La plataforma maneja datos de salud. Se protege según la **Ley 1581 de 2012** (protección de datos personales en Colombia).
- No se escriben claves ni tokens en el código.
- No se imprimen datos personales en la consola.
- Cada usuaria solo debe poder ver sus propios datos.
- Para pruebas y capturas se usan datos ficticios, nunca reales.

## Quiero colaborar
Lee [CONTRIBUTING.md](./CONTRIBUTING.md): explica cómo crear una rama, cómo escribir los commits y cómo pedir que revisen tu trabajo.

## Documentos del proyecto
- Documento de requisitos: GA1-220501092-AA3-EV01
- Prototipos de interfaz y casos de prueba: GA1-220501092-AA4-EV01