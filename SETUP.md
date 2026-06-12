## Configuración del MCP de Notion

Sigue estos pasos antes de ejecutar el skill por primera vez.

### 1 — Requisitos previos
- Node.js 18+ instalado — https://nodejs.org/
- Claude Code instalado en el cmd del pc:
```  npm install -g @anthropic-ai/claude-code
```
- MCP oficial de Notion instalado en el cmd del pc:
```  npm install -g @notionhq/notion-mcp-server
```

### 2 — Crear cuenta en Notion
Ve a https://www.notion.so/ y crea una cuenta gratuita.

### 3 — Crear la integración de Notion
1. Ve a https://www.notion.so/profile/integrations
2. Click **"New connection"**
3. Nombre: `betterway-ats-skill`
4. Método de autenticación: **Token de acceso**
5. Click **Save**
6. Copia el token que empieza con `ntn_...`

### 4 — Crear la página destino en Notion
1. En Notion crea una página nueva llamada `Agent ATS Betterway`
2. Dentro de esa página click en los 3 puntos`...` arriba a la derecha
3. Click **"Connections"**
4. Selecciona `betterway-ats-skill`
5. Copia el Page ID del URL — son los últimos 32 caracteres:

### 5 — Conectar el MCP de Notion a Claude Code
1. Abre el **Command Prompt** de Windows:
   - Presiona `Windows + R`
   - Escribe `cmd`
   - Presiona Enter
2. Ejecuta este comando reemplazando `ntn_tu-token-aqui` con tu token real: (debes ejecutarlo sin las comillas)
```claude mcp add notion npx @notionhq/notion-mcp-server --env OPENAPI_MCP_HEADERS="{\"Authorization\": \"Bearer ntn_tu-token-aqui\", \"Notion-Version\": \"2022-06-28\"}"
```
### 6 — Verificar la conexión
En el mismo Command Prompt ejecuta:
```claude mcp list
```
Deberías ver: notion: npx @notionhq/notion-mcp-server - √ Connected

### 7 — Preparar los inputs
1. Clona el repositorio en tu máquina:
```git clone https://github.com/Julianvgr09/Agent_betterway_ats.git
```
2. Abre la carpeta clonada en el explorador de Windows
3. Copia tus PDFs de candidatos dentro de `inputs/candidates/`
4. Copia tus PDFs de descripciones de puesto dentro de `inputs/jobs/`

Las carpetas ya están creadas — solo debes poner los archivos dentro.


### 8 — Ejecutar el skill

1. Abre el Command Prompt de Windows:
   - Presiona las teclas `Windows + R` al mismo tiempo
   - Se abre una ventana pequeña llamada "Ejecutar"
   - Escribe `cmd` y presiona Enter
   - Se abre una ventana negra — eso es el Command Prompt

2. Navega hasta la carpeta del proyecto:
   - En el Command Prompt escribe `cd ` (con espacio al final)
   - Abre el explorador de Windows y busca la carpeta `Agent_betterway_ats`
   - Copia la ruta completa de la carpeta (aparece en la barra superior
     del explorador, ejemplo: `C:\Users\pablo\Desktop\Agent_betterway_ats`)
   - Pégala después del `cd ` y presiona Enter
   - Ejemplo:
```     cd C:\Users\pablo\Desktop\Agent_betterway_ats
```
   - Sabes que estás en la carpeta correcta cuando la línea del Command
     Prompt muestra el nombre de la carpeta, ejemplo:
     C:\Users\pablo\Desktop\Agent_betterway_ats>

3. Verifica que las carpetas de inputs existen:
   - Escribe este comando y presiona Enter:
```     dir inputs
```
   - Deberías ver dos carpetas: `candidates` y `jobs`
   - Si no aparecen, verifica que clonaste el repositorio correctamente
     en el paso anterior

4. Verifica que tus PDFs están en las carpetas correctas:
   - Para ver los candidatos:
```
     dir inputs\candidates
```
   - Para ver los puestos:
```
     dir inputs\jobs
```
   - Deberías ver tus archivos PDF listados

5. Inicia Claude Code escribiendo:
```
   claude
```
   - Claude Code se iniciará y mostrará un mensaje de bienvenida
   - Escribe este mensaje y presiona Enter:
   Please read and execute the agent.md file in this directory

- Claude Code leerá el `agent.md` y ejecutará el skill completo
     sin intervención adicional
   - Al finalizar verás en pantalla el link directo al reporte en Notion

## Notas importantes
- Nunca compartas tu token de Notion públicamente
- El skill es completamente reutilizable con cualquier tipo de vacante
- Los PDFs de inputs no se suben al repositorio — las carpetas están
  creadas pero vacías, solo debes copiar tus archivos dentro
- No se requiere código Python ni ninguna dependencia adicional
- Si Claude Code pide confirmación durante la ejecución, responde `yes`
- Si algo falla, verifica que el MCP de Notion está conectado ejecutando
  `claude mcp list` en el Command Prompt antes de iniciar