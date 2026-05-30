# To-Do App - S-SDLC (Fase Inicial / Línea Base)

## 1. Definición de la aplicación y cómo ejecutarla
Esta es la aplicación de gestión de tareas ("To-Do" list) oficial del tutorial de Docker. Se utiliza como punto de partida para implementar progresivamente un Ciclo de Vida de Desarrollo Seguro (S-SDLC).

**Cómo ejecutarla localmente:**
1. Navega a la carpeta de la aplicación: `cd app`
2. Construye la imagen base: `docker build -t todo-app-baseline .`
3. Ejecuta el contenedor: `docker run -d -p 3000:3000 --name mi-todo-app todo-app-baseline`
4. Accede en tu navegador a: `http://localhost:3000`

---

## 2. Consideraciones de seguridad en el diseño (Threat Modeling Inicial)
En esta primera iteración, se ha analizado la arquitectura base identificando intencionadamente las siguientes brechas de seguridad (deuda técnica), las cuales servirán como hoja de ruta para futuras correcciones:

* **Ejecución con privilegios (Root):** Actualmente, el `Dockerfile` no define un usuario sin privilegios, por lo que el proceso de Node.js se ejecuta como `root` dentro del contenedor.
* **Superficie de ataque amplia:** Se está utilizando la imagen base `node:18` (basada en Debian), la cual contiene múltiples librerías y utilidades innecesarias para la ejecución de la app, aumentando los vectores de ataque.
* **Dependencias vulnerables:** No se ha realizado un filtrado de dependencias de desarrollo (`yarn install` por defecto), lo que incrementa el riesgo de ataques a la cadena de suministro.

---

## 3. Creación de la aplicación siguiendo el S-SDLC (Iteración 1)

Al estar en la primera iteración del S-SDLC, nos encontramos estableciendo la línea base del proyecto:

* **Fase de Requisitos:** Se establece la necesidad funcional de la aplicación (gestor de tareas) y se define el requisito no funcional de auditar su estado actual.
* **Fase de Diseño:** Se ha creado una estructura de repositorio que separa limpiamente el código fuente (`/app`), la documentación de arquitectura/seguridad (`/docs`) y las herramientas de validación (`/tests`).
* **Fase de Desarrollo:** Se ha integrado el código base funcional sin aplicar refactorizaciones de seguridad todavía.
* **Fase de Pruebas y Verificación:** Se ha diseñado un entorno de pruebas inicial en la carpeta `/tests`. Mediante contenedores efímeros, se ejecuta un análisis SCA (`yarn audit`) para generar un reporte de `vulnerabilidades_iniciales.txt` que sirva como punto de partida.
* **Fase de Despliegue:** Despliegue local básico sin restricciones de recursos (CPU/Memoria) ni hardening del demonio de Docker.