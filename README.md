Perfecto, aquí tienes el archivo **README.md** actualizado, integrando la estructura de directorios (árbol de carpetas) dentro del flujo de la documentación técnica para que se entienda la jerarquía del proyecto.

---

# socialPoint 📍

> **"Escuchando el pulso de la audiencia en tiempo real."**

**socialPoint** es un ecosistema modular diseñado para la ingesta, procesamiento y visualización de datos de sentimiento en eventos masivos. La plataforma transforma el ruido social en métricas accionables mediante un pipeline de Inteligencia Artificial y Web Scraping reactivo.

---

## 📂 Estructura del Proyecto

La organización del código sigue un patrón de separación de responsabilidades para facilitar el mantenimiento y la escalabilidad del sistema:

```text
socialPoint/
├── backend/                # FastAPI Server + Inteligencia Artificial
│   ├── app/
│   │   ├── auth/           # Gestión de JWT, hashing y seguridad de usuarios
│   │   ├── core/           # Configuración global, variables de entorno y constantes
│   │   ├── database/       # Driver de conexión y lógica de persistencia en MongoDB
│   │   ├── models/         # Esquemas de Pydantic y definiciones de colecciones NoSQL
│   │   ├── nlp/            # Motores de análisis de sentimiento (VADER/HuggingFace)
│   │   ├── routes/         # Definición de Endpoints RESTful
│   │   └── scraping/       # Motores de extracción asíncrona con Playwright
│   ├── requirements.txt    # Dependencias del entorno Python
│   └── main.py             # Punto de entrada y orquestación del servidor
├── frontend/               # React + Vite UI (Dashboard)
│   ├── src/
│   │   ├── assets/         # Recursos estáticos, estilos globales y media
│   │   ├── components/     # UI Dinámica, gráficas de Recharts y elementos atómicos
│   │   ├── hooks/          # Lógica de estado y efectos reutilizables
│   │   ├── services/       # Cliente de API centralizado (Axios)
│   │   └── views/          # Vistas principales (Login, Análisis de Sentimiento, Dash)
│   ├── index.html          # Punto de montaje del DOM
│   └── package.json        # Manifiesto de dependencias de Node.js
└── README.md               # Documentación técnica

```

---

## 🏗️ Arquitectura y Stack Tecnológico

### Backend: Procesamiento de Alta Disponibilidad

El núcleo está construido sobre **FastAPI (Python 3.10+)**, aprovechando la programación asíncrona para gestionar tareas intensivas de I/O.

* **Pipeline de NLP:** Implementación híbrida. Utiliza **VADER** para velocidad en textos cortos y **HuggingFace Transformers** para análisis semántico profundo en contextos complejos.
* **Web Scraping:** Motor basado en **Playwright** capaz de renderizar contenido dinámico y superar las barreras de las Single Page Applications (SPAs).
* **Persistencia:** **MongoDB** gestiona el almacenamiento de datos no estructurados, permitiendo una indexación rápida de grandes volúmenes de comentarios.

### Frontend: Visualización Reactiva

Interfaz desarrollada con **React (Vite)** enfocada en la experiencia de usuario y la representación de datos en tiempo real.

* **Estilizado:** **Tailwind CSS** para un diseño moderno, responsivo y de bajo peso.
* **Gráficas:** **Recharts** para la visualización de series temporales de polaridad (positiva, negativa, neutra).

---

## 🔄 Pipeline de Datos

1. **Ingesta:** El sistema activa procesos de scraping asíncronos basados en parámetros de búsqueda (hashtags, eventos o keywords).
2. **Tratamiento:** Se realiza una limpieza de datos (normalización de texto) y se somete al motor de **Procesamiento de Lenguaje Natural (NLP)**.
3. **Almacenamiento:** Los resultados se categorizan y guardan en documentos JSON dentro de la base de datos NoSQL.
4. **Entrega:** La API expone los datos procesados que son consumidos por el Dashboard para mostrar el "estado de ánimo" de la audiencia en vivo.

---

## 🛠️ Aspectos Técnicos Relevantes

* **Validación de Datos:** Uso estricto de **Pydantic** en el backend para asegurar que la comunicación entre el cliente y el servidor sea íntegra y tipada.
* **Seguridad:** Implementación de **OAuth2 con Password Flow** y tokens **JWT** para el control de acceso.
* **Escalabilidad:** El diseño modular permite desacoplar los motores de scraping para ejecutarlos como microservicios independientes si la carga de datos lo requiere.

---

> **Estatus Técnico:** El proyecto prioriza la eficiencia en el procesamiento de texto y la reactividad de la interfaz, asegurando que la latencia entre la captura del dato y su visualización sea mínima.