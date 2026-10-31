# GraphRAG y sus variantes como referencia para conText

Fecha de consulta: 30 oct 2026. Las fuentes se consultaron ese día. Las páginas de GitHub muestran el estado de ese momento (estrellas y noticias). Marco con **[antiguo]** la información de 2024 o anterior.

## 1. Microsoft GraphRAG: para qué sirve, pipeline, costo, LazyGraphRAG, estado y licencia

### Takeaway
GraphRAG es infraestructura para desarrolladores, no un producto para aprendices. Un LLM extrae entidades y relaciones de un corpus grande (del orden de 1M de tokens). Luego Leiden jerárquico agrupa el grafo en comunidades y el LLM resume cada una, de modo que se pueden responder preguntas globales de sensemaking ("¿cuáles son los temas principales?"). Indexar es caro. LazyGraphRAG traslada el uso del LLM al momento de la consulta e indexa por un 0,1 % del costo. El repositorio es MIT y Microsoft lo declara "largely in maintenance mode", aunque se siguen publicando versiones 3.x en PyPI.

### Cited Findings
- Paper de Edge et al. ("From Local to Global"). Un LLM construye el índice en dos etapas: primero un grafo de entidades a partir de los documentos y luego resúmenes pregenerados por comunidad. Para responder, cada resumen de comunidad produce una respuesta parcial y esas parciales se resumen en la respuesta final. En preguntas de sensemaking global sobre datasets de ~1M de tokens supera a la RAG convencional en exhaustividad (comprehensiveness) y diversidad. Versión v1 del 24 abr 2024 [antiguo], v2 revisada el 19 feb 2025. — [arXiv 2404.16130](https://arxiv.org/abs/2404.16130)
- Pipeline de indexado por defecto:
  - Se trocea el texto en TextUnits de 1200 tokens por defecto.
  - El LLM extrae entidades (título, tipo, descripción) y relaciones (origen, destino, descripción).
  - El LLM resume las descripciones acumuladas de cada entidad y relación.
  - La extracción de claims (covariables con estado y límites temporales) es opcional, viene apagada y suele requerir ajuste de prompts.
  - **Hierarchical Leiden** agrupa el grafo de forma recursiva hasta un umbral de tamaño.
  - El LLM escribe un "community report" por comunidad.
  - Se calculan embeddings de descripciones, TextUnits y reports.
  
  — [GraphRAG docs: Default dataflow](https://microsoft.github.io/graphrag/index/default_dataflow/)
- **Resolución de entidades ingenua.** Las entidades se fusionan cuando comparten el mismo título y tipo, y sus descripciones se acumulan en una lista. Las relaciones se fusionan por el par origen/destino. — [GraphRAG docs: Default dataflow](https://microsoft.github.io/graphrag/index/default_dataflow/)
- Existe un modo **FastGraphRAG** que extrae entidades y relaciones con NLP en lugar del LLM, para ahorrar llamadas. En este modo no se extraen claims. — [GraphRAG docs: Default dataflow](https://microsoft.github.io/graphrag/index/default_dataflow/)
- Advertencia oficial de costo: "GraphRAG indexing can be an expensive operation". La recomendación es leer la documentación de costos y empezar con poco. — [GitHub microsoft/graphrag](https://github.com/microsoft/graphrag)
- Estado y licencia del repositorio:
  - Licencia MIT.
  - El proyecto está "largely in maintenance mode": no acepta PRs ni features nuevas, pero sigue con correcciones de bugs y actualizaciones de dependencias (CVE).
  - Se presenta como código de demostración y "not an officially supported Microsoft offering".
  
  — [GitHub microsoft/graphrag](https://github.com/microsoft/graphrag)
- Versiones en PyPI: 3.3.0 (8 oct 2026), 3.2.0 (23 sep 2026), 3.1.2 (21 ago 2026), 3.1.1 (18 jul 2026) y 3.1.0 (28 may 2026). El historial también muestra una 1.2.4 con fecha 20 oct 2026, sin explicación; puede ser un backport o un error de datos. — [PyPI graphrag](https://pypi.org/project/graphrag/)
- **LazyGraphRAG** (Microsoft Research, 25 nov 2024 [antiguo]). Indexado:
  - No resume nada con LLM al indexar. Construye un grafo de conceptos extrayendo frases nominales con NLP y saca las comunidades jerárquicas con estadística de grafos.
  - El costo de indexado es "idéntico a vector RAG" y equivale al **0,1 %** del de GraphRAG completo.
  
  — [MSR Blog LazyGraphRAG](https://www.microsoft.com/en-us/research/blog/lazygraphrag-setting-a-new-standard-for-quality-and-cost/)
- LazyGraphRAG, consulta:
  - El LLM descompone la pregunta en subconsultas.
  - Combina búsqueda best-first (similitud de embeddings) con búsqueda en anchura y profundización iterativa, limitadas por un "relevance test budget".
  - Iguala a GraphRAG Global Search en preguntas globales con un costo de consulta más de 700 veces menor.
  - Con un 4 % de ese costo supera a todos los métodos comparados.
  
  — [MSR Blog LazyGraphRAG](https://www.microsoft.com/en-us/research/blog/lazygraphrag-setting-a-new-standard-for-quality-and-cost/)
- Cómo se evaluó LazyGraphRAG: 5590 noticias de AP y 100 preguntas sintéticas (50 locales y 50 globales). Un LLM juez comparó respuestas por pares (head-to-head). Según una nota del 6 jun 2025, la tecnología se integró en Microsoft Discovery y en Azure Local (public preview). — [MSR Blog LazyGraphRAG](https://www.microsoft.com/en-us/research/blog/lazygraphrag-setting-a-new-standard-for-quality-and-cost/)

### Inferences
- GraphRAG está pensado para corpus grandes y estáticos, preguntas de un analista y procesamiento por lotes. Un grafo por sesión, incremental y de decenas de nodos, como el de conText, está en el extremo opuesto de escala y propósito.
- La evaluación de LazyGraphRAG la hizo el propio Microsoft con preguntas sintéticas y un LLM como juez. Conviene tomarla como evidencia de proveedor, no como benchmark independiente.
- La aparente contradicción entre "maintenance mode" y la publicación de 3.x en 2026 se explica si se lee como "sin features nuevas, pero con releases de mantenimiento". No lo confirmé en el changelog.

### Gaps
- No encontré cifras actuales en dólares por millón de tokens indexados. La documentación solo advierte que es caro.
- No verifiqué qué cambió en las versiones 3.x ni si LazyGraphRAG está disponible en el paquete open-source. Microsoft solo lo menciona en productos Azure y Discovery.

## 2. Variantes: LightRAG, nano-graphrag, Neo4j GraphRAG, Graphiti/Zep, Cognee

### Takeaway
Todas son bibliotecas o infraestructura para desarrolladores que construyen RAG o memoria de agentes. Ninguna es una aplicación para estudiantes.
- **LightRAG** (MIT, unas 40k estrellas): recuperación dual local/global sin community reports y con inserciones incrementales.
- **nano-graphrag** (MIT, ~1100 líneas): reimplementación mínima y fácil de modificar.
- **Neo4j GraphRAG** (Apache-2.0, mantenido por Neo4j): pipeline con esquema y resolvers de entidades (fuzzy o spaCy).
- **Graphiti/Zep** (Apache-2.0, unas 32k estrellas): grafo temporal incremental con deduplicación por LLM e invalidación de hechos contradictorios. Es lo más parecido al flujo turno a turno de conText.
- **Cognee** (Apache-2.0, unas 33k estrellas): plataforma de memoria para agentes con ontologías.

### Cited Findings
- **LightRAG (HKUDS)**:
  - Se presenta como "efficient alternative to Microsoft GraphRAG".
  - Recuperación de doble nivel: *local* (entidades concretas), *global* (temas y cadenas de relaciones) y modos *hybrid* y *mix* (este último por defecto).
  - Evita los community reports y el razonamiento multi-hop, lo que reduce las llamadas al LLM.
  - Tiene fases de "merge" de entidades y relaciones con resumen LLM al fusionar (`FORCE_LLM_SUMMARY_ON_MERGE`).
  - Licencia MIT, unas 40,4k estrellas.
  - Noticias recientes: en 2026.07, reconocimiento de encabezados en Word; en 2026.05 se integró RAG-Anything; en 2026.03 se añadió el backend OpenSearch; en 2025.11, RAGAS y Langfuse; en 2025.08, reranker y borrado de documentos con regeneración del grafo.
  - Backends: por defecto NetworkX, NanoVectorDB y JSON. Para producción recomienda PostgreSQL y también admite Neo4j, Memgraph, Milvus, Qdrant y MongoDB.
  
  — [GitHub HKUDS/LightRAG](https://github.com/HKUDS/LightRAG)
- **nano-graphrag**:
  - "A simple, easy-to-hack GraphRAG implementation" de unas 1100 líneas sin contar tests ni prompts.
  - Licencia MIT.
  - API asíncrona, inserción incremental por hash, modos local, global y naive, y exportación a GraphML.
  - Grafo en NetworkX o Neo4j.
  - Unas 4,0k estrellas, 67 issues abiertas. No pude ver la fecha del último commit.
  
  — [GitHub gusye1234/nano-graphrag](https://github.com/gusye1234/nano-graphrag)
- **Neo4j GraphRAG for Python**:
  - Paquete oficial de Neo4j, "first-party", con soporte a largo plazo.
  - `SimpleKGPipeline` y `Pipeline` extraen entidades y relaciones de texto o PDF a Neo4j siguiendo un **esquema** de tipos de nodos, tipos de relaciones y patrones permitidos.
  - Resolvers experimentales: `FuzzyMatchResolver` (RapidFuzz) y `SpaCySemanticMatchResolver` (spaCy).
  - Retrievers: vector, hybrid y Text2Cypher.
  - Proveedores de LLM soportados, entre otros: OpenAI, Anthropic, Ollama y Bedrock.
  - Licencia Apache 2.0, unas 1,3k estrellas.
  
  — [GitHub neo4j/neo4j-graphrag-python](https://github.com/neo4j/neo4j-graphrag-python)
- **Graphiti (getzep)**:
  - Framework de "temporal context graphs" para agentes que integra episodios de forma incremental, sin recomputar todo el grafo.
  - Los hechos tienen ventana de validez. Cuando llega un hecho contradictorio, el anterior se **invalida** pero se conserva en el historial.
  - Extracción y deduplicación de entidades y aristas con salidas JSON estructuradas del LLM, con prompts sobrescribibles. La deduplicación puede delegarse a modelos externos (por defecto, umbral de duplicado 0,7 y de contradicción 0,8).
  - Admite tipos de entidad personalizados con Pydantic.
  - Licencia Apache-2.0, unas 32,4k estrellas. Zep es la versión gestionada.
  - Su propia tabla lo compara con GraphRAG: GraphRAG trabaja por lotes, con latencias de segundos a decenas de segundos; Graphiti es incremental y "typically sub-second".
  
  — [GitHub getzep/graphiti](https://github.com/getzep/graphiti)
- **Paper de Zep** (20 ene 2025): 94,8 % en DMR frente a 93,4 % de MemGPT. En LongMemEval, hasta +18,5 % de exactitud y -90 % de latencia frente a la línea base. — [arXiv 2501.13956](https://arxiv.org/abs/2501.13956)
- **Cognee (topoteretes)**:
  - Plataforma open-source de memoria para agentes que convierte documentos, código y conversaciones en un grafo de conocimiento. Operaciones: remember, recall, improve y forget.
  - Admite ontologías y modelos de datos propios.
  - Puede trabajar en local, sin clave de LLM: extrae entidades con GLiNER y usa embeddings locales.
  - Licencia Apache-2.0, unas 32,9k estrellas.
  - Versiones v1.6.1 (24 sep 2026) y v1.6.0 (18 sep 2026).
  - El almacén de grafos sobre Postgres es solo una demo; la versión de producción es un producto con licencia.
  - Reporta 0,79 en BEAM a 100K tokens, con configuraciones específicas para ese benchmark.
  
  — [GitHub topoteretes/cognee](https://github.com/topoteretes/cognee)

### Inferences
- Para conText, Graphiti es la referencia conceptual más cercana, por cuatro razones:
  - El grafo se construye por episodios (turnos).
  - Deduplica contra el grafo existente.
  - Detecta contradicciones.
  - Acepta tipos de entidad personalizados, parecidos a los tipos concept/claim/evidence/... de conText.
  
  Aun así, está diseñado como memoria de agentes, no como visualización pedagógica.
- LightRAG y nano-graphrag servirían como backend si conText quisiera algún día hacer recuperación sobre el material del estudiante. Para un grafo de ≤ decenas de nodos por sesión son excesivos.

### Gaps
- No encontré el algoritmo exacto de deduplicación por defecto de Graphiti en el README; el paper completo de Zep lo describe y no lo leí.
- No vi la licencia comercial ni el modelo de negocio de Cognee más allá de la nota sobre Postgres. No verifiqué su financiación.
- No vi la fecha del último commit de nano-graphrag.

## 3. Limitaciones conocidas: costo, calidad de extracción, duplicados, latencia, benchmarks

### Takeaway
GraphRAG no gana siempre. En búsqueda de hechos simples, la RAG básica empata o gana. GraphRAG solo aventaja en razonamiento complejo y resumen, y a cambio usa prompts mucho más largos: el modo global de MS-GraphRAG llega a ~331k tokens de prompt de media. La resolución de entidades suele ser coincidencia de cadenas y es una fuente reconocida de ruido.

### Cited Findings
- **GraphRAG-Bench** ("When to use Graphs in RAG", Xiang et al., arXiv jun 2025, v3 feb 2026, ICLR 2026):
  - En recuperación de hechos, la RAG básica iguala o supera a GraphRAG por el ruido que introduce el grafo.
  - En razonamiento complejo, la RAG logra ~41–43 %, frente a 53,4 % de HippoRAG2 y 50,9 % de MS-GraphRAG (dataset de novelas).
  - En resumen contextual, la RAG logra ~50–51 %, frente a 64,4 % de MS-GraphRAG.
  
  — [arXiv 2506.05690v3](https://arxiv.org/html/2506.05690v3); [ICLR 2026](https://iclr.cc/virtual/2026/poster/10007992)
- Tokens de prompt promedio en GraphRAG-Bench (novelas):

  | Método | Tokens de prompt |
  |---|---|
  | RAG básica | 879 |
  | HippoRAG2 | 1008 |
  | Fast-GraphRAG | 4204 |
  | MS-GraphRAG local | 38 707 |
  | LightRAG | 100 832 |
  | MS-GraphRAG global | 331 375 |

  Los autores señalan que el prompt crece con la dificultad de la tarea y puede añadir ruido. — [arXiv 2506.05690v3](https://arxiv.org/html/2506.05690v3)
- Ese mismo paper cita trabajos previos en los que GraphRAG fue un 13,4 % menos exacto que la RAG básica en Natural Questions y un 16,6 % peor en preguntas sensibles al tiempo. En HotpotQA multi-hop, la recuperación por grafo dio +4,5 % a cambio de 2,3× más latencia. — [arXiv 2506.05690v3](https://arxiv.org/html/2506.05690v3)
- Densidad de los grafos: MS-GraphRAG, LightRAG y HippoRAG generan grafos con grado medio de ~1,5–2,1, es decir, poco conectados. HippoRAG2 llega a 8,75–13,3. — [arXiv 2506.05690v3](https://arxiv.org/html/2506.05690v3)
- **Resolución de entidades** (Deg-RAG, arXiv 2510.14271):
  - MS GraphRAG y LightRAG detectan duplicados por coincidencia simple de cadenas.
  - Durante la extracción, los LLM no mantienen consistentes los nombres de entidades a lo largo de contextos largos, y eso genera duplicados.
  - El paper compara de forma sistemática estrategias de *blocking*, embeddings, métricas de similitud y métodos de fusión.
  
  — [arXiv 2510.14271](https://arxiv.org/html/2510.14271v1)
- La documentación oficial de GraphRAG confirma la fusión por título y tipo idénticos. — [GraphRAG docs](https://microsoft.github.io/graphrag/index/default_dataflow/)
- Fuentes de practicantes (menos rigurosas) enumeran entidades omitidas, entidades duplicadas, etiquetas de relación débiles y claims mal extraídos. También mencionan el costo de la deduplicación con LLM como juez. — [Rubin Lake](https://rubinlake.com/en/technology-radar/ai-and-data-engineering/graphrag-context-graphs); [Substack "When Knowledge Graphs Mislead"](https://thecodexandthecompass.substack.com/p/when-knowledge-graphs-mislead)

### Inferences
- Los problemas de duplicados y nombres inconsistentes aparecen en cualquier extracción turno a turno, incluida la de conText. Es probable que en conText sean más visibles porque el estudiante ve el grafo.
- El grado medio bajo (~2) de los grafos extraídos por LLM sugiere que un grafo de conText también tenderá a ser disperso. Las relaciones explícitas y tipadas que pide conText pueden ser una ventaja pedagógica frente a grafos que solo existen para recuperar.

### Gaps
- No leí el apéndice de costo de construcción del grafo (G.3) de GraphRAG-Bench.
- No encontré una encuesta dedicada que cubra de punta a punta la resolución de entidades en GraphRAG.

## 4. Aplicaciones en educación y tutoría (2024–2026)

### Takeaway
Hay investigación sobre tutores con grafo de conocimiento (KG-RAG), GraphRAG en MOOCs con grafos personales del aprendiz, y mapas conceptuales generados por LLM. En casi todos los casos el grafo lo construye el curso o el docente y sirve para recuperar o para generar preguntas. No encontré sistemas en los que el **estudiante vea un grafo construido en vivo a partir de su propio diálogo**, que es el nicho de conText.

### Cited Findings
- **KG-RAG para tutores adaptativos** (Dong et al., v1 nov 2023, v7 feb 2025): combina representación estructurada del conocimiento con recuperación sensible al contexto para anclar las respuestas del tutor. En un experimento controlado con 76 estudiantes, las puntuaciones de evaluación subieron un 35 % (p<0,001). Un resumen secundario reporta medias de 6,37 frente a 4,71. — [arXiv 2311.17696v7](https://arxiv.org/abs/2311.17696v7)
- **Graph RAG en MOOCs** (Abdelmagied, Chatti et al., EMOOCs 2025): pipeline en la plataforma CourseMapper con dos grafos. El Educational KG sirve para responder preguntas usando las relaciones entre conceptos. El Personal KG del aprendiz sirve para generar y recomendar preguntas personalizadas. La evaluación fue con 3 instructores expertos en 3 MOOCs y no reporta métricas cuantitativas. — [arXiv 2505.10074](https://arxiv.org/abs/2505.10074v1)
- Estudio que compara RAG y GraphRAG para responder preguntas a nivel de página sobre un libro de matemáticas universitario. — [arXiv 2509.16780](https://arxiv.org/pdf/2509.16780v1) (solo vi el resumen del buscador)
- Generación adaptativa de ejercicios: construye un grafo de conocimiento a partir de bancos de preguntas y razona sobre un grafo de dominio del aprendiz. — [arXiv 2602.00020](https://arxiv.org/pdf/2602.00020v2) (solo vi el resumen del buscador)
- Mapas conceptuales para generar preguntas de opción múltiple de física de secundaria: un mapa conceptual guía al LLM y cada distractor queda ligado a una concepción errónea concreta. Superó a un LLM base y a una RAG en revisión experta. — [arXiv 2505.02850](https://arxiv.org/pdf/2505.02850)
- Mapas conceptuales hechos con ChatGPT (Schicchi et al., Interactive Learning Environments 2025, 83 estudiantes de secundaria): calidad comparable a los mapas de docentes y menos tiempo para producirlos. — [IRIS Roma Tre](https://iris.uniroma3.it/handle/11590/511000)

### Inferences
- La evidencia de KG-RAG en tutoría tiene que ver con el **anclaje** (menos alucinación, más coherencia con el curso), no con mostrarle el grafo al estudiante. conText podría citarla como apoyo para grounding con el grafo, no para la visualización.
- El Personal KG de CourseMapper (generar preguntas a partir del grafo del aprendiz) es la idea académica más cercana al tutor socrático de conText.

### Gaps
- No encontré estudios con resultados de aprendizaje sobre grafos *visibles* construidos en vivo desde el diálogo tutor-estudiante.
- Tampoco encontré trabajos que combinen knowledge tracing (BKT/DKT) con GraphRAG basado en LLM. No hice una búsqueda específica, así que sigue pendiente.
- No leí los textos completos de 2509.16780 ni de 2602.00020.

## 5. Qué técnicas podrían mejorar un grafo de ideas pequeño por sesión como el de conText

### Takeaway
conText no compite con GraphRAG: aquel es infraestructura de recuperación para desarrolladores y conText es un producto pedagógico para el estudiante. Adoptar GraphRAG o LightRAG completos sería sobredimensionado. Sí conviene tomar prestadas cuatro ideas baratas: (a) resolución de entidades al insertar, (b) comunidades (Leiden/Louvain) para mostrar clusters, (c) resúmenes por cluster e invalidación de contradicciones al estilo Graphiti, y (d) usar el grafo para anclar los prompts del tutor y detectar huecos.

### Cited Findings
- Los resolvers de Neo4j combinan fuzzy matching con RapidFuzz y similitud semántica con spaCy. Es un patrón barato para fusionar nodos casi idénticos. — [neo4j-graphrag-python](https://github.com/neo4j/neo4j-graphrag-python)
- Graphiti deduplica cada episodio nuevo contra el grafo existente (vía LLM o un modelo de decisión con umbrales de duplicado 0,7 y contradicción 0,8) e invalida, sin borrar, los hechos contradichos. — [getzep/graphiti](https://github.com/getzep/graphiti)
- La fusión por título y tipo de GraphRAG es demasiado ingenua. Deg-RAG muestra que conviene combinar *blocking*, embeddings, métrica de similitud y política de fusión. — [GraphRAG docs](https://microsoft.github.io/graphrag/index/default_dataflow/); [arXiv 2510.14271](https://arxiv.org/html/2510.14271v1)
- GraphRAG usa Hierarchical Leiden y luego un LLM resume cada comunidad. — [GraphRAG docs](https://microsoft.github.io/graphrag/index/default_dataflow/)
- LazyGraphRAG obtiene las comunidades con estadística de grafos, sin LLM, y deja el resumen para el momento de la consulta. — [MSR Blog](https://www.microsoft.com/en-us/research/blog/lazygraphrag-setting-a-new-standard-for-quality-and-cost/)
- El Personal KG del aprendiz se usa para generar preguntas personalizadas. — [arXiv 2505.10074](https://arxiv.org/abs/2505.10074v1)
- Los mapas conceptuales ligan distractores a concepciones erróneas. — [arXiv 2505.02850](https://arxiv.org/pdf/2505.02850)

### Inferences
Propuestas concretas para conText, derivadas de las fuentes; no las he probado.
1. **Resolución de entidades al insertar** (prioridad alta y baja complejidad):
   - Antes de añadir un nodo, se normaliza la etiqueta (minúsculas, acentos, lematización simple).
   - Se compara con fuzzy matching y con similitud de embeddings contra los nodos existentes de la sesión. Con ≤100 nodos esto cuesta milisegundos.
   - Si se supera un umbral, se fusiona: se conserva el origen (material/estudiante/tutor) como multi-etiqueta y se acumulan descripciones.
   - Alternativa sin código adicional: pasarle al LLM extractor la lista de etiquetas existentes y pedirle que reutilice IDs. Es lo que hace Graphiti, que deduplica contra el grafo vivo.
2. **Comunidades para mostrar clusters**:
   - Louvain o Leiden sobre el grafo de la sesión, por ejemplo con `graphology-communities-louvain` en JS o NetworkX en el servidor. En grafos tan pequeños es instantáneo y no consume LLM, como en LazyGraphRAG.
   - Se colorean los clusters y, opcionalmente, un LLM barato pone un título de 3–5 palabras a cada uno (un "community report" mínimo).
   - Advertencia: en grafos de 10–40 nodos y grado ~2, la detección de comunidades puede ser inestable. Conviene fijar una semilla y suavizar los cambios entre turnos.
3. **Contradicciones al estilo Graphiti**: cuando aparece una arista `contradicts` o un claim nuevo choca con otro anterior, no hay que borrar nada. Se marca el anterior como "revisado" o "superado" y se muestra la evolución, que tiene valor pedagógico porque el cambio conceptual queda visible.
4. **Prompts del tutor anclados en el grafo**: en cada turno se inyecta un resumen compacto del grafo (nodos y aristas tipadas por origen) en el prompt del tutor socrático, para que pregunte por:
   - claims sin `evidence` que los soporte;
   - nodos `question` sin arista `answers`;
   - conceptos del *material* que aún no tocó el *estudiante*;
   - clusters desconectados, para hacer preguntas puente.
   
   Es la versión "local search" de GraphRAG, pero con fines pedagógicos. También se alinea con el KG-RAG de tutoría (anclaje) y con el Personal KG que genera preguntas.
5. **Detección de huecos con estadística de grafo, sin LLM**:
   - componentes desconectados;
   - nodos aislados;
   - grado bajo en conceptos centrales del material;
   - proporción de nodos por origen (por ejemplo, si casi todo viene del tutor y no del estudiante, eso indica baja agencia).
6. **Lo que no conviene adoptar**:
   - el indexado de GraphRAG completo, por costo, latencia y escala;
   - la búsqueda global por map-reduce, que usa ~331k tokens de prompt de media;
   - backends de grafo pesados (Neo4j) para grafos efímeros por sesión.
   
   Como mucho, LightRAG o nano-graphrag podrían servir si conText añadiera RAG sobre materiales largos subidos por el estudiante.
7. **Posicionamiento**: GraphRAG, LightRAG, Graphiti y Cognee son infraestructura para desarrolladores, con licencias MIT o Apache que permitirían usarlos. Ninguno es una app de aprendizaje, así que conText no compite con ellos y puede presentarse como "GraphRAG visible y pedagógico".

### Gaps
- No encontré evaluaciones de detección de comunidades en grafos muy pequeños (<50 nodos) en contextos educativos.
- No verifiqué la calidad de la extracción de LLM en español para tipos pedagógicos (claim/evidence).
