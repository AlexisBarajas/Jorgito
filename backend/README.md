# Backend Core 

Este módulo representa el motor analítico de socialPoint. Construido sobre **FastAPI**, está diseñado para manejar flujos de datos asíncronos (I/O Bound) de manera altamente concurrente, integrando extracción de datos (Scraping) y Procesamiento de Lenguaje Natural (NLP).

##  Arquitectura Interna y Módulos

El backend sigue un patrón de diseño basado en **Controlador-Servicio-Repositorio**, desacoplando la lógica de negocio de las rutas de la API.

*   **`app/auth/` (Seguridad y JWT):** Maneja el ciclo de vida de los tokens de acceso. Expone funciones para crear, firmar y decodificar JWTs.
*   **`app/core/` (Configuración):** Almacena el objeto `Settings` (usando `pydantic-settings`). Carga variables de entorno (CORS, secret keys, credenciales de DB) y las hace accesibles globalmente.
*   **`app/database/` (Capa de Persistencia):** Implementa el cliente asíncrono de Motor (MongoDB). Mantiene el pool de conexiones mediante eventos de `startup` y `shutdown` de FastAPI.
*   **`app/models/` (Data Transfer Objects & Schemas):** El corazón de la validación. Define esquemas **Pydantic** para Requests/Responses y esquemas ODMs para los documentos de MongoDB.
*   **`app/nlp/` (Motor Cognitivo):** Envuelve los modelos de análisis (VADER/HuggingFace). Recibe strings crudos y devuelve diccionarios con scores de polaridad y etiquetas semánticas.
*   **`app/routes/` (Endpoints API):** Los controladores. Reciben la petición HTTP, delegan la lógica a los servicios (Scraping/NLP) y retornan la respuesta formateada.
*   **`app/scraping/` (Ingesta de Datos):** Scripts asíncronos de Playwright. Manejan el contexto del navegador, evaden bloqueos y extraen el DOM.

##  Flujo de Datos y Paso de Parámetros

La comunicación entre módulos está estrictamente tipada y se gestiona mediante **Inyección de Dependencias** y **Modelos Pydantic**.

1.  **De HTTP a Route (`app/routes/`):**
    *   El cliente envía un payload JSON. FastAPI utiliza un esquema de `app/models/` para validar el tipado automáticamente.
    *   *Ejemplo de Parámetro:* `payload: AnalysisRequest` o query parameters `event_id: str`.
2.  **De Route a Dependencias (`app/database/` y `app/auth/`):**
    *   Se inyecta la sesión de la base de datos y la validación del usuario directamente en la firma de la función de la ruta usando `Depends()`.
    *   *Ejemplo:* `db: AsyncIOMotorClient = Depends(get_database)`, `current_user = Depends(verify_jwt)`.
3.  **De Route a Lógica de Negocio (`app/scraping/` y `app/nlp/`):**
    *   Las rutas invocan las funciones de scraping pasando parámetros primitivos validados (ej. `target_url`, `keywords`).
    *   El módulo de scraping extrae una lista de textos y se la inyecta al módulo NLP: `nlp.analyze_batch(texts: List[str]) -> List[SentimentResult]`.
4.  **De Lógica a Base de Datos:**
    *   El resultado estructurado se mapea a un modelo de Base de Datos y se inserta pasando el diccionario al driver de MongoDB.