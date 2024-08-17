# Welcome to My Code Galaxy 🌌

Hello, fellow Earthling or resident of the cosmos! Welcome to my personal repository, where I store the artefacts of my coding adventures across various programming landscapes. Whether you're here by accident or on a quest for knowledge, grab a coffee ☕, and enjoy exploring my creations in Python, Shell, HTML, CSS, JavaScript, Go, and Rust.

## What's in the Box? 📦

Here's a sneak peek of what you'll find in this galaxy:

- 🐍 **Python**: Sssssome spectacular scripts that automate the boring stuff and tackle data like a boss.
- 🐚 **Shell**: Commands that summon the power of the terminal, unleashing scripts that orchestrate my Linux universe.
- 🌐 **HTML & CSS**: The skeleton and skin of my web pages—crafted with care to be both eye-catching and functional.
- 🖥️ **JavaScript**: Dynamic functionalities that make web pages come to life. Get ready for interactions that are out of this world!
- 🚀 **Go**: Fast, compiled code that runs like a cheetah. It's used for my backend systems that need speed and efficiency.
- 🦀 **Rust**: Safe, concurrent, and practical code pieces that aim to prevent galactic anomalies.

## Star Features ⭐

- **Interactivity**: Not just static code! Examples included have interactive elements to demonstrate real-world applications.
- **Cross-Language Projects**: Some projects utilize multiple languages, showing off how they can interact harmoniously.
- **Detailed Comments**: Wherever you look, you'll find detailed comments explaining the magic behind the code.

## Poject BPM  🛠️

```mermaid
flowchart TD;
    A[Inicio] --> B[Leer configuración desde config.json];
    B --> C[Establecer conexión a la base de datos SQL];
    C -->|Conexión exitosa| D[Ejecutar consulta SQL y cargar datos en DataFrame];
    C -->|Conexión fallida| E[Mostrar error y cerrar conexión];
    D --> F[Procesar datos];
    F --> G[Filtrar datos por fecha y eliminar filas no deseadas];
    G --> H[Guardar datos procesados en un archivo Excel];
    H --> I[Llamar a la función send_email con la ruta del archivo];
    I --> J[Crear contenedor de mensaje];
    J --> K[Adjuntar archivo Excel al correo];
    K --> L[Conectar al servidor SMTP];
    L -->|Conexión exitosa| M[Enviar correo electrónico];
    L -->|Conexión fallida| N[Mostrar error de envío];
    M --> O[Mostrar mensaje de éxito];
    O --> P[Fin];
