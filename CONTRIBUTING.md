# Guía para trabajar en equipo

Este documento explica **cómo hacemos cambios en MaterniCitas** para no pisarnos el trabajo entre compañeros. Léelo antes de tu primer cambio. Son pocas reglas y se aprenden rápido.

## La idea general
1. La rama `main` siempre debe funcionar. **Nadie sube cambios directo a `main`.**
2. Cada tarea se hace en **su propia rama**.
3. Cuando terminas, abres un **Pull Request (PR)**: una solicitud para que un compañero revise tu trabajo.
4. Cuando alguien lo aprueba, tu rama se une a `main`.

## Paso a paso para hacer un cambio

### 1. Trae lo último de `main`
```bash
git checkout main
git pull
```

### 2. Crea tu rama
Nombre: `tipo/descripcion-corta`.

```bash
git checkout -b feat/rf-01-registro
```

Tipos de rama:
| Tipo | Cuándo usarlo | Ejemplo |
|---|---|---|
| `feat` | Una funcionalidad nueva | `feat/rf-07-reprogramar-cita` |
| `fix` | Arreglar un error | `fix/login-contador-intentos` |
| `docs` | Cambios solo de documentación | `docs/actualizar-readme` |
| `style` | Cambios visuales sin cambiar la lógica | `style/ajustar-colores-boton` |
| `test` | Agregar o corregir pruebas | `test/rf-02-bloqueo` |

### 3. Trabaja y guarda tus cambios (commits)
```bash
git add .
git commit -m "feat(auth): agrega formulario de registro"
```

Formato del mensaje: `tipo(parte): qué hiciste`.
- Usa los mismos tipos de arriba (`feat`, `fix`, `docs`...).
- La parte entre paréntesis es la carpeta o funcionalidad: `auth`, `pregnancy`, `appointments`, `education`, `ui`.
- Escribe en español, en presente y de forma corta.

Ejemplos buenos:
- `feat(appointments): agrega botón para cancelar cita`
- `fix(auth): corrige validación del correo`
- `docs: explica cómo ejecutar el proyecto`

Ejemplos que debes evitar: `cambios`, `arreglos`, `listo`, `asdf`.

Haz commits pequeños y frecuentes. Es mejor varios commits claros que uno gigante.

### 4. Antes de subir, revisa que todo funcione
```bash
npm run lint
npm run build
```
Si alguno da error, corrígelo antes de continuar.

### 5. Sube tu rama
```bash
git push -u origin feat/rf-01-registro
```

### 6. Abre el Pull Request en GitHub
1. Entra al repositorio en GitHub. Verás un botón para crear el PR de tu rama.
2. Pon un título claro (igual que el commit principal).
3. Explica en pocas líneas qué hiciste y cómo se prueba.
4. Pide la revisión de un compañero.

### 7. Revisión
- Quien revisa mira que el cambio funcione, que respete las reglas de carpetas y que sea fácil de entender.
- Si pide ajustes, haz los cambios en **la misma rama**, haz commit y sube de nuevo: el PR se actualiza solo.
- Cuando lo aprueban, se une a `main` y se puede borrar la rama.

## Cómo repartirnos el trabajo
- Cada requisito (RF-01 a RF-08) se asigna a una persona mediante una **Issue** en GitHub.
- Antes de empezar, avisa en el grupo en qué estás trabajando.
- Si dos personas necesitan tocar el mismo archivo, hablen antes. Eso evita la mayoría de los conflictos.

## Reglas del código
- **Cada funcionalidad vive en su carpeta** dentro de `src/features/`. Revisa el [README](./README.md) para ver cómo se organiza.
- No uses archivos internos de otra funcionalidad. Si algo se comparte, va en `src/components`, `src/hooks` o `src/lib`.
- No muevas `src/components/ui` ni `src/lib/utils.ts`.
- Usa los colores del proyecto (`bg-primary`, `text-foreground`...), no colores sueltos.
- Textos de la interfaz en español claro y sencillo.
- Cada pantalla debe tener estado de carga, estado vacío y mensaje de error.
- Formularios con etiquetas visibles y que se puedan usar con teclado.

## Lo que nunca se sube a Git
- Archivos `.env` ni claves, contraseñas o tokens.
- La carpeta `node_modules`.
- Datos reales de usuarias ni capturas con información personal.
- Texto médico inventado: el contenido educativo debe citar una fuente oficial (Minsalud u OMS).

Si por error subes una clave, **avísale al equipo de inmediato**: hay que cambiarla, y borrarla del último commit no es suficiente.

## ¿Qué hago si hay un conflicto?
Un conflicto aparece cuando dos personas cambiaron la misma parte de un archivo. No es un error grave:
1. No fuerces nada (`--force`).
2. Pide ayuda en el grupo.
3. Entre las dos personas deciden qué versión se queda.

## Dudas
Escribe en el grupo del equipo. Mejor preguntar antes que dejar un cambio a medias.