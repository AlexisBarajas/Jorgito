# socialPoint - Frontend UI 

La capa de presentación de socialPoint. Una Single Page Application (SPA) reactiva construida con **React + Vite**, diseñada para renderizar métricas de alto rendimiento y gráficas en tiempo real sin saturar el hilo principal del navegador.

##  Arquitectura Interna y Componentes

Se utiliza una estructura basada en características (Feature-based) orientada a la reutilización y el aislamiento de estados.

*   **`src/assets/` (Recursos y Estilos):** Contiene la configuración de Tailwind CSS, variables CSS globales, iconografía SVG y tipografías.
*   **`src/components/` (UI Atómica):** Componentes puros (Dumb components). Reciben datos y funciones únicamente a través de *props*. Aquí viven las tarjetas, botones, inputs y las envolventes de las gráficas de Recharts.
*   **`src/hooks/` (Lógica de Estado - Custom Hooks):** Extrae la lógica compleja de las vistas. Incluye hooks como `useSentimentData()` para manejar el fetching y estado (loading, error, data) o `useAuth()` para el contexto de sesión.
*   **`src/services/` (Capa de Red):** Wrappers sobre **Axios**. Define clientes HTTP preconfigurados con interceptores para inyectar automáticamente el token JWT en cada petición e interceptar errores 401 para re-autenticación.
*   **`src/views/` (Páginas y Layouts):** Componentes inteligentes (Smart components). Corresponden a las rutas del enrutador (ej. `/dashboard`, `/login`). Orquestan los hooks y pasan la data a los componentes atómicos.

##  Flujo de Datos y Paso de Parámetros

El frontend mantiene un flujo de datos **unidireccional** (Top-Down), característico de React, complementado con contextos globales para datos de sesión.

1.  **Estado Global (Session / Auth):**
    *   Se utiliza **React Context API** (o Zustand) para almacenar el JWT y el perfil del usuario.
    *   Cualquier vista que requiera saber quién está logueado consume el contexto usando el hook custom `useAuth()`.
2.  **Llamadas a la API (`views/` -> `hooks/` -> `services/`):**
    *   Una vista dispara un hook, por ejemplo: `const { data, loading } = useEventAnalysis(eventId);`.
    *   El hook invoca al servicio: `api.get(\`/events/\${eventId}/metrics\`)`.
    *   Los parámetros de URL (como `eventId`) se capturan mediante el enrutador (`react-router-dom` con `useParams()`) y se inyectan hacia abajo.
3.  **Inyección hacia Componentes UI (`views/` -> `components/`):**
    *   La vista recibe la data estructurada del backend y se la pasa a los componentes de visualización mediante **Props**.
    *   *Ejemplo:* `<SentimentChart data={data.timeSeries} colorScheme="dark" />`.
    *   Las interacciones del usuario (clicks, filtros) se pasan hacia arriba mediante funciones de callback inyectadas también como props: `<FilterBar onFilterChange={(filter) => updateTarget(filter)} />`.
4.  **Actualización Reactiva (Polling / WebSockets):**
    *   Para la "escucha en tiempo real", los hooks implementan `useEffect` que gestionan intervalos de *polling* o escuchan eventos de WebSocket, forzando un re-render de las gráficas de Recharts únicamente cuando las referencias de memoria de la prop `data` cambian.