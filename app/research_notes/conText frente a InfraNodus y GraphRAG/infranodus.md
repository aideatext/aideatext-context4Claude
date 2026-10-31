# InfraNodus (infranodus.com): competidor y referencia para conText

Notas tomadas el 2026-10-30. La mayoría de las fuentes son páginas de la propia empresa, así que son afirmaciones de InfraNodus sobre sí misma. Las páginas sin fecha visible o con fecha anterior a 2024 se marcan como **[posiblemente desactualizado]**.

## 1. Qué es InfraNodus y cómo construye el grafo

### Takeaway
InfraNodus convierte cualquier texto en una red de co-ocurrencia de lemas. Usa una ventana de 4 palabras, quita stopwords, detecta clústeres con Louvain/modularidad y dimensiona los nodos por betweenness centrality. Muestra al usuario los clústeres temáticos, los conceptos más influyentes, los "structural gaps" entre clústeres y un puntaje de diversidad del discurso. Es un análisis estadístico y estructural del texto, no un modelo de lo que entiende una persona.

### Cited Findings
- Es una herramienta de análisis de texto que convierte el texto en un grafo de conocimiento: las palabras son nodos y sus co-ocurrencias son enlaces. Sirve para modelado de temas, para encontrar conceptos influyentes y para detectar huecos entre temas. La desarrolla Nodus Labs y se basa en un método con revisión por pares publicado en WWW'19 — [infranodus.com](https://infranodus.com/)
- Empresa: InfraNodus SAS (la política de privacidad también nombra a Nodus Labs como responsable), 60 rue François Ier, 75008 París, Francia — [Privacy Policy](https://infranodus.com/about/privacy-policy)
- Preprocesamiento: el texto se lematiza, se quitan las stopwords y cada lema es un nodo. Los nodos se conectan si aparecen dentro de una ventana de 4 palabras, y cuanto más cerca están, mayor es el peso del enlace — [How it works](https://infranodus.com/about/how-it-works)
- Pesos de los enlaces: palabras adyacentes = 3, con una palabra en medio = 2, con dos palabras en medio = 1. Las stopwords se filtran con listas y/o tf-idf. Ejemplo de lematización: "laboratories" → "laboratory" — [Text Network Analysis: Methodology](https://infranodus.com/docs/text-network-analysis)
- Layout ForceAtlas en 2D o 3D. El tamaño del nodo depende de la betweenness centrality. Las comunidades se detectan por modularidad (Louvain, Blondel et al.) y cada una tiene su color. Los enunciados se etiquetan con tema y sentimiento — [Methodology](https://infranodus.com/docs/text-network-analysis)
- Otras vistas: "entrance points" (conceptos con alta influencia por ocurrencia), "periphery" (ideas que conectan el texto con otros discursos) y los "structural gaps" (clústeres distintos y distantes que podrían conectarse) — [How it works](https://infranodus.com/about/how-it-works)
- Los structural gaps se describen como los "structural holes" de las ciencias sociales. La IA integrada revela esos huecos y propone preguntas de investigación que los conectan. La red se calcula con librerías de código abierto, entre ellas Cytoscape y Graphology — [Network analysis docs](https://infranodus.com/docs/network-analysis), [Generate insight](https://infranodus.com/docs/generate-insight)
- Stack declarado: NestJS, Prisma, PostgreSQL, Sigma.js, Cytoscape y Graphology. Algunas herramientas se publican bajo AGPL — [How it works](https://infranodus.com/about/how-it-works)
- Puntaje de "cognitive diversity" del discurso, con cuatro estados: Biased (bajo), Focused (medio), Diversified (óptimo) y Dispersed (muy alto). Se basa en la modularidad (>0,4 indica clústeres distintos) y en la distribución de la influencia. La herramienta recomienda acciones para cambiar de estado; por ejemplo, conectar clústeres poco influyentes cuando el discurso es "biased" — [Cognitive Diversity](https://infranodus.com/about/cognitive-diversity), [Cognitive Variability](https://infranodus.com/about/cognitive-variability), [Towards Data Science: Measuring Discourse Bias](https://towardsdatascience.com/measuring-discourse-bias-usingtext-network-analysis-9f251be5f6f3) **[artículo de TDS: posiblemente anterior a 2024]**
- Paper de referencia: Paranyushkin, D. (2019). "InfraNodus: Generating Insight Using Text Network Analysis", WWW '19, pp. 3584–3589 — [ACM DL](https://dl.acm.org/doi/10.1145/3308558.3314123) **[2019]**
- Paranyushkin presentó InfraNodus ("reveal non-obvious and find the gaps") en el 37C3 del Chaos Communication Congress, diciembre de 2023 — [CCC 2023 hub](https://events.ccc.de/congress/2023/hub/en/event/infranodus-reveal-non-obvious-and-find-the-gaps-wi/)

### Inferences
- El grafo de InfraNodus se basa en la proximidad de palabras dentro del texto. No extrae "ideas" como proposiciones ni relaciones semánticas tipadas. Un nodo es un lema y no una idea formulada. conText, en cambio, muestra ideas (probablemente extraídas con un LLM) con su origen.
- Las métricas de conText ("ideas que conectó el estudiante", "idea más central", "% de ideas sueltas") se parecen conceptualmente a las de InfraNodus: centralidad (betweenness), nodos periféricos o desconectados y gaps. InfraNodus es el antecedente metodológico obvio y sirve para justificar esas métricas con literatura (Paranyushkin 2019; los structural holes de Burt).
- La "diversidad del discurso" (biased/focused/diversified/dispersed) podría inspirar un indicador pedagógico en conText, aunque InfraNodus no lo presenta como medida de aprendizaje.

### Gaps
- No encontré documentación pública sobre el tratamiento de saltos de párrafo (InfraNodus históricamente usa una ventana adicional por enunciado o párrafo, pero no lo verifiqué en una fuente de 2026).
- No confirmé los umbrales exactos de cada estado de diversidad. Leí el texto completo del paper WWW'19 solo a través de resúmenes.

## 2. Rol de la IA/LLM y si existe un modo socrático o de diálogo

### Takeaway
La IA de InfraNodus (originalmente GPT; hoy OpenAI, Anthropic, Google y xAI a elección) sirve para resumir clústeres, generar preguntas de investigación que conectan gaps, proponer ideas y chatear sobre partes seleccionadas del grafo. Su función es dar ideas o insights al usuario. No hay un tutor socrático ni un diseño pedagógico que se niegue a "hacer la tarea".

### Cited Findings
- La IA integrada (los planes mencionan créditos de GPT-5) genera panoramas, resúmenes, preguntas de investigación, hechos e ideas nuevas. El análisis de gaps encuentra temas débilmente conectados y la IA propone cómo unirlos. Hay chat con IA sobre partes seleccionadas del grafo y sobre contenido de YouTube. El grafo puede "guiar" las respuestas del LLM y mantenerlas ancladas al material fuente — [infranodus.com](https://infranodus.com/)
- InfraNodus dice ser "la primera herramienta que usa GPT AI no solo para generar texto sino también para analizar texto". El análisis de red se usa para diseñar los prompts que se envían a los modelos — [How it works](https://infranodus.com/about/how-it-works)
- Proveedores de IA actuales: OpenAI, Anthropic (Claude), Google (Gemini) y xAI (Grok). Cuando una función necesita IA, el texto relevante se envía al proveedor que elige el usuario — [Privacy Policy, actualizada el 24-oct-2026](https://infranodus.com/about/privacy-policy)
- API "graphAndAdvice" con modo "question" para generar preguntas. Integración con Dify como GraphRAG: un botón "Generate Question" en el chatbot — [Dify blog](https://dify.ai/blog/level-up-your-dify-chatbot-integrating-infranodus-for-advanced-q-a-and-idea-generation)
- El servidor MCP puede analizar el razonamiento o el chat de un LLM para detectar sesgos y evaluar coherencia — [TypingMind MCP listing](https://www.typingmind.com/mcp/infranodus), [InfraNodus MCP](https://infranodus.com/mcp)
- Puede convertir una conversación de ChatGPT en una red de texto que muestra los temas principales y los huecos entre ellos (mediante la extensión del navegador) — [OpenAI Community](https://community.openai.com/t/infranodus-visualize-chatgpt-chats-high-level-ideas-and-gaps-within/269571) **[hilo de 2023]**
- Plugin de Obsidian: genera preguntas de investigación a partir de un gap seleccionado, resume fragmentos y ofrece chat sobre el grafo (GPT-4, sin necesidad de clave propia) — [Obsidian plugin](https://infranodus.com/obsidian-plugin) **[footer "© 2011-2024"]**
- La documentación de análisis cualitativo describe una función de GPT-4 que extrae temas e ideas de alto nivel, más resúmenes por tema. No menciona una interfaz de chat conversacional en ese flujo — [Qualitative thematic analysis](https://infranodus.com/docs/qualitative-thematic-analysis)

### Inferences
- La IA de InfraNodus actúa como un generador de ideas que responde al usuario. Su pregunta típica conecta dos clústeres ("¿cómo se relaciona X con Y?"). Es un "Socratic-lite" orientado a la investigación, no un tutor que guía con preguntas ni que retiene respuestas. Aquí conText se diferencia claramente: tutor socrático + estudiante adolescente + la negativa explícita a resolver la tarea.
- Las preguntas que conectan gaps son una técnica que conText podría adoptar: el tutor pregunta específicamente por las "ideas sueltas" o los clústeres desconectados que detecta el grafo.

### Gaps
- No encontré evidencia de un "modo tutor" o "modo socrático" en InfraNodus. La búsqueda fue limitada (sitio y docs públicos) y no revisé tutoriales en video.

## 3. Usuarios objetivo, casos de uso y oferta educativa

### Takeaway
InfraNodus apunta a investigadores, estudiantes universitarios o de posgrado, marketers, consultores, especialistas en SEO, escritores y builders de IA (GraphRAG/MCP). No encontré ninguna oferta para escuelas secundarias, docentes K-12 ni menores. La educación se limita a un plan Basic "para estudiantes" y a descuentos negociados caso por caso.

### Cited Findings
- Segmentos declarados: (1) investigadores y estudiantes: mapeo de literatura, análisis cualitativo y temático; (2) marketers y consultores: estudios de mercado, encuestas y reseñas, competencia, SEO y keyword gaps; (3) escritores: brainstorming, mapas mentales, escritura creativa, autorreflexión; (4) builders de IA: grafos como contexto GraphRAG o "reasoning experts". Organizaciones citadas: Greenpeace y Procter & Gamble — [infranodus.com](https://infranodus.com/)
- Caso de estudio universitario: Plymouth State University — [infranodus.com](https://infranodus.com/)
- Casos de uso adicionales: análisis de entrevistas, encuestas abiertas y transcripciones SRT — [Qualitative thematic analysis](https://infranodus.com/docs/qualitative-thematic-analysis), [Creative thinking use case](https://infranodus.com/use-case/creative-thinking)
- Educación: no hay plan estudiantil estándar. La empresa "en general no ofrece descuentos", pero está dispuesta a discutir una licencia educativa o un descuento. El usuario debe crear una cuenta y escribir indicando su institución, el uso previsto y cómo podría promocionar la herramienta (un blog post, o más de 10 cuentas). Opciones mencionadas: 20–80% de descuento, uso gratuito durante un proyecto, descuentos por volumen — [Support: Student discounts and educational licenses](https://support.noduslabs.com/hc/en-us/articles/19999990843932-Does-InfraNodus-Offer-Student-Discounts-and-Educational-Licenses) (no pude leer la página completa por un 403; el detalle viene del snippet del buscador)
- Una búsqueda de InfraNodus en aulas o con docentes no devolvió casos K-12 — [búsqueda; ver el listado de Toolmage](https://www.toolmage.com/ja/tool/infranodus/)

### Inferences
- InfraNodus es una herramienta para adultos y para el trabajo de conocimiento. Exige que el usuario interprete grafos (ver las debilidades en la sección 6). El segmento de escolares de secundaria y docentes de colegio, sobre todo en LatAm y en español, parece desatendido por InfraNodus.

### Gaps
- No hay cifras públicas de usuarios ni de clientes educativos. La home no da números concretos.

## 4. Precios, idiomas e integraciones (2025–2026)

### Takeaway
Hay tres planes pagos (Basic €12, Advanced €32 y Premium €66 al mes con facturación anual), todos con 14 días de prueba gratis. El plan Basic es solo para uso personal o académico. El ecosistema de integraciones es amplio: API, MCP, Obsidian, VS Code/Cursor, extensiones de Chrome y Firefox, n8n y skills para Claude Code. Dice soportar español en el análisis.

### Cited Findings
- Precios con facturación anual: Basic €12/mes, Advanced €32/mes, Premium €66/mes. Todos con 14 días de prueba gratis. Premium incluye una hora de capacitación al mes — [infranodus.com](https://infranodus.com/)
- Basic se presenta como el plan "para estudiantes y análisis ocasional", con uso personal o académico. Advanced añade el uso comercial — [resultado de búsqueda sobre infranodus.com / support](https://support.noduslabs.com/hc/en-us/articles/360014537620-Starting-a-Subscription)
- Conflicto de precios: un artículo de soporte más antiguo indica €19/mes para Basic, probablemente con facturación mensual. Los agregadores de terceros (SaaSworthy, aisotools) se contradicen sobre si hay plan gratuito — [Starting a Subscription](https://support.noduslabs.com/hc/en-us/articles/360014537620-Starting-a-Subscription), [SaaSworthy](https://www.saasworthy.com/product/infranodus/pricing), [aisotools](https://aisotools.com/pricing/infranodus)
- La página del plugin de Obsidian dice que "una cuenta gratuita basta para probarlo" y que el plan pago quita los límites de análisis y de peticiones a la IA. Esto sugiere una cuenta gratuita limitada, además de la prueba de 14 días — [Obsidian plugin](https://infranodus.com/obsidian-plugin)
- Idiomas: inglés, alemán, francés, portugués, español "y otros" — [infranodus.com](https://infranodus.com/). Los reseñadores de G2 elogian el soporte multilingüe — [G2](https://www.g2.com/products/infranodus/reviews)
- Integraciones: servidor MCP (ChatGPT, Claude y otros clientes), API, nodo de n8n, plugin de Obsidian, extensión de VS Code/Cursor, extensiones de Chrome y Firefox, skills para Claude Code. Fuentes de importación: PDF, Markdown, CSV, Google y Google Scholar, YouTube, sitios web, reseñas de Amazon y RSS — [infranodus.com](https://infranodus.com/), [MCP](https://infranodus.com/mcp), [API](https://infranodus.com/api), [API docs](https://infranodus.com/api/docs)
- El plugin de Obsidian es gratuito en el directorio de plugins de la comunidad. Analiza notas, carpetas, tags o resultados de búsqueda y detecta notas y conceptos que nunca se enlazaron — [Obsidian plugin](https://infranodus.com/obsidian-plugin), [Obsidian community](https://community.obsidian.md/plugins/infranodus-graph-view)

### Inferences
- InfraNodus cuesta unos €144/año como mínimo por usuario. Para un colegio peruano esto es caro y además no está pensado para cuentas de alumnos. conText puede diferenciarse con un acceso gratuito y sin cuenta.
- No hay evidencia de que InfraNodus acepte la foto de un cuaderno (OCR de escritura a mano) como entrada. Sus entradas son texto digital, PDF, web y YouTube.

### Gaps
- La página de precios no respondió a una consulta directa (404 en /pricing y /subscribe). Los montos vienen de la home y de snippets.
- No verifiqué si la interfaz está traducida al español ni la calidad de la lematización en español.
- No confirmé si soporta OCR de imágenes.

## 5. Privacidad, manejo de datos y aptitud para menores

### Takeaway
InfraNodus guarda los datos en AWS Irlanda bajo GDPR y ofrece un "Private Mode" que no almacena nada. Los grafos guardados se conservan hasta que el usuario los borra, es decir, no hay borrado automático. La política no menciona una edad mínima ni a menores. conText (borrado a los 7 días, pensado para escolares) se diferencia claramente en este punto.

### Cited Findings
- Los textos subidos y los grafos guardados se almacenan en AWS Irlanda (UE). Los grafos guardados y las "memories" persisten hasta que el usuario los elimina. Los datos de MCP/API no guardados no se conservan — [Privacy Policy, actualizada el 24-oct-2026](https://infranodus.com/about/privacy-policy)
- Private Mode: "It is not stored in our DB nor it is not saved in cache". Se puede borrar un grafo inmediatamente después de analizarlo — [Privacy Policy](https://infranodus.com/about/privacy-policy)
- En la API, por defecto `doNotSave=true` y no se guardan logs — [API docs](https://infranodus.com/api/docs) (vía resumen de búsqueda)
- Procesadores de IA: OpenAI, Anthropic, Google y xAI. Según InfraNodus, esos datos no se usan para entrenar modelos — [Privacy Policy](https://infranodus.com/about/privacy-policy)
- Páginas más antiguas afirmaban que solo se envían a OpenAI los conceptos de los clústeres principales, no el contenido completo. Esto puede estar desactualizado: la política de 2026 dice que se envía "el texto relevante" — [resultados de búsqueda en infranodus.com/support] y [Privacy Policy](https://infranodus.com/about/privacy-policy) (posible contradicción)
- El sitio usa Google Analytics, Facebook Pixel y HotJar — [Privacy Policy](https://infranodus.com/about/privacy-policy) (vía resumen de búsqueda)
- La política no establece una edad mínima ni trata a los menores de 13 o 16 años — [Privacy Policy](https://infranodus.com/about/privacy-policy)

### Inferences
- Sin una política para menores, con píxeles de marketing y sin borrado automático, InfraNodus no está diseñado para que lo usen adolescentes. Es un argumento de diferenciación fuerte para conText ante colegios y padres (cumplimiento de la Ley 29733 de Perú; esto es una inferencia y no está verificado aquí).

### Gaps
- No confirmé qué pasa con los datos al cerrar la cuenta (la política no lo aborda).

## 6. Fortalezas y debilidades (reseñas y discusión)

### Takeaway
Se elogian la visualización, el método con respaldo académico, el soporte multilingüe y el ecosistema de integraciones. Se critican la curva de aprendizaje, la interfaz confusa y la dificultad para interpretar los grafos. La muestra de reseñas es pequeña.

### Cited Findings
- G2 tiene solo 3 reseñas, con una media de 4,0. Las críticas incluyen "UI/UX un poco complicada", "confusa para un usuario nuevo" y que cuesta encontrar las funciones. Los elogios destacan la visualización y el soporte multilingüe — [G2 reviews](https://www.g2.com/products/infranodus/reviews)
- SaaSworthy: "puede ser algo técnica… lleva una curva de aprendizaje llegar a todas las funcionalidades" — [SaaSworthy reviews](https://www.saasworthy.com/product/infranodus/reviews)
- Fitgap señala como contra la "curva de aprendizaje para interpretar" los resultados. También dice que quien necesite clasificación automatizada a escala o ingesta omnicanal requerirá herramientas complementarias — [Fitgap](https://us.fitgap.com/products/003514/infranodus)
- Fortaleza: el método está publicado con revisión por pares (WWW'19) y la herramienta lleva activa desde ~2011 (footer "© 2011-2024") — [ACM](https://dl.acm.org/doi/10.1145/3308558.3314123), [Obsidian plugin](https://infranodus.com/obsidian-plugin)
- Neo4j Online Meetup #35 presentó InfraNodus para obtener insights de texto con visualización de grafos — [Neo4j](https://neo4j.com/videos/neo4j-online-meetup-35-get-insights-into-text-using-graph-visualization/) **[antiguo, anterior a 2024]**

### Inferences
- Interpretar un grafo de co-ocurrencia con betweenness y modularidad cuesta incluso a adultos. Para un escolar, conText debe traducir las métricas a lenguaje llano (como ya hace con "idea más central" o "% ideas sueltas"). InfraNodus no hace esa traducción pedagógica.

### Gaps
- No encontré reseñas en Capterra con contenido sustantivo ni hilos de Reddit (no se buscaron específicamente por el límite de llamadas).

## 7. Autoría y origen de las ideas, y medición del aprendizaje

### Takeaway
No encontré ninguna función de InfraNodus que marque el origen de cada idea (quién la dijo: material, estudiante o tutor) ni que mida el aprendizaje de una persona a lo largo del tiempo. Su unidad es un corpus de texto, no la interacción entre un aprendiz y una IA. Este es probablemente el principal diferenciador de conText.

### Cited Findings
- La documentación de análisis cualitativo no describe etiquetas por hablante ni por fuente. Lo único relacionado es un paso manual para eliminar texto, como las preguntas del entrevistador, del grafo — [Qualitative thematic analysis](https://infranodus.com/docs/qualitative-thematic-analysis)
- Lo más parecido en el MCP es un filtrado de entidades (personas, empresas u organizaciones) para construir redes sociales, no una atribución por hablante — [Glama: InfraNodus MCP README](https://glama.ai/mcp/servers/@infranodus/mcp-server-infranodus/blob/2b32674a5718be0e96115188609a70816d0b5441/README.md)
- Puede analizar un chat de ChatGPT como un todo (temas y gaps), sin distinguir los aportes del humano de los de la IA en lo que encontré — [OpenAI Community](https://community.openai.com/t/infranodus-visualize-chatgpt-chats-high-level-ideas-and-gaps-within/269571)
- Las métricas de "cognitive diversity" o "variability" se refieren a la estructura de un discurso (biased → dispersed), no al progreso de aprendizaje de un estudiante — [Cognitive Diversity](https://infranodus.com/about/cognitive-diversity)

### Inferences
- Rutas de diferenciación para conText frente a InfraNodus:
  1. Atribución del origen (material / estudiante / tutor) y la métrica "ideas que conectó el estudiante".
  2. Tutor socrático que no hace la tarea, frente a una IA que genera ideas o respuestas.
  3. Público escolar (14–17 años), en español y para LatAm.
  4. Entrada por foto del cuaderno.
  5. Privacidad para menores (borrado a los 7 días) frente a persistencia indefinida y píxeles de marketing.
  6. Gratuidad o bajo costo frente a €144–792/año.
  7. Métricas en lenguaje llano frente a betweenness y modularidad.
- Lo que conText podría tomar de InfraNodus: el concepto de "structural gaps" y la generación de preguntas que conectan clústeres, como fuente de las preguntas del tutor; la ventana de co-ocurrencia y la betweenness como fundamento citable de la "idea más central".

### Gaps
- InfraNodus podría permitir un análisis comparativo de dos grafos (por ejemplo, el texto del estudiante frente al material) mediante funciones no documentadas en las páginas consultadas. No pude verificarlo; de existir, sería lo más cercano a la atribución de origen.
