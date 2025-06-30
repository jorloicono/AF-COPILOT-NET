# Analizar y documentar código con GitHub Copilot

GitHub Copilot puede ayudarte a comprender y documentar una base de código generando explicaciones y documentación. En este ejercicio, usarás GitHub Copilot para analizar una base de código y generar documentación para el proyecto.

## IMPORTANTE

Para completar este ejercicio, debes contar con una cuenta de GitHub y una suscripción activa a GitHub Copilot.

* Puedes registrarte para una cuenta gratuita individual de GitHub y utilizar el plan gratuito de GitHub Copilot.
* Si ya tienes acceso a Copilot Pro, Copilot Pro+, Copilot Business o Copilot Enterprise, puedes usar esa suscripción.

---

## Requisitos previos

Tu entorno de laboratorio debe tener lo siguiente:

* Git 2.48 o superior
* .NET SDK 9.0 o superior
* Visual Studio Code con la extensión **C# Dev Kit**
* Acceso a una cuenta de GitHub con Copilot habilitado

### Si usas una PC local como entorno de laboratorio:

* Sigue esta guía: [Configurar recursos del entorno de laboratorio](#)
* Para habilitar GitHub Copilot en VS Code: [Habilitar GitHub Copilot en Visual Studio Code](#)

### Si usas un entorno hospedado:

* Habilita Copilot con esta URL: [Habilitar GitHub Copilot en Visual Studio Code](#)

---

## Escenario del ejercicio

Eres un desarrollador del departamento de TI de tu comunidad local. El sistema backend de la biblioteca pública se perdió en un incendio. Tu equipo debe desarrollar una solución temporal. Un compañero ya creó una versión inicial, pero no tuvo tiempo de documentarla.

**Tu tarea:** Analizar la base de código y crear la documentación del proyecto.

---

## Tareas del ejercicio

### 1. Configurar la aplicación en Visual Studio Code

1. Descarga el archivo ZIP: `AZ2007LabAppM2.zip` desde [GitHub Copilot lab - Analyze and document code](#)
2. Extrae los archivos.
3. Copia la carpeta `AccelerateDevGHCopilot` al escritorio.
4. Abre la carpeta en VS Code:

   * Menú: `Archivo > Abrir Carpeta > AccelerateDevGHCopilot`
5. Asegúrate de que la solución se vea así:

```
AccelerateDevGHCopilot
├── src
│   ├── Library.ApplicationCore
│   ├── Library.Console
│   └── Library.Infrastructure
└── tests
    └── UnitTests
```

6. Compila la solución (clic derecho en `AccelerateDevGHCopilot > Compilar`). Ignora advertencias, pero no debe haber errores.

---

### 2. Usar GitHub Copilot para explicar la base de código

#### a. Analizar usando la vista de chat

1. Abre la vista de Chat de Copilot (`Ctrl + Alt + I` o botón en la parte superior).

2. Escribe el siguiente prompt para obtener una descripción del proyecto:

   ```bash
   @workspace describe this project
   ```

3. Compara la respuesta con los archivos reales del proyecto.

4. Abre `ConsoleApp.cs` (`src/Library.Console/`) y pide una explicación:

   ```bash
   @workspace #usages How is the ConsoleApp class used?
   ```

5. Abre `Program.cs` y escribe:

   ```bash
   @workspace /explain Explain the Program.cs file
   ```

#### b. Mejorar respuestas con contexto adicional

1. Abre los archivos:

   * `JsonData.cs`
   * `JsonLoanRepository.cs`
   * `JsonPatronRepository.cs`

   Arrástralos a la ventana de Chat para añadir contexto.

2. Escribe el siguiente prompt:

   ```bash
   @workspace /explain Explain how the data access classes work
   ```

3. Revisa los archivos JSON (`src/Library.Console/Json`) y observa cómo los IDs enlazan entidades como préstamos, libros y usuarios.

---

### 3. Compilar y ejecutar la aplicación

1. En el **Explorador de soluciones**, haz clic derecho en `Library.Console > Depurar > Iniciar nueva instancia`.

#### Ejemplo de uso:

* Introduce: `One`
* Selecciona: `2` (elige segundo usuario)
* Verifica libros y estado de préstamo.
* Introduce: `1` (elige primer libro)
* Introduce: `r` (devolver libro)
* Confirma mensaje: “**Book was successfully returned.**”
* Introduce: `s` (nueva búsqueda)
* Repite pasos con mismo usuario.
* Verifica que el libro esté marcado como `Returned: True`.
* Introduce: `q` (salir)

---

### 4. Crear archivo README.md

1. Agrega un archivo `README.md` en la raíz del proyecto.

2. Abre la vista de Chat.

3. Introduce el siguiente prompt:

   ```bash
   @workspace Generate the contents of a README.md file for a code repository. Use "Library App" as the project title. The README file should include the following sections: Description, Project Structure, Key Classes and Interfaces, Usage, License. Format all sections as raw markdown. Use a bullet list with indents to represent the project structure. Do not include ".gitignore" or the ".github", "bin", and "obj" folders.
   ```

4. Copia la respuesta generada y pégala en `README.md`.

5. Ajusta formato y contenido si es necesario.

---

## Resumen

En este ejercicio aprendiste a usar GitHub Copilot para:

* Analizar la estructura del proyecto.
* Explicar clases clave y componentes de acceso a datos.
* Crear un archivo `README.md` con la documentación esencial.

---

## Limpieza

Si hiciste cambios en tu cuenta de GitHub o en la suscripción a GitHub Copilot que no deseas conservar, revierte los cambios ahora.


