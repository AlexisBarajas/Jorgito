# socialPoint 📍
> **"Escuchando el pulso de tus eventos en tiempo real."**

socialPoint es una plataforma modular de análisis de datos diseñada para capturar, procesar y visualizar el sentimiento de las audiencias en eventos masivos (presenciales o digitales). Mediante técnicas de **Web Scraping** y **NLP**, transformamos el ruido de las redes sociales y foros en insights accionables para organizadores, empresas y gobiernos.

## Stack Tecnológico
*   **Frontend:** React + Vite, Tailwind CSS, Recharts.
*   **Backend API:** Python (FastAPI).
*   **Base de Datos:** MongoDB (NoSQL).
*   **Scraping:** Python (Playwright / BeautifulSoup).
*   **NLP:** Python (VADER / TextBlob / Hugging Face).

## Estructura del Proyecto
*   [`/frontend`](./frontend/README.md): Interfaz de usuario y dashboards interactivos.
*   [`/backend-api`](./backend-api/README.md): Orquestador central y API REST.
*   [`/scraping-service`](./scraping-service/README.md): Microservicio de extracción de datos.
*   [`/nlp-service`](./nlp-service/README.md): Procesamiento de lenguaje natural y análisis de sentimiento.

## Pipeline de Datos
1.  **Captura:** El usuario ingresa un término o URL en la web.
2.  **Extracción:** El scraper recolecta comentarios de fuentes externas.
3.  **Refinado:** El módulo NLP limpia el texto y asigna un puntaje emocional.
4.  **Almacenamiento:** Los resultados se indexan en MongoDB.
5.  **Visualización:** El dashboard consume la API y muestra tendencias y alertas.
