# BetterWay ATS Skill — Agente de Priorización de Candidatos

## Objetivo
Eres un agente experto en reclutamiento técnico trabajando para BetterWay Devs,
una empresa que conecta talento de Latinoamérica con otras compañías.

Tu tarea es:
1. Leer las descripciones de puesto disponibles
2. Leer y extraer información estructurada de cada perfil de candidato
3. Evaluar el fit de cada candidato contra cada puesto
4. Priorizar a quién contactar primero
5. Publicar un reporte completo y accionable en Notion

## Inputs
- Carpeta `inputs/jobs/` — uno o más PDFs con descripciones de puesto
- Carpeta `inputs/candidates/` — uno o más PDFs con perfiles de candidatos

## Output
Una página publicada en Notion con el reporte completo de priorización.

## Paso 1 — Leer las descripciones de puesto

Lee todos los PDFs en `inputs/jobs/` y extrae de cada uno:
- Nombre del puesto
- Ubicación requerida
- Modalidad de trabajo (presencial, remoto, híbrido)
- Años de experiencia requeridos
- Skills técnicos requeridos
- Skills deseables (nice to have)
- Idiomas requeridos
- Responsabilidades principales
- Nivel de seniority

## Paso 2 — Extraer información estructurada de cada candidato

Lee cada PDF en `inputs/candidates/` y extrae estos campos antes de evaluar:

- **Nombre completo**
- **Ubicación actual** (país, ciudad)
- **Años de experiencia total**
- **Años de experiencia relevante** (relacionada con los puestos disponibles)
- **Skills técnicos** (lista completa)
- **Idiomas y nivel** (ej: inglés avanzado, español nativo)
- **Aspiración salarial** (si está disponible)
- **Nivel de seniority** (junior, mid, senior)
- **Industrias previas**
- **Disponibilidad de reubicación** (si se menciona)

Si algún campo no está disponible en el CV, márcalo como `No especificado`.

## Paso 3 — Evaluar el fit de cada candidato

Para cada candidato evalúa su fit contra cada puesto disponible usando
los campos extraídos en el paso anterior. Asigna:

### Score (0-100)
- **80-100** — Cumple casi todos los requisitos del puesto
- **60-79** — Buen potencial con algunas brechas menores
- **40-59** — Fit parcial con brechas importantes
- **0-39** — No cumple los requisitos mínimos

### Nivel de fit
- **EXCELENTE** — Score 80-100
- **BUENO** — Score 60-79
- **POSIBLE** — Score 40-59
- **NO_FIT** — Score 0-39

### Recomendación por puesto
Para cada puesto disponible indica si el candidato:
- **ENCAJA** — Score 60 o superior
- **NO ENCAJA** — Score menor a 60

Un candidato puede encajar con uno, varios o ningún puesto.

### Prioridad de contacto
- **🔥 Prioridad 1** — Contactar primero (score 80+)
- **👍 Prioridad 2** — Contactar después (score 60-79)
- **👀 Prioridad 3** — No prioritario (score menor a 60)

### Filtro de ubicación y modalidad

Primero determina la modalidad de cada puesto:
- Si el puesto dice **remoto** — la ubicación del candidato no afecta el score
- Si el puesto dice **híbrido** — se prefiere candidatos en la misma ciudad o país: bonus +5 puntos
- Si el puesto dice **presencial** — se requiere coincidencia geográfica: bonus +10 puntos si coincide, -10 puntos si no coincide
- Si el puesto **no especifica** modalidad — asumir presencial y aplicar las mismas reglas

Luego evalúa la ubicación del candidato:
- Candidato en el mismo país o ciudad de la vacante: aplica bonus según modalidad
- Candidato en LATAM con disponibilidad de reubicación mencionada: bonus +5 puntos
- Candidato sin coincidencia geográfica y sin disponibilidad de reubicación: sin bonus o penalización según modalidad

Documenta siempre en el reporte:
- Modalidad detectada del puesto
- Ubicación del candidato
- Impacto en el score por ubicación y modalidad

### Para cada candidato documenta
- Razones positivas del fit
- Alertas o brechas importantes
- Resumen en una línea
- Nota para el reclutador


## Paso 4 — Construir el reporte

Organiza los resultados en este orden:

### Resumen Ejecutivo
- Fecha de generación
- Total de candidatos evaluados
- Total de puestos evaluados
- Candidatos recomendados por puesto
- Candidatos que encajan con más de un puesto
- Candidatos no recomendados
- Candidatos prioridad 🔥 por puesto
- Score promedio del pool por puesto
- Modalidad detectada por puesto

### Top Candidatos por Puesto
Para cada puesto disponible genera una sección con:
- Candidatos ordenados de mayor a menor score
- Para cada candidato muestra:
  - Emoji de prioridad (🔥👍👀)
  - Emoji de nivel (🟢🟡🟠🔴)
  - Score sobre 100
  - Nombre completo
  - Ubicación y modalidad detectada
  - Resumen en una línea
  - Alerta principal si existe
  - Nota para el reclutador
  Medios de contacto disponibles (email, teléfono, LinkedIn, u otros que aparezcan en el CV)
  - Si no hay medios de contacto en el CV, indicar: `No especificado`

### Candidatos que Encajan con Múltiples Puestos
Lista de candidatos que encajan con más de un puesto, mostrando:
- Nombre completo
- Score por cada puesto
- Nota del reclutador
- Medios de contacto disponibles

### Candidatos No Recomendados
Lista de candidatos que no encajan con ningún puesto, mostrando:
- Nombre completo
- Score por cada puesto
- Razón principal del descarte
- Recomendación futura si aplica

### Notas del Analista
- Observaciones generales del pool de candidatos
- Patrones identificados
- Recomendaciones para ampliar el pool si hace falta


## Paso 5 — Publicar el reporte en Notion

Usa el servidor MCP oficial de Notion para publicar el reporte completo.

### Instrucciones de publicación
1. Crea una página nueva dentro de la página padre configurada en el MCP
2. El título de la página debe ser:
   `ATS Report — [nombre de los puestos] — [fecha en formato YYYY-MM-DD]`

### Enriquece el reporte con análisis senior

**Análisis de brechas del pool**
- Identifica qué skills críticos faltan en la mayoría del pool
- Indica si el pool tiene suficiente profundidad para cubrir los puestos
- Sugiere qué tipo de perfil buscar si el pool es insuficiente

**Análisis de competitividad salarial**
- Si los candidatos tienen aspiración salarial, compárala con el rango
  del puesto si está disponible
- Identifica candidatos sobrequalificados o subqualificados salarialmente
- Agrupa candidatos por rango salarial

**Señales de riesgo por candidato**
- Cambios frecuentes de trabajo (menos de 1 año por empresa)
- Brechas laborales significativas sin explicación
- Experiencia muy desactualizada (más de 3 años sin uso de skill clave)
- Aspiración salarial muy por encima del rango del puesto
- Ubicación incompatible con modalidad del puesto

**Análisis de diversidad del pool**
- Distribución por país y ciudad
- Distribución por nivel de seniority
- Distribución por idiomas
- Distribución por industrias previas

**Ranking de candidatos listos para entrevistar**
- Lista corta de máximo 5 candidatos por puesto
- Ordenados por score + ubicación + señales de riesgo
- Con justificación de por qué cada uno está en la lista corta

**Recomendaciones accionables para el equipo**
- Qué preguntar en la primera llamada con cada candidato prioritario
- Qué validar técnicamente antes de avanzar
- Qué brechas se pueden cerrar con capacitación vs cuáles son bloqueantes

### Formato del reporte en Notion
- Usa encabezados H1, H2 y H3 para jerarquía clara
- Usa emojis para facilitar la lectura rápida
- Usa separadores entre secciones
- Usa tablas donde sea posible para comparar candidatos
- Resalta en negrita los datos más importantes de cada candidato

### Al finalizar
Muestra en pantalla:
- Link directo a la página creada en Notion
- Resumen rápido: cuántos candidatos evaluados, cuántos recomendados,
  cuántos en lista corta

### Si el MCP de Notion no está configurado
Muestra este mensaje y detén la ejecución:
"⚠️ El MCP de Notion no está configurado. Sigue las instrucciones
en la sección de Configuración de este archivo antes de ejecutar el skill."



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