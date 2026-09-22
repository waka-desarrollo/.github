# 🌊 waka studio — github

El código del estudio. Organizado, versionado, deployado.

---

## Acceso

| Rol    | Quién                    | Puede                                 |
| ------ | ------------------------ | ------------------------------------- |
| Owner  | Alejandro, Alicia, Vito, Mariusz       | Todo — gestionar org, repos, miembros |
| Member | otros devs | Push/pull en repos asignados          |

¿Necesitas acceso a un repo? Habla con Mariusz.

---

## Convenciones

### Repos

```
✅ studio-mai
✅ waka-base-theme
✅ quattrocento

❌ StudioMai
❌ studio_mai
❌ Studio Mai
```

Siempre minúsculas, siempre guiones, nunca espacios.

## La regla de oro

main - producción (lo que ve el cliente)
dev-\* - desarrollo (lo que ves tú)

Nunca subas código sin probar directamente a main.
Siempre pasa por staging primero.

### Ramas

```
main          - producción, siempre estable
dev-nombre    - tu rama de desarrollo personal
               dev-mariusz / dev-vito / dev-alicia
feature/x     - nueva funcionalidad específica
               feature/hero-animation
fix/x         - corrección de bug
               fix/mobile-menu
chore/x       - mantenimiento
               chore/update-dependencies
```

**Nunca trabajes directamente en `main`.**

### Commits — Conventional Commits

```
feat:     nueva funcionalidad
fix:      corrección de bug
style:    cambios visuales sin cambios de lógica
refactor: reestructuración sin cambios visuales
chore:    mantenimiento, dependencias
docs:     solo documentación
```

Ejemplos:

```
feat: añadir filtro de proyectos por servicio
fix: corregir scroll horizontal en Safari
chore: actualizar dependencias npm
```

---

## Mover proyectos existentes

### Paso 1 — Transferir el repo a la organización

```
GitHub - tu repo personal
- Settings - scroll hasta abajo - Danger Zone
- Transfer repository
- Introduce el nombre del repo para confirmar
- Elige: somoswaka
- Confirmar transferencia
```

El repo ahora vive en `github.com/somoswaka/proyecto`.
Los links antiguos redirigen automáticamente.

### Paso 2 — Actualizar la URL remota en tu ordenador

Después de transferir, actualiza el remote en tu máquina local.

**Con terminal (Git Bash):**

```bash
cd /c/MAMP/htdocs/proyecto/wp-content/themes/proyecto
git remote set-url origin git@github.com:somoswaka/proyecto.git
git remote -v  # verifica que el cambio es correcto
```

**Con SourceTree:**

```
Settings - Remotes - origin - Edit
- cambia la URL a:
  git@github.com:somoswaka/proyecto.git
- OK
```

### Paso 3 — Verificar

```bash
git fetch origin
git status
# Debería decir: Your branch is up to date with 'origin/main'
```

---

## Trabajar en equipo

Cuando más de una persona trabaja en el mismo proyecto
— como la nueva web de waka — cada developer trabaja
en su propia rama y nunca directamente en `main`.

### El flujo

```
Mariusz trabaja en - dev-mariusz
Vito trabaja en    - dev-vito
Alicia trabaja en  - dev-alicia

Cada push a dev-* - deploy automático a staging
Todos ven los cambios de todos en staging

Cuando algo está listo y aprobado:
- merge a main - deploy automático a producción
```

### Cómo sincronizarse con el trabajo de los demás

Si Vito ha subido cambios y los quieres en tu rama:

**Terminal:**

```bash
git fetch origin
git merge origin/dev-vito
# o
git rebase origin/dev-vito
```

**SourceTree:**

```
Fetch - selecciona origin/dev-vito - Merge
```

### Si hay un conflicto

Un conflicto pasa cuando dos personas han editado
la misma línea del mismo archivo. No es un error —
es Git diciendo "necesito que decidas tú".

```
1. Git marca los conflictos en el archivo así:
   <<<<<<< HEAD
   tu versión
   =======
   versión de la otra persona
   >>>>>>> dev-vito

2. Editas el archivo y dejas solo lo correcto
3. git add archivo-con-conflicto
4. git commit -m "merge: resolver conflicto en header.php"
```

Ante cualquier duda — habla con la otra persona antes de resolver.

---

## GitHub Secrets y deploy automático

Los deploys a staging y producción son automáticos —
cada push a la rama correcta dispara el deploy solo.

Las credenciales FTP (host, usuario, contraseña, path)
**nunca están en el código**. Se guardan encriptadas
en GitHub Secrets:

```
GitHub - tu repo - Settings - Secrets and variables - Actions
```

Para configurar el deploy automático en un proyecto
usamos los scripts de waka-scripts — no hace falta
tocar nada manualmente:

```bash
# Deploy a staging (una vez por proyecto)
bash /c/waka-scripts/setup-deploy-dev.sh

# Deploy a producción (una vez por proyecto)
bash /c/waka-scripts/setup-deploy-prod.sh
```

Los scripts añaden los secrets automáticamente y crean
el workflow de GitHub Actions. Después solo necesitas
hacer `git push` y el resto ocurre solo.

- Para más detalle: [DEPLOY_GUIDE.md](../waka-scripts/DEPLOY_GUIDE.md)

---

## Buenas prácticas

```
✅ Haz commits pequeños y frecuentes
   Un commit = un cambio con sentido

✅ Escribe mensajes de commit que expliquen el qué
   feat: añadir sección de testimonios
   no: "cambios" / "arreglos" / "wip"

✅ Haz push al final de cada día de trabajo
   Tu código en local solo existe en tu ordenador

✅ Comprueba siempre en qué rama estás antes de empezar
   git branch (terminal) / barra inferior en SourceTree

✅ Nunca pongas credenciales en el código
   Usa .env para local, GitHub Secrets para deploy

❌ Nunca hagas push directamente a main
❌ Nunca commitees node_modules/
❌ Nunca commitees archivos .env
```

---

## .gitignore — lo que nunca debe subir a GitHub

Asegúrate de que tu `.gitignore` incluye siempre:

```
node_modules/
.env
.DS_Store
Thumbs.db
*.log
```

El tema base ya incluye un `.gitignore` correcto.

---

## Recursos

| Qué                             | Dónde                                                                                                   |
| ------------------------------- | ------------------------------------------------------------------------------------------------------- |
| Montar un proyecto nuevo        | [waka-scripts/README.md](https://github.com/waka-desarrollo/wa-scripts/blob/main/README.md)             |
| Configurar deploy automático    | [waka-scripts/DEPLOY_GUIDE.md](https://github.com/waka-desarrollo/wa-scripts/blob/main/DEPLOY_GUIDE.md) |
| Tema base                       | [waka-base-theme](../waka-base-theme)                                                                   |
| Módulos WordPress reutilizables | [wa-wp-utils](https://github.com/waka-desarrollo/wa-wp-utils)                                           |

---

_¿Algo no está aquí o algo ha cambiado? Actualiza este documento._
_Es de todos — no solo de quien lo escribió._

