# Analítica de aprendizaje con redes de discurso/conocimiento, y contexto regulatorio Perú/LatAm (referencia para conText)

Fecha de las notas: 30-oct-2026. Investigación de ~20 consultas web. Muchos hallazgos vienen de resúmenes de búsqueda o fuentes secundarias (estudios jurídicos, gremios); donde la fuente es secundaria se indica. Los textos oficiales de El Peruano (DS 016-2024-JUS, DS 115-2025-PCM) no se pudieron abrir directamente (403/404).

---

## A1. Epistemic Network Analysis (ENA) y Ordered Network Analysis (ONA): qué miden, herramientas, uso educativo

### Takeaway
ENA, de Shaffer y la "Quantitative Ethnography", mide la estructura de co-ocurrencias entre códigos del discurso, es decir qué ideas o habilidades conecta un estudiante y no solo cuántas veces las menciona. ONA añade orden y dirección. Es el antecedente académico más cercano y defendible para el "grafo de ideas" de conText. Su diferencia clave es que ENA trabaja sobre un **codebook teórico fijo**, mientras que conText extrae ideas libres. Además, ENA compara redes entre grupos o momentos.

### Cited Findings
- ENA identifica patrones en datos codificados y va más allá de la evaluación por frecuencias, porque examina la estructura de las conexiones (co-ocurrencias). Modela redes completas y permite comparar los modelos de red de forma cuantitativa y cualitativa. — [rENA en CRAN](https://cloud.r-project.org/web/packages/rENA/refman/rENA.html); [rENA DESCRIPTION](https://codymarquart.r-universe.dev/rENA/DESCRIPTION)
- Herramientas: webENA (herramienta web, con video y tutorial de Shaffer, Collier y Ruis) y rENA (paquete R mantenido por Cody Marquart, versión 0.3.1, GPL-3). rENA da más control que la versión web y se integra con otros análisis estadísticos. — [epistemicnetwork.org/resources](https://www.epistemicnetwork.org/resources/); [rENA CRAN](https://cran.r-universe.dev/api/packages/rENA)
- Origen: un proyecto NSF de Shaffer (terminado en 2012) desarrolló ENA para evaluar si los estudiantes **vinculan** habilidades y conocimientos, y no solo si dominan hechos aislados. — [WCER project 853](https://wcer.wisc.edu/About/Project/853)
- El libro *Quantitative Ethnography* (Shaffer) es el texto de referencia. Shaffer y Ruis publicaron un ejemplo trabajado de "theory-based learning analytics" en el Handbook of Learning Analytics de SoLAR, cap. 15. — [SoLAR chapter 15 bib](https://www.solaresearch.org/wp-content/uploads/2017/05/chapter15_bib.txt); [Springer, enseñanza de QE](https://link.springer.com/chapter/10.1007/978-3-031-47014-1_33)
- ONA (Tan, Ruis, Marquart, Cai, Knowles y Shaffer, ICQE 2022, pp. 101-116) es la contraparte dirigida de ENA. Considera el orden de los eventos y produce redes dirigidas y ponderadas. Por ejemplo, distingue "pregunta de proceso → respuesta breve" de "respuesta breve → pregunta de proceso". Recibió el premio al mejor paper estudiantil. Algunas fuentes lo citan como 2023. — [dblp](https://dblp.uni-trier.de/pid/10/5421.html); [UW–Madison noticia](https://edpsych.education.wisc.edu/tan-recieves-best-student-paper-award-at-international-conference/); [arXiv 2512.22747 (reanálisis con ONA)](https://arxiv.org/pdf/2512.22747)
- Directed ENA (dENA, 2021) modela a la vez la interdependencia y el orden en el trabajo grupal. — [Monash research](https://research.monash.edu/en/publications/directed-epistemic-network-analysis/)
- **ENA aplicado a diálogos con chatbots/tutores IA (2024-2025):**
  - Un preprint de arXiv (nov. 2024) usa ENA para comparar las interacciones de aprendices con un chatbot convencional y con uno de andamiaje, según su alfabetización en IA generativa (n≈67). — [arXiv 2411.15597](https://arxiv.org/pdf/2411.15597)
  - Un paper de ICCE 2025 combina codificación IRF con ENA. Encuentra que los diálogos de tutoría humanos superan a los simulados por IA en longitud de enunciados, preguntas y retroalimentación. — [ICCE 2025](https://library.apsce.net/index.php/ICCE/article/view/6066); [arXiv 2509.01914](https://arxiv.org/abs/2509.01914v1)
  - Un estudio preliminar usa ENA sobre un tutor LLM de andamiaje en matemáticas de primaria. — [CEUR-WS Vol-4248](https://ceur-ws.org/Vol-4248/short4.pdf)
  - Un trabajo combina NLP con ENA para estudiar la "knowledge integration" de estudiantes dentro de diálogos con IA. Pertenece a la línea del grupo de Linn. — [NSF PAR 10553647](https://par.nsf.gov/biblio/10553647-combining-natural-language-processing-epistemic-network-analysis-investigate-student-knowledge-integration-within-ai-dialog)

### Inferences
- Una forma de hacer defendibles las métricas de conText sería reportar co-ocurrencias de **categorías conceptuales** definidas por la rúbrica del curso (estilo ENA) además de ideas libres, y comparar la red del estudiante con la del material o la de un experto.
- conText ya etiqueta el origen de cada idea (material, estudiante, tutor), así que un análisis tipo ONA de quién introduce qué y en qué orden, por ejemplo "tutor pregunta → estudiante conecta", encaja con su diseño socrático.
- ENA exige un codebook validado y cálculos de confiabilidad entre codificadores. Si se usa un LLM para codificar, habría que validarlo contra codificación humana, que es la práctica habitual en QE.

### Gaps
- No verifiqué el estado actual de webENA (versión 2026) ni si ofrece API.
- No abrí el texto completo de los preprints 2024-2025; los tamaños muestrales vienen de resúmenes.

---

## A2. Knowledge Building / Knowledge Forum, Idea Thread Mapper, scoring de mapas conceptuales y métricas de red como indicadores de comprensión

### Takeaway
Hay tres tradiciones relevantes:
1. **Analítica de Knowledge Building** (Knowledge Forum, ITM, KBDeX): mide el progreso colectivo de ideas.
2. **Evaluación de mapas conceptuales** (Novak, scoring de proposiciones, HIMATT y AKOVIA de Ifenthaler y Pirnay-Dummer): compara la red del estudiante con un mapa experto.
3. **Scoring automático con LLM** (2024-2026): emergente, con resultados prometedores pero preliminares.

La lección común es que las métricas estructurales sueltas, como densidad o centralidad, valen poco sin **comparación semántica con una referencia** (experto o material).

### Cited Findings
- **Idea Thread Mapper (ITM)** es una herramienta de mapeo colectivo que traza y visualiza "hilos de ideas" en discusiones en línea prolongadas, y apoya el knowledge building dirigido por estudiantes (NSF 1441479). Referencia principal: Zhang, Tao, Chen, Sun, Judson y Naqvi (2018), *Journal of the Learning Sciences*, 27(3), 390-430. — [NSF STEM for All video](https://stemforall2019.videohall.com/presentations/1623); [dblp](https://dblp.uni-trier.de/pid/144/1628-4)
- **KBDeX** es una plataforma de análisis de redes sociales y de palabras para el discurso colaborativo. Se atribuye a Oshima, Oshima y Matsuzawa (2012) y se ha aplicado a notas de Knowledge Forum. Knowledge Forum también trae analíticas internas. — [ICCE/APSCE](https://library.apsce.net/index.php/ICCE/article/download/3648/3524/4612); [KBSI2018 Proceedings](https://ikit.org/KBSI2018/KBSI2018-Proceedings.pdf)
- Se han generado "Idea-Friend Maps" a partir del discurso en Knowledge Forum para andamiar la creación de conocimiento. — [KBSI 2020, Xueqi Feng](https://ikit.org/summerinstitute2020/wp-content/uploads/2021/03/181-KBSI-Xueqi-Feng.pdf)
- **HIMATT** (≈2008) es una suite web que obtiene texto y mapas conceptuales anotados y los compara con un modelo experto de referencia, con una representación previa del mismo estudiante o con la de otros aprendices. — [Springer ETR&D editorial](https://link.springer.com/article/10.1007/s11423-009-9127-8); [IADIS](https://www.iadisportal.org/digital-library/highly-integrated-model-assessment-technology-and-tools)
- **AKOVIA** (Ifenthaler, 2014, *Technology, Knowledge and Learning*) analiza representaciones en lenguaje natural y gráficas y produce **siete medidas cuantitativas de similitud**, que el autor reporta como probadas en confiabilidad y validez. — [Curtin espace](https://espace.curtin.edu.au/handle/20.500.11937/31696)
- Ifenthaler: "Scope of graphical indices in educational diagnostics" (2010), sobre índices de grafos en diagnóstico educativo. — [Curtin espace](https://espace.curtin.edu.au/handle/20.500.11937/14125?show=full)
- En mapas conceptuales de equipo, la **centralidad** resultó una medida útil de la estructura de conocimiento. — [Penn State](https://pure.psu.edu/en/publications/using-centrality-of-concept-maps-as-a-measure-of-problem-space-st/)
- Las herramientas abiertas de features de grafos (por ejemplo cookiemilk) calculan densidad y "holistic graph centrality" y comparan la estructura del estudiante con un mapa experto. — [cookiemilk tutorial](https://cookiemilk-tutorial.readthedocs.io/en/latest/Part%203%20Advanced%20tips.html)
- La centralidad de grado usa solo información local. Las medidas basadas en caminos, como betweenness, usan la estructura global. — [arXiv 2203.09219](https://arxiv.org/pdf/2203.09219)
- **Scoring automático con LLM:**
  - Maskur et al. (2026) usan RAG (BM25 o FAISS) con LLM pequeño y GPT para puntuar proposiciones de mapas de estudiantes. La recuperación semántica mejora la precisión, y los modelos grandes captan mejor las relaciones pero cuestan más cómputo. Señalan que la evaluación manual es subjetiva, inconsistente y poco escalable. — [Kinetik journal](https://kinetik.umm.ac.id/index.php/kinetik/article/view/2594)
  - Un paper de AERA 2026 pregunta si la IA puede aproximar la calificación experta de exactitud y completitud en mapas de 257 estudiantes, con base en la teoría de asimilación y el aprendizaje generativo. — [AERA 2026](https://convention2.allacademic.com/one/aera/aera26/online_program_direct_link/view_paper/2285612/)
  - Un estudio de 2025 en *Interactive Learning Environments* recogió las percepciones de 83 estudiantes de secundaria sobre mapas generados por ChatGPT frente a mapas de docentes. — [Uniroma3 PDF](https://iris.uniroma3.it/retrieve/76c3a282-177e-436f-9d87-42211dd0a281/A%20closer%20look%20at%20ChatGPT%20s%20role%20in%20concept%20map%20generation%20for%20education.pdf)
  - Antecedente pre-LLM: Maharjan y Rus (2019) puntuaron respuestas abiertas convirtiéndolas en mapas conceptuales. — [NSF PAR 10111321](https://par.nsf.gov/biblio/10111321); [Memphis](https://digitalcommons.memphis.edu/facpubs/3288)

### Inferences
- El scoring clásico de Novak cuenta proposiciones válidas, niveles jerárquicos y **cross-links** válidos. Puede adaptarse así: "ideas conectadas por el estudiante" se convierte en "**proposiciones válidas** del estudiante", evaluadas por un LLM contra el material, con el enfoque RAG de Maskur et al.
- La "idea más central" (centralidad de grado) es descriptiva. Su versión pedagógica sería "¿coincide la idea central del estudiante con el concepto nuclear del material?", es decir, similitud con la referencia al estilo de HIMATT y AKOVIA.
- Un "% de ideas aisladas" bajo no implica comprensión por sí solo. Puede venir de conexiones triviales hechas por el tutor. Conviene separar los enlaces por quien los creó (tracking de origen).

### Gaps
- No obtuve fuente primaria accesible del método de scoring de Novak y Gowin (1984) ni del relational scoring de McClure et al. (1999); lo descrito en las inferencias es conocimiento de fondo y debe verificarse.
- No encontré estudios que validen la densidad como predictor de aprendizaje en grafos generados automáticamente desde chat.

---

## A3. Evidencia sobre aprendizaje generativo, autoexplicación, ICAP y "learning by teaching"

### Takeaway
ICAP (Chi y Wylie, 2014) predice que el aprendizaje aumenta de pasivo a activo, constructivo e interactivo. Que el estudiante genere y explique conexiones es una actividad "constructiva" o "interactiva", lo que da sustento teórico a contar las conexiones **generadas por el estudiante**. Pero la evidencia empírica tiene condiciones de borde. El "protégé effect" (Betty's Brain) respalda enseñar a un agente construyendo un mapa causal.

### Cited Findings
- Chi y Wylie (2014), "The ICAP Framework: Linking Cognitive Engagement to Active Learning Outcomes", *Educational Psychologist* 49(4), 219-243. Clasifica las actividades por la conducta observable y predice I > C > A > P; los niveles altos implican comprensión más profunda. — [PDF ASU](https://chilab.asu.edu/sites/default/files/lcl/chiwylie2014icap_2.pdf); [ASU Pure](https://asu.elsevierpure.com/en/publications/the-icap-framework-linking-cognitive-engagement-to-active-learnin)
- Procesos asociados en ICAP:
  - Pasivo: almacenar, con resultado de recuerdo.
  - Activo: integrar, con resultado de aplicación.
  - Constructivo: inferir y generar, con resultado de transferencia.
  — [PMC tabla](https://pmc.ncbi.nlm.nih.gov/articles/PMC5998321/table/T2); [ASU LTH guide](https://lth.engineering.asu.edu/reference-guide/icap-framework/)
- La evidencia es mixta.
  - **A favor:** en aulas de ingeniería universitaria, las actividades constructivas superaron a las activas y pasivas. — [AERA Open 2017](https://journals.sagepub.com/doi/10.1177/2332858417708567)
  - **En contra:** en un test experimental, contra lo que predice ICAP, el grupo pasivo rindió mejor, seguido de activo, interactivo y constructivo. Los autores lo atribuyen a la complejidad de la tarea y a la preparación de los aprendices. — [TOJET v23i2](https://www.tojet.net/articles/v23i2/2322.pdf)
- **Betty's Brain** (Vanderbilt, Biswas; Stanford AAA Lab, Schwartz): los estudiantes enseñan a un agente construyendo un mapa causal con enlaces de aumento o disminución. La corrección se juzga contra un mapa experto oculto, y un mentor (Mr. Davis) da retroalimentación. — [Springer IJAIED 2016](https://link.springer.com/10.1007/s40593-015-0057-9); [Vanderbilt PDF](https://eecs.vuse.vanderbilt.edu/People/bobbyb/pubs/idi04.pdf)
- "Protégé effect": quienes construyeron mapas para enseñar a Betty se esforzaron más que quienes aprendían para sí mismos, con casi el doble de tiempo leyendo y más tiempo revisando sus mapas (fuente de prensa sobre los estudios de Stanford). — [EdWeek 2010](https://www.edweek.org/ew/articles/2010/10/28/10avatar.h30.html); [Columbia/Schwartz PDF](https://www.tc.columbia.edu/faculty/so2269/faculty-profile/files/Technology_MetaCognition.pdf)

### Inferences
- conText ya distingue el origen de cada idea. Eso le permite operacionalizar ICAP: las ideas y conexiones del **estudiante** son conducta constructiva, y las que el estudiante construye a partir de una pregunta del tutor son interactivas. Una métrica tipo "proporción de conexiones generadas por el estudiante" tiene respaldo teórico, a diferencia del simple número de ideas.
- Al estilo de Betty's Brain, un modo "enséñale al tutor" con un mapa de referencia oculto (derivado del material) daría una medida de exactitud además de estructura.

### Gaps
- No obtuve tamaños de efecto primarios del protégé effect, ni metaanálisis recientes (2023-2026) de autoexplicación o de learning-by-teaching. La investigación sobre autoexplicación de Chi (1989/1994) y el metaanálisis de Bisra et al. (2018) quedan como pendientes de verificar.

---

## A4. ¿Qué métricas serían mejores que las actuales de conText para indicar aprendizaje profundo?

### Takeaway
Las métricas actuales (ideas conectadas, número de enlaces, idea más central por grado y % de ideas aisladas) son puramente estructurales y no tienen referencia. La literatura sugiere pasar a métricas de **(a) validez semántica, (b) similitud con una referencia, (c) autoría y agencia, y (d) cambio en el tiempo**.

### Cited Findings (bases)
- ENA y QE: valor de las **conexiones** frente a las frecuencias, y comparación entre redes y momentos. — [rENA](https://cloud.r-project.org/web/packages/rENA/refman/rENA.html)
- HIMATT y AKOVIA: comparar con el modelo experto o con una representación previa, mediante siete medidas de similitud. — [Springer](https://link.springer.com/article/10.1007/s11423-009-9127-8); [Curtin](https://espace.curtin.edu.au/handle/20.500.11937/31696)
- Scoring de proposiciones con RAG y LLM. — [Kinetik](https://kinetik.umm.ac.id/index.php/kinetik/article/view/2594)
- ONA para el orden y la dirección de los turnos. — [dblp ICQE 2022](https://dblp.uni-trier.de/pid/10/5421.html)
- ICAP para distinguir lo generado por el estudiante. — [Chi y Wylie 2014](https://chilab.asu.edu/sites/default/files/lcl/chiwylie2014icap_2.pdf)
- Comparación con mapa experto oculto (Betty's Brain). — [Springer IJAIED](https://link.springer.com/10.1007/s40593-015-0057-9)

### Inferences (propuesta de métricas; elaboración propia sobre las fuentes)
1. **Proposiciones válidas del estudiante.** Contar enlaces creados por el estudiante que un LLM, anclado al material con RAG, juzga correctos, en lugar de "ideas conectadas". Es el scoring de Novak y Maskur automatizado.
2. **Cobertura del núcleo del material.** Porcentaje de los conceptos clave del material (los de mayor centralidad en el grafo del material) que el estudiante integró con al menos una proposición propia. Es similitud con la referencia, al estilo de HIMATT y AKOVIA.
3. **Cross-links entre subtemas.** Enlaces del estudiante que unen comunidades o clústeres distintos del grafo (betweenness, puentes entre módulos). Se corresponden con los cross-links de Novak y reflejan integración, no solo acumulación.
4. **Agencia (ratio ICAP).** Proporción de conexiones iniciadas por el estudiante frente a las sugeridas por el tutor, y patrón dirigido tutor-pregunta → estudiante-conecta (ONA).
5. **Trayectoria.** Cambio en las métricas anteriores entre el inicio y el final de la sesión, o entre sesiones (pre/post, como HIMATT con una representación previa).
6. **Conservar el % de aisladas y la idea central solo como indicadores descriptivos o de UX,** no como evidencia de aprendizaje.
- Validar con un estudio pequeño: correlacionar las métricas con un post-test o con la calificación de un docente sobre el mapa. Es la práctica de AKOVIA y del paper de AERA 2026.

### Gaps
- No hay estudios que validen estas métricas compuestas en tutoría socrática con LLM y grafos autogenerados; esa validación sería una contribución original.

---

## B1. Perú: Ley 29733, nuevo reglamento (DS 016-2024-JUS), Ley 31814 y su reglamento (DS 115-2025-PCM), lineamientos de MINEDU

### Takeaway
- **Datos de menores:** el nuevo reglamento de la Ley 29733 (DS 016-2024-JUS, vigente desde el 30-mar-2025, según fuentes secundarias) exige el consentimiento de padres o tutores para tratar datos de menores. Los adolescentes **de 14 a 18 años pueden consentir por sí mismos** (con matices por confirmar en el texto oficial). También prohíbe recabar de un menor datos sobre su familia.
- **IA:** Perú tiene un reglamento general de IA (DS 115-2025-PCM, publicado el 9-sep-2025, vigente desde el **22-ene-2026**), con tres categorías de riesgo e implementación gradual por sector.
- **MINEDU:** no encontré lineamientos oficiales sobre IA generativa para estudiantes. Solo hay capacitaciones a docentes y talleres.

### Cited Findings
- DS 016-2024-JUS, art. 27: el tratamiento de datos de menores requiere el consentimiento de quienes ejercen la patria potestad o la tutela. — [COMEX Perú, carta nov-2024](https://www.comexperu.org.pe/upload/articles/proyectos-de-ley/carta-mujer_imagen-y-datos-personales-de-los-menores-de-edad-en-linea_nov2024.pdf); [COMEX feb-2025](https://comexperu.org.pe/upload/articles/proyectos-de-ley/carta-mujer_menores-de-edad-digital_feb2025-1.pdf)
- Art. 28: los adolescentes de 14 a 18 años pueden otorgar su consentimiento; por debajo de esa edad consienten los padres o tutores. La cita en COMEX está truncada, así que el alcance exacto debe verificarse. — [modelo.pe](https://modelo.pe/carta-de-consentimiento-informado-para-tratamiento-de-datos-personales-de-menores-de-edad/); [COMEX](https://comexperu.org.pe/upload/articles/proyectos-de-ley/carta-mujer_menores-de-edad-digital_feb2025-1.pdf)
- Art. 29: prohíbe obtener de un menor datos que permitan conocer información sobre otros miembros de su familia. — [COMEX](https://comexperu.org.pe/upload/articles/proyectos-de-ley/carta-mujer_menores-de-edad-digital_feb2025-1.pdf)
- Vigencia del reglamento: 30 de marzo de 2025 (fuente secundaria). — [modelo.pe](https://modelo.pe/carta-de-consentimiento-informado-para-tratamiento-de-datos-personales-de-menores-de-edad/)
- El Proyecto de Ley 8930/2024-CR propone nueva regulación de datos digitales de menores. COMEX (feb. 2025) sostiene que la normativa vigente ya los regula con detalle. No confirmé su estado en 2026. — [COMEX](https://comexperu.org.pe/upload/articles/proyectos-de-ley/carta-mujer_menores-de-edad-digital_feb2025-1.pdf)
- **Ley 31814 y DS 115-2025-PCM:**
  - El decreto se publicó en El Peruano el 9-sep-2025 y entra en vigencia el 22-ene-2026. — [EY Perú](https://www.ey.com/es_pe/technical/tax-alert/reglamento-ley-promueve-uso-inteligencia-artificial); [Cuatrecasas](https://www.cuatrecasas.com/es/latam/tecnologia-medios-digitales/art/que-establece-el-reglamento-de-la-ley-de-inteligencia-artificial); [El Peruano](https://busquedas.elperuano.pe/dispositivo/NL/2436426-1)
  - Su cumplimiento obligatorio avanza de forma gradual por sectores. — [Infobae](https://www.infobae.com/peru/2025/09/10/reglamento-de-ia-en-peru-claves-para-entender-la-ley-que-promueve-el-uso-de-la-inteligencia-artificial-para-el-desarrollo/); [Garrigues](https://www.garrigues.com/es_ES/noticia/peru-aprueba-reglamento-ley-promueve-uso-inteligencia-artificial-favor-desarrollo-economico)
  - Principios: no discriminación, privacidad de datos personales, derechos fundamentales, derecho de autor, seguridad, supervisión humana, transparencia, rendición de cuentas y desarrollo ético. — [Cuatrecasas](https://www.cuatrecasas.com/es/latam/tecnologia-medios-digitales/art/que-establece-el-reglamento-de-la-ley-de-inteligencia-artificial)
  - Establece **tres categorías de riesgo**. — [Cuatrecasas](https://www.cuatrecasas.com/es/latam/tecnologia-medios-digitales/art/que-establece-el-reglamento-de-la-ley-de-inteligencia-artificial)
  - Según CMS, Perú es el primer país de América Latina con un reglamento general de IA. — [CMS](https://cms.law/es/per/publication/se-aprueba-el-reglamento-de-la-ley-que-promueve-el-uso-de-la-inteligencia-artificial-ia)
  - Lo refrenda, entre otros, el Ministro de Educación. — [lpderecho](https://lpderecho.pe/reglamento-ley-promueve-uso-inteligencia-artificial-desarrollo-economico-pais-decreto-supremo-115-2025-pcm/); [Telesemana](https://www.telesemana.com/blog/2025/09/09/peru-reglamento-la-ley-que-promueve-el-uso-de-la-inteligencia-artificial-con-foco-en-el-desarrollo-economico-y-social/)
  - El borrador de 2024 incluía un principio de sensibilización y educación en IA (alfabetización digital). — [Proyecto de reglamento, LPDerecho 2024](https://img.lpderecho.pe/wp-content/uploads/2024/05/Proyecto-reglamento-Inteligencia-artificial-LPDerecho.pdf)
- El Proyecto de Ley 13553/2025-CR modificaría el art. 4 de la Ley 31814 para una "implementación progresiva", y MINEDU emitió opinión. No confirmé si se aprobó. — [api.congreso.gob.pe](https://api.congreso.gob.pe/spley-portal-service/archivo/Mzg5MzM2/pdf)
- **MINEDU:**
  - Oficio múltiple 00658-2024 sobre la réplica de talleres de integración de IA en el ámbito educativo. — [gob.pe PDF](https://cdn.www.gob.pe/uploads/document/file/7190725/6160580-oficio_multiple-00658-2024-minedu-vmgi-drelm-ugel05-dir-agebre.pdf)
  - Una comunidad de expertos recomendó que MINEDU elabore una guía de habilidades en IA de 1.º de primaria a 5.º de secundaria y cursos para certificar a docentes. Es una recomendación, no una norma. — [MetaRed pronunciamiento](https://metared.org/content/dam/metared/pdf/pe/Pronunciamiento%20de%20la%20Comunidad%20de%20Expertos%20en%20Inteligencia%20Artificial%20para%20la%20Educaci%C3%B3n.pdf)
  - MINEDU capacitó a más de 6,000 directivos en el método "El Director Libre" (IA para reducir carga administrativa) desde 2025. — [Banco Mundial, jul-2026](https://www.bancomundial.org/es/results/2026/07/02/artificial-intelligence-in-action-in-latin-america-s-schools-evidence-from-peru)

### Inferences
- conText no tiene cuentas y almacena datos anónimos por 7 días, lo que reduce el riesgo. Aun así, el **texto conversado y el material subido** pueden contener datos personales del menor o de su familia (art. 29). Conviene:
  - un aviso de no ingresar datos personales;
  - un filtro o redacción automática de PII;
  - una política de privacidad en lenguaje adolescente;
  - si se usa en aulas con menores de 14 años, consentimiento de padres gestionado por la escuela.
- El reglamento de IA exige transparencia y supervisión humana. Ayudaría informar claramente que el interlocutor es una IA y que las métricas no son una calificación. Si se usaran para evaluar estudiantes, podrían caer en una categoría de riesgo mayor; esto es por confirmar en el texto oficial.

### Gaps
- No pude abrir los textos oficiales en El Peruano (403/404). No verifiqué la redacción literal de los arts. 27-29 del DS 016-2024-JUS, ni qué usos educativos clasifica el DS 115-2025-PCM como de "riesgo alto" o "inaceptable", ni los plazos de adecuación del sector educación y de las MYPE.
- No encontré lineamientos oficiales de MINEDU sobre IA generativa para estudiantes de secundaria a oct-2026. La nota "MINEDU capacita a más de 87,000 docentes en IA" solo apareció como título en un listado y no la verifiqué.
- No verifiqué si se aprobaron el PL 8930/2024-CR (menores digitales) ni el PL 13553/2025-CR.

---

## B2. LatAm: programas y financiamiento públicos (BID, Banco Mundial, CAF) para IA en educación 2024-2026

### Takeaway
El caso más relevante para Perú es el programa del **Banco Mundial** (desde 2025) que lleva IA a escuelas públicas peruanas, con un tutor IA de matemáticas y un RCT en curso, en colaboración con edtechs (uDocz) y empresas de IA (Anthropic, Microsoft). El **BID** tiene la convocatoria "IA Presente" (con Ceibal y Socialab, 2025), que mapea y asesora pero no financia directamente. No encontré información de CAF.

### Cited Findings
- **Banco Mundial en Perú** (página de jul-2026):
  - El programa empezó en 2025 y parte de la premisa de que la IA solo mejora la educación si llega a todo el sistema y va acompañada de apoyo humano.
  - "Eligiendo Mi Camino" combina un tutor IA de matemáticas con un orientador apoyado por IA. Llega a unos 4,500 estudiantes de 5.º de secundaria en 85 escuelas públicas de Lima desde marzo de 2026.
  - Solo el 13% de los estudiantes de secundaria alcanza el nivel esperado en matemáticas.
  - Un RCT en 390 escuelas de Lima mostró que una capacitación breve aumentó el uso activo de IA por docentes, y mejoró su redacción de prompts y su evaluación crítica de las respuestas.
  - La observación de aula con IA dio resultados comparables a la de observadores humanos.
  - Hay otro RCT en curso sobre cómo diseñar tutores IA que construyan habilidades duraderas.
  - Colaboradores: uDocz, Anthropic y Microsoft.
  - Todavía no hay resultados de aprendizaje ni tamaños de efecto. Hay iniciativas similares en Ecuador y El Salvador.
  — [Banco Mundial](https://www.bancomundial.org/es/results/2026/07/02/artificial-intelligence-in-action-in-latin-america-s-schools-evidence-from-peru)
- **BID "IA Presente"** (convocatoria de 2025, con Ceibal y Socialab): busca soluciones de IA en educación desarrolladas en ALC para mapearlas, visibilizarlas y asesorarlas técnicamente. Participaron startups edtech, OSC, academia y empresas. — [BID noticia](https://www.iadb.org/es/noticias/bid-lanza-convocatoria-para-identificar-soluciones-de-inteligencia-artificial-que-transformen-la); [DPL News](https://dplnews.com/bid-lanza-convocatoria-para-identificar-soluciones-de-inteligencia-artificial-que-transformen-la-educacion/); [Bases IA Presente](https://dlfkybwubjirgkydpnhr.supabase.co/storage/v1/object/public/policy-document//Bases%20IA%20PRESENTE%20ESP.pdf)
- El BID publicó el informe "IA presente: soluciones reales con IA para los desafíos educativos de ALC". Según el catálogo es de oct-2026, fecha por verificar. — [IDB Publications](https://publications.iadb.org/en/node/38453); [PDF](https://publications.iadb.org/publications/spanish/document/IA-presente-soluciones-reales-con-IA-para-los-desafios-educativos-de-America-Latina-y-el-Caribe.pdf)
- BID y UNESCO lanzaron una nueva edición del Observatorio de IA, y el BID tiene un mapeo del ecosistema de IA en 12 países de ALC. — [BID-UNESCO](https://www.iadb.org/es/noticias/bid-y-unesco-lanzan-nueva-edicion-del-observatorio-de-inteligencia-artificial); [Mapeo 12 países](https://www.iadb.org/es/noticias/bid-lanza-mapeo-sobre-ecosistema-de-ia-en-12-paises-de-america-latina-y-el-caribe)
- Alternativas no multilaterales:
  - TecPrize (Tec de Monterrey, IFE) premia proyectos edtech con IA. — [Corresponsables](https://www.corresponsables.com/mx/actualidad/tec-de-monterrey-reconocera-iniciativas-en-edtech-que-transformen-el-sector-educativo/)
  - En Perú, MentorIA (Kodea y Empresarios por la Educación) forma facilitadores para capacitar a 1,500 docentes. — [RPP](https://rpp.pe/campanas/publirreportaje/kodea-y-empresarios-por-la-educacion-impulsan-la-educacion-digital-con-inteligencia-artificial-noticia-1622984)

### Inferences
- Para AIdeaText, la vía más alineada parece ser vincularse al ecosistema del programa del Banco Mundial y MINEDU en Perú, que ya trabaja con edtechs locales y busca diseños de tutor IA que generen aprendizaje duradero. También cabe postular a futuras ediciones de IA Presente del BID, sobre todo por visibilidad y asesoría.
- Un piloto con métricas validadas (sección A4) y un diseño cuasi-experimental fortalecería cualquier postulación, dado que estos programas enfatizan la evidencia mediante RCTs.

### Gaps
- No encontré programas de CAF de IA en educación 2024-2026, ni fondos con montos o fechas de cierre vigentes a oct-2026 para startups edtech. Tampoco busqué ProInnóvate o Concytec (Perú), que podrían ser relevantes.
- No verifiqué si el Banco Mundial abrirá convocatorias para proveedores edtech, ni en qué condiciones colabora con uDocz.
