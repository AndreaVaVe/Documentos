1 Introducción
En los últimos años, los sistemas de emergencias públicos han experimentado una creciente presión para mejorar su capacidad de respuesta frente a una demanda cada vez más compleja y heterogénea. El volumen de datos operativos generados por estos sistemas —registros de llamadas, tipificaciones de incidentes, ubicaciones geográficas, tiempos de atención y despacho— constituye un activo de enorme potencial analítico que, en la mayoría de los casos, permanece subutilizado debido a la ausencia de infraestructuras de datos adecuadas y de herramientas de analítica integradas.
En este contexto, el Centro de Monitoreo y Sistema de Emergencias 911 (CMySE 911) de la ciudad de San Salvador de Jujuy no es una excepción. El centro opera de manera continua, procesando aproximadamente 15.000 llamadas mensuales provenientes de un área de cobertura que incluye San Salvador de Jujuy, sectores de Palpalá, Alto Comedero, Villa Jardín de Reyes, San Pablo de Reyes y Yala. A pesar de la relevancia estratégica de esta información, su explotación se realiza actualmente de forma manual, mediante planillas de cálculo, sin un área formal de estadística ni un sistema integrado de análisis que permita monitorear el desempeño del servicio, identificar patrones de demanda o anticipar necesidades operativas.
Esta situación genera consecuencias concretas: demoras en la producción de reportes para las autoridades, riesgo de errores en la consolidación de datos, imposibilidad de detectar oportunamente cuellos de botella en la atención —como tiempos de respuesta excesivos o saturación de operadores en determinadas franjas horarias— y una subutilización sistemática del potencial de los datos históricos acumulados por el sistema a lo largo de los años.
Frente a esta problemática, la ciencia de datos ofrece herramientas concretas para transformar registros operativos en información accionable: la construcción de un Data Warehouse permite consolidar y estructurar datos históricos para su análisis multidimensional; los modelos de series temporales posibilitan anticipar volúmenes de demanda y tipos de incidentes; y las técnicas de análisis geoespacial facilitan la identificación de zonas críticas y patrones espacio-temporales. La integración de estas capacidades en un sistema analítico unificado puede proporcionar a los responsables del CMySE 911 una base objetiva y sistemática para optimizar la gestión de recursos, planificar despliegues operativos y mejorar la respuesta a la ciudadanía.
El presente trabajo propone el diseño, implementación y evaluación de un sistema integral de analítica de datos para el CMySE 911, que combine una arquitectura de Data Warehouse basada en la metodología Hefesto, modelos predictivos de series temporales (ARIMA) y análisis geoespacial mediante Python y QGIS, articulados a través de tableros de control interactivos orientados a la toma de decisiones operativas y estratégicas.
1.1 Objetivos 
1.1.1 Objetivo General 
Diseñar, implementar y evaluar un sistema integral de analítica de datos para el CMySE 911 de San Salvador de Jujuy, que integre una arquitectura de Data Warehouse, modelos predictivos de series temporales y análisis geoespacial, con el fin de transformar los datos operativos históricos del centro en información accionable para la toma de decisiones y la optimización del servicio de emergencias.
1.1.2 Objetivos Específicos 
•	Relevar y analizar los procesos actuales de registro, gestión y calidad de los datos operativos del CMySE 911, identificando limitaciones, inconsistencias y requerimientos para el diseño de un sistema analítico basado en información del período 2018–2025.
•	Diseñar e implementar una arquitectura de Data Warehouse utilizando la metodología Hefesto, integrando y transformando los datos históricos de incidentes para su explotación mediante análisis multidimensional y consultas eficientes.
•	Desarrollar y evaluar modelos de series temporales (ARIMA y variantes) para la predicción del volumen y tipo de incidentes, comparando el desempeño de los modelos mediante métricas estadísticas (RMSE, MAE, MAPE) y seleccionando aquel que presente el mejor ajuste.
•	Aplicar técnicas de análisis geoespacial (mapas de calor, densidad de kernel, clustering espacial) mediante Python y QGIS, con el fin de identificar zonas críticas y patrones espacio-temporales de concentración de incidentes.
•	Diseñar e implementar visualizaciones interactivas y tableros de control que permitan explorar la información, interpretar los resultados de los análisis y generar reportes automatizados para la gestión operativa y estratégica del CMySE 911.
•	Evaluar la utilidad, usabilidad e impacto del sistema analítico desarrollado mediante entrevistas y encuestas a personal del CMySE 911, determinando su contribución al fortalecimiento de la toma de decisiones y la optimización del servicio.
1.2 Antecedentes
El presente apartado reúne y analiza antecedentes relevantes vinculados al uso de tecnologías de analítica de datos, sistemas de información y metodologías de inteligencia operativa aplicadas a servicios de emergencia. Se revisan experiencias internacionales, regionales y locales, así como trabajos de grado y posgrado relacionados, con el fin de identificar enfoques existentes, buenas prácticas y vacíos de investigación que fundamentan la pertinencia y el aporte del presente estudio.
Antecedentes internacionales de posgrado en analítica de emergencias y seguridad ciudadana
En el ámbito latinoamericano se observan diversos trabajos de posgrado que aplican técnicas de ciencia de datos, minería de datos y análisis espacio-temporal sobre registros de emergencias o criminalidad. Estos estudios muestran la viabilidad de explotar grandes volúmenes de datos operativos para apoyar la gestión de la seguridad ciudadana, aunque con enfoques y alcances distintos al propuesto en esta tesis.
Un antecedente central es la tesis de Almeida Murillo (2025), "Detección de patrones de tiempo y espacio en el servicio integrado de seguridad ECU 911 para identificar las parroquias de alta peligrosidad en la ciudad de Quevedo", presentada en la Universidad Nacional de Chimborazo (Ecuador). El autor utiliza registros de incidentes del ECU 911 correspondientes al año 2023 para identificar patrones espacio-temporales de riesgo, combinando análisis descriptivo y exploratorio de los incidentes por parroquia, circuito y subcircuito; técnicas de análisis geoespacial (mapas de calor, SIG); y algoritmos de clustering (KMeans, DBSCAN, HDBSCAN) para agrupar incidentes y detectar zonas críticas, complementados con análisis temporal (horas pico, variación mensual). Los resultados permiten ubicar parroquias de alta peligrosidad y proponer lineamientos para la planificación preventiva. No obstante, el estudio se concentra en el análisis de patrones sobre datos históricos, sin diseñar una arquitectura de datos institucional ni un Data Warehouse formal que soporte de manera integrada la toma de decisiones en el tiempo.
En la misma línea, Vergara Loor (2024), en su tesis de maestría "Patrones en Incidentes de Emergencia en Ecuador a través del Sistema ECU 911" (Universidad Técnica Estatal de Quevedo, Maestría en Ciencia de Datos), analiza el conjunto de datos del ECU 911 a escala nacional mediante la aplicación de reglas de asociación para la detección de patrones temporales y geográficos en incidentes de emergencia. Los resultados son presentados a través de visualizaciones interactivas en Tableau. Si bien el trabajo demuestra la potencia de las técnicas de minería de datos aplicadas a sistemas de emergencias y la utilidad de las herramientas de visualización para la toma de decisiones, su alcance no incluye el diseño de un Data Warehouse institucional, modelos predictivos de series temporales ni análisis geoespacial avanzado, elementos que el presente trabajo incorpora de manera integrada.
Además de estas dos tesis directamente referenciadas, el trabajo de Almeida (2025) recupera otros antecedentes latinoamericanos que complementan el panorama internacional:
•	Chacón Encalada (2021) analiza el Sistema Integrado SIS ECU 911 Ibarra aplicando técnicas de minería de datos y análisis geoespacial para detectar patrones en incidentes de emergencia y reducir costos técnico-económicos. Se demuestra que la minería de datos mejora la gestión de emergencias, aunque el estudio se apoya principalmente en datos históricos y no en una plataforma integrada de datos.
•	Martínez Narváez (2023 approx.) desarrolla un análisis geoespacial de feminicidios y homicidios contra mujeres en Torreón (México), identificando puntos calientes delictivos y destacando la utilidad de SIG y técnicas de minería de datos para orientar estrategias de intervención focalizada.
•	López e Inga (2022) comparan denuncias judiciales y reportes de emergencia en el centro histórico de Cuenca (Ecuador), mostrando discrepancias relevantes entre ambas fuentes y subrayando la importancia de mejorar la integración y calidad de los registros para la toma de decisiones.
•	Villamar Barros (2023) desarrolla un sistema de visualización de datos espacio-temporales para la predicción de eventos de emergencia en Ecuador, enfatizando el valor de dashboards y mapas interactivos para identificar patrones de riesgo y apoyar la asignación de recursos.
•	Valdez Huaraca (2021) realiza un análisis estadístico espacio-temporal de delitos en la provincia de Chimborazo (Ecuador), utilizando técnicas estadísticas y geoespaciales para comprender la dinámica de la inseguridad ciudadana en el período 2015–2019.

En conjunto, estos antecedentes de posgrado conforman un corpus internacional latinoamericano que demuestra la pertinencia del análisis espacio-temporal, los SIG y los algoritmos de clustering en la gestión de la seguridad ciudadana, así como el potencial de los modelos de visualización avanzada para apoyar la toma de decisiones. Al mismo tiempo, muestran que el foco suele estar en estudios de caso específicos centrados en la exploración de patrones, con menor desarrollo de una infraestructura de datos robusta que perdure como activo institucional.
Antecedentes locales
En el ámbito local, el artículo "Text Mining aplicado a la Base de Datos del Servicio del 911 de la Provincia de Jujuy" (Medrano, Tejerina, Castillo y Rodríguez, 2019) aplica técnicas de minería de texto sobre el campo de descripción libre de los registros del 911 provincial, señalando que dicho campo —raramente considerado en los informes convencionales— constituye una fuente valiosa de información no estructurada para identificar patrones en la atención de emergencias. Este antecedente local refuerza la pertinencia del caso de estudio seleccionado y la existencia de una tradición de investigación sobre los datos del 911 de Jujuy.
Asimismo, el trabajo final de grado de la autora (Velázquez, 2024) constituyó el punto de partida de la presente investigación: en él se desarrolló un módulo de análisis descriptivo y visualización sobre datos reales del CMySE 911, implementando dashboards en Power BI y un prototipo de cubo multidimensional mediante la metodología Hefesto. La presente tesis de maestría extiende y profundiza ese trabajo, incorporando una arquitectura de Data Warehouse completa, modelos predictivos de series temporales y análisis geoespacial en un sistema analítico integrado.

1.3 Justificación
El crecimiento sostenido de los incidentes de emergencia y la creciente complejidad de los entornos urbanos demandan herramientas más avanzadas para gestionar información y soportar la toma de decisiones en organismos de seguridad y respuesta inmediata. El Sistema de Emergencias 911 constituye el punto crucial de atención y coordinación ante situaciones críticas, generando grandes volúmenes de datos operativos provenientes de llamadas, ubicaciones geográficas, tipologías de incidentes y tiempos de respuesta. Sin embargo, la explotación de esta información suele ser limitada debido a registros heterogéneos, falta de integración entre fuentes, escasa capacidad analítica y ausencia de modelos predictivos formales.
En este contexto, la analítica avanzada de datos ofrece oportunidades sustanciales para mejorar la eficiencia y efectividad del servicio. La construcción de un Data Warehouse permite consolidar, depurar y estructurar datos históricos para su análisis multidimensional; los modelos de predicción basados en series temporales posibilitan anticipar demandas futuras; y las técnicas de análisis geoespacial facilitan la identificación de zonas críticas y patrones espacio-temporales de incidentes. Integrar estas capacidades en un sistema analítico unificado proporciona a los responsables del 911 una base objetiva para optimizar recursos, planificar despliegues operativos y mejorar la respuesta a la ciudadanía.
Pese a su relevancia estratégica, en la provincia de Jujuy no existen soluciones analíticas integrales aplicadas al Sistema de Emergencias 911 que combinen infraestructura de datos, predicción de demanda, análisis espacial y visualización interactiva en un mismo proyecto. La presente tesis busca cubrir este vacío, ofreciendo un aporte técnico concreto mediante el diseño e implementación de un sistema integral de analítica de datos orientado a fortalecer la toma de decisiones operativas y estratégicas del CMySE 911.
La investigación se justifica desde tres perspectivas complementarias. Desde una perspectiva social, contribuye a la mejora de un servicio público esencial para la seguridad ciudadana. Desde una perspectiva institucional, proporciona al CMySE 911 una herramienta de gestión basada en evidencia que reduce la dependencia de procesos manuales y mejora la trazabilidad de la información. Y desde una perspectiva científica, integra metodologías de ingeniería de datos (Hefesto), análisis predictivo (modelos ARIMA y variantes) y sistemas de información geográfica (QGIS, Python) en un caso de aplicación real del sector público, contribuyendo al campo de la ciencia de datos aplicada a la gestión de emergencias en contextos latinoamericanos con recursos institucionales limitados. De este modo, el proyecto propone una solución tecnológicamente viable, escalable y alineada con las necesidades actuales de modernización del sistema de seguridad y emergencia de la provincia.

1.4 Alcance del Trabajo
El presente trabajo de tesis se orienta al diseño, desarrollo e implementación de un sistema integral de analítica de datos aplicado al Centro de Monitoreo y Sistema de Emergencias 911 (CMySE 911) de la ciudad de San Salvador de Jujuy, a partir de la explotación de datos históricos de incidentes correspondientes al período 2018–2025, en la medida en que dichos datos se encuentren disponibles y con la calidad mínima requerida para su análisis.
Estará comprendido dentro del alcance de este trabajo:
•	Relevar y analizar los procesos actuales de registro y gestión de incidentes del CMySE 911, así como las estructuras y fuentes de datos asociadas, con el fin de identificar sus principales características, limitaciones y requerimientos para el diseño del sistema analítico.
•	Diseñar el modelo conceptual y lógico de un Data Warehouse temático de incidentes de emergencia, definiendo hechos, dimensiones, indicadores y relaciones necesarias para soportar el análisis multidimensional de la información operativa.
•	Implementar la arquitectura del Data Warehouse conforme al modelo lógico definido, integrando y transformando los datos históricos disponibles en un repositorio único que permita la construcción de consultas, cubos y vistas analíticas.
•	Desarrollar y evaluar modelos de series temporales para la predicción del volumen y tipo de incidentes en horizontes de corto plazo, utilizando como insumo los datos consolidados en el Data Warehouse y métricas estándar de evaluación.
•	Aplicar técnicas de análisis geoespacial sobre los datos de ubicación de incidentes, con el objetivo de identificar zonas críticas y patrones espacio-temporales de concentración de eventos.
•	Diseñar e implementar tableros de control y visualizaciones interactivas que integren los principales indicadores descriptivos, resultados de los modelos predictivos y hallazgos geoespaciales, orientados a la consulta y exploración por parte de responsables operativos y de planificación del CMySE 911.
•	Realizar una evaluación preliminar del sistema desarrollado a través de entrevistas y/o encuestas a personal del CMySE 911, con el fin de relevar percepciones sobre utilidad, usabilidad y potencial impacto en la toma de decisiones.
El alcance de este trabajo se limita al desarrollo de un prototipo funcional de sistema de analítica de datos en un entorno de prueba o laboratorio. Quedan expresamente fuera del alcance:
•	El registro operativo de incidencias, la actualización y mantenimiento continuo de la base de datos transaccional del CMySE 911.
•	La adquisición de licencias de software, infraestructura de hardware o adecuaciones de los sistemas productivos existentes.
•	La definición, modificación o implementación de protocolos de atención de emergencias o procedimientos operativos internos.
•	El diseño e implementación de modelos avanzados de optimización de recursos y ruteo de móviles, que se consideran posibles líneas de trabajo futuras.

Para la finalidad de este trabajo se analizarán prioritariamente datos cuantitativos producidos por la institución, excluyendo datos personales de los usuarios, en cumplimiento de la Ley de Protección de Datos Personales y normativa vigente. Tanto los análisis descriptivos como los modelos predictivos y geoespaciales estarán condicionados por la cantidad, calidad y completitud de los datos provistos por el CMySE 911, reconociéndose que, a mayor volumen y mejor calidad de datos, mayor será la precisión y confiabilidad de los resultados obtenidos..
2. MARCO TEÓRICO
2.1. Sistemas de emergencias 911 y gestión de la seguridad ciudadana
En las últimas décadas, los sistemas de respuesta a emergencias se han consolidado como un componente estratégico de la gestión de la seguridad ciudadana y la protección civil. Estos sistemas constituyen el punto de contacto inmediato entre la población y las instituciones responsables de atender incidentes que amenazan la vida, la integridad física o los bienes materiales, tales como delitos, siniestros viales, incendios o emergencias médicas.
En términos generales, una emergencia puede definirse como una situación inesperada que requiere una intervención rápida y coordinada para prevenir o reducir daños significativos sobre personas, bienes o el entorno (Molina López, 2016; Torres & Navarro, 2019). La literatura distingue entre emergencias de origen:
•	Natural (terremotos, inundaciones, fenómenos meteorológicos extremos).
•	Antrópico (accidentes industriales, incendios urbanos, hechos de violencia).
•	Sanitario o epidemiológico (brotes, epidemias y pandemias) (Vega & Molina, 2017).
Los sistemas de emergencias 911 representan una modalidad particular de estos dispositivos, caracterizados por la centralización de la recepción de llamadas, la coordinación interinstitucional y la gestión integral del ciclo de atención a incidentes. En este tipo de sistemas, como el Centro de Monitoreo y Sistema de Emergencias 911 (CMySE 911) de San Salvador de Jujuy, confluyen:
•	La atención telefónica y registro de incidentes.
•	La asignación y despacho de recursos operativos (policiales, de salud, bomberos u otros).
•	El monitoreo mediante cámaras, sensores u otros dispositivos tecnológicos.
•	La generación de información operativa para los niveles tácticos y estratégicos de decisión.
Desde la perspectiva de la gestión pública, estos sistemas cumplen una doble función: por un lado, son dispositivos de respuesta operativa inmediata; por otro, son productores de datos que permiten caracterizar la demanda de servicios de emergencia, evaluar el desempeño institucional y apoyar la planificación de políticas de seguridad y prevención (Davenport & Harris, 2007; Joyanes Aguilar, 2019).
2.1.1. Importancia de los sistemas de respuesta a emergencias
La relevancia de los sistemas de emergencias se vincula tanto con su capacidad de reducir tiempos de respuesta como con su aporte a la coordinación entre múltiples agencias. En contextos donde segundos o minutos pueden ser decisivos, la existencia de protocolos claros, flujos de comunicación eficientes y una adecuada priorización de incidentes resulta determinante para minimizar daños y salvar vidas.
Diversos estudios señalan que la efectividad de estos sistemas depende no solo de la infraestructura tecnológica y logística disponible, sino también de la calidad de la información que circula a lo largo del proceso de atención (desde la llamada hasta el cierre del incidente) y de la capacidad de las instituciones para aprender de su propia operación (Pusala et al., 2016; Attiah & Kalkatawi, 2025). En este sentido, la gestión sistemática de datos sobre:
•	volumen y tipo de incidentes,
•	tiempos de respuesta,
•	distribución espacial de eventos,
•	utilización de recursos,
permite identificar cuellos de botella, zonas críticas y patrones temporales de demanda, insumos clave para la mejora continua del servicio.
En el caso argentino, la normativa de Sistema de Emergencias Coordinadas (Ley Nacional N.° 25.367) y la Ley de Protección de Datos Personales (N.° 25.326) enmarcan el funcionamiento de estos dispositivos, planteando simultáneamente la obligación de coordinar la respuesta y la necesidad de resguardar la confidencialidad y el uso adecuado de los datos personales. Para centros como el CMySE 911, ello implica articular la dimensión operativa cotidiana con exigencias de registro, trazabilidad y resguardo de la información.
2.1.2. Roles operativos y demanda de información en el sistema 911
Aunque el foco de este trabajo no reside en el análisis exhaustivo de los perfiles de puesto, resulta pertinente señalar que el funcionamiento de un sistema 911 se apoya en una estructura organizacional compuesta, al menos, por tres tipos de roles operativos:
•	Receptores u operadores telefónicos, responsables de la recepción inicial de las llamadas, la aplicación de protocolos de entrevista, el registro estructurado de datos y la clasificación preliminar del incidente.
•	Despachadores, encargados de interpretar la información registrada, asignar recursos adecuados, monitorear la atención en curso y mantener la comunicación con las unidades desplegadas.
•	Supervisores, que coordinan el funcionamiento general de la sala, controlan la calidad del registro y del despacho, y canalizan la información hacia niveles superiores de gestión.
La literatura sobre gestión de emergencias subraya que estos roles se apoyan intensivamente en la disponibilidad y calidad de los datos; es decir, en la medida en que los sistemas informáticos y los repositorios de información sean capaces de proveer:
•	registros actualizados y completos,
•	historiales de incidentes y reincidencias,
•	indicadores de desempeño operativos,
se favorece una toma de decisiones más informada tanto en el plano inmediato (respuesta a un incidente) como en el estratégico (planificación de recursos, redistribución territorial, revisión de protocolos) (Bennett, 2019; Torres Gálvez, 2012).
En este marco, la ausencia de soluciones integradas de análisis de datos —como ocurre actualmente en el CMySE 911 de San Salvador de Jujuy, donde la explotación de información se realiza de manera manual y fragmentada— implica una subutilización del potencial analítico de los datos generados diariamente por el sistema y limita la capacidad institucional para mejorar sus procesos.
2.1.3. Desafíos actuales en la gestión de datos en centros de emergencias
El aumento sostenido en el volumen y complejidad de los datos generados por los sistemas de emergencia se inscribe en el fenómeno más amplio del Big Data en el sector público. Informes especializados destacan que la disponibilidad de datos masivos ofrece oportunidades significativas para mejorar la eficiencia y la transparencia, pero también plantea desafíos en términos de infraestructura, calidad de datos y capacidades analíticas (Lafferriere, 2018; Statista, s. f.).
Entre los principales problemas identificados en la literatura y observables en contextos como el del CMySE 911 se encuentran:
•	Fragmentación y dispersión de la información, cuando los datos se almacenan en múltiples planillas o sistemas no integrados, dificultando la obtención de una visión global.
•	Procesos manuales de consolidación y reporte, que aumentan el riesgo de errores, demoran la generación de informes y limitan la frecuencia del monitoreo.
•	Ausencia de modelos de datos orientados al análisis, donde las bases operacionales (OLTP) no se encuentran articuladas con un entorno analítico (Data Warehouse) que permita consultas multidimensionales y análisis histórico.
•	Déficit de herramientas de visualización y de capacidades analíticas internas, lo que restringe la posibilidad de explotar plenamente la información producida por el sistema.
•	Requerimientos crecientes de seguridad, privacidad y cumplimiento normativo, especialmente en lo referido al tratamiento de datos personales sensibles.
Estos desafíos justifican la necesidad de avanzar hacia arquitecturas de datos más robustas, combinadas con proyectos de ciencia de datos aplicada que permitan transformar los registros operativos en insumos sistemáticos para la gestión de emergencias. El presente trabajo se inscribe precisamente en este enfoque, proponiendo el desarrollo de un Data Warehouse y la aplicación de técnicas de análisis descriptivo, predictivo y geoespacial sobre los datos del CMySE 911.
2.2. La Ciencia de Datos aplicada a la gestión de emergencias
La ciencia de datos se ha consolidado como un campo interdisciplinario que integra estadística, informática, aprendizaje automático y conocimiento del dominio para extraer conocimiento útil y apoyar la toma de decisiones basada en datos (Provost & Fawcett, 2013; Joyanes Aguilar, 2019). En el ámbito de los servicios públicos y, en particular, de los sistemas de emergencias, la ciencia de datos ofrece herramientas para pasar de un enfoque reactivo a un enfoque más proactivo y preventivo en la gestión de incidentes.
Autores como Davenport y Harris (2007) han señalado que las organizaciones que logran competir “sobre analítica” se caracterizan por incorporar sistemáticamente el análisis de datos en sus procesos clave de decisión, apoyándose en infraestructuras de datos consolidadas, modelos analíticos y mecanismos de retroalimentación. En el contexto de los centros de emergencias 911, esta perspectiva se traduce en la posibilidad de:
•	Caracterizar la demanda histórica de servicios por tipo de incidente, franja horaria y zona geográfica.
•	Identificar patrones y tendencias que permitan anticipar picos de demanda o la aparición de zonas de riesgo.
•	Evaluar de manera objetiva los tiempos de respuesta y otros indicadores de desempeño.
•	Orientar la planificación de recursos humanos y materiales en función de evidencias empíricas.
Estudios recientes muestran cómo la aplicación de técnicas de aprendizaje automático y análisis de datos en grandes repositorios de incidentes puede mejorar la clasificación automatizada de llamadas, la priorización de eventos y la generación de reportes para la toma de decisiones (Attiah & Kalkatawi, 2025; Yoon et al., 2025). Estas experiencias refuerzan la pertinencia de incorporar proyectos de analítica de datos en centros de emergencias de contextos latinoamericanos, donde la infraestructura de datos suele estar menos desarrollada.
2.2.1. Fundamentos de la ciencia de datos en el sector público
La adopción de enfoques de ciencia de datos en el sector público presenta características particulares. Por un lado, las administraciones disponen de grandes volúmenes de información producida en el curso normal de sus actividades (registros administrativos, sistemas de gestión, sensores, entre otros). Por otro, enfrentan restricciones de recursos, marcos normativos específicos y la necesidad de garantizar transparencia, equidad y protección de derechos.
La literatura sobre analítica en el sector público subraya que el valor de los datos se materializa cuando éstos son integrados, depurados y analizados dentro de marcos institucionales que articulan capacidades técnicas y políticas públicas (Davenport & Harris, 2007; Joyanes Aguilar, 2019). En el caso de los sistemas 911, esto implica:
•	Definir indicadores clave de desempeño (KPIs) relevantes para la seguridad ciudadana.
•	Establecer procesos sistemáticos de recolección, validación y actualización de datos.
•	Desarrollar modelos y herramientas analíticas alineados con las necesidades de los decisores operativos y estratégicos.
2.2.2. Tipos de análisis de datos en servicios de emergencia
De acuerdo con Pusala et al. (2016), el análisis de datos puede agruparse en tres grandes categorías, que resultan especialmente pertinentes para los sistemas de emergencias:
•	Análisis descriptivo: se orienta a responder qué ha ocurrido y cómo ha evolucionado un fenómeno en el tiempo, mediante técnicas de estadística descriptiva, minería de datos exploratoria y visualización. En un 911, esto incluye la descripción de volúmenes de llamadas, tipos de incidentes, tiempos de respuesta y patrones temporales y territoriales.
•	Análisis predictivo: utiliza modelos estadísticos y de aprendizaje automático para anticipar eventos futuros en función de datos históricos. Para centros de emergencias, esto se traduce, por ejemplo, en la predicción del volumen de llamadas o de incidentes por tipo, día y franja horaria, tal como se propone en este trabajo mediante modelos de series temporales.
•	Análisis prescriptivo: busca sugerir cursos de acción óptimos ante diferentes escenarios posibles, integrando resultados predictivos con técnicas de optimización y simulación. Si bien este trabajo no se focaliza en la analítica prescriptiva, la generación de pronósticos fiables puede constituir un insumo para futuros desarrollos de asignación óptima de recursos.
Estos tipos de análisis no son excluyentes, sino complementarios, y se apoyan en una infraestructura de datos adecuada (Data Warehouse), herramientas de visualización y metodologías de ciencia de datos estructuradas, como se desarrollará en los apartados siguientes.
2.2.3. Datos para la mejora continua y la toma de decisiones en emergencias
La literatura sobre gestión de calidad y mejora continua enfatiza el rol de la retroalimentación basada en datos como condición para el aprendizaje organizacional. En el contexto de los sistemas de emergencias, la disponibilidad de información consolidada y analizada permite:
•	Revisar y ajustar protocolos de atención a la luz de evidencia empírica.
•	Identificar situaciones recurrentes o reincidentes que requieran intervenciones específicas (prevención, control, campañas de concientización).
•	Evaluar el impacto de cambios organizacionales, tecnológicos o normativos sobre los tiempos de respuesta y la satisfacción ciudadana.
Trabajos recientes en servicios de salud y seguridad han mostrado cómo la integración de módulos de analítica de datos en sistemas operativos permite no solo generar reportes automáticos, sino también habilitar espacios de diálogo entre técnicos y decisores, facilitando la interpretación conjunta de resultados y la toma de decisiones informadas (Bennett, 2019; Attiah & Kalkatawi, 2025).
En este sentido, el proyecto propuesto para el CMySE 911 se alinea con la tendencia internacional hacia la construcción de sistemas de información inteligentes, capaces de articular datos históricos, análisis predictivo y visualización interactiva para fortalecer la gestión operativa y estratégica de las emergencias.
2.3. Arquitectura y Gestión de Datos para la Inteligencia Operacional
La capacidad de transformar datos brutos en información accionable y conocimiento es fundamental para cualquier organización moderna, y más aún para los sistemas de emergencias que operan en entornos de alta presión y criticidad. Esta transformación se sustenta en una arquitectura de datos robusta y bien diseñada, que define cómo los datos son recolectados, almacenados, procesados, gestionados y utilizados (Torres Gálvez, 2012). Una arquitectura de datos efectiva no solo soporta las operaciones diarias, sino que también habilita la inteligencia operacional y la toma de decisiones estratégicas.
2.3.1. Conceptos fundamentales de arquitectura de datos y bases de datos relacionales
La arquitectura de datos establece el marco conceptual y tecnológico para la gestión de la información. Incluye no solo los sistemas físicos de almacenamiento (bases de datos, Data Warehouses), sino también los flujos de datos, las políticas de gobernanza, los estándares de calidad y los mecanismos de seguridad que aseguran una gestión eficiente y segura de la información (Torres Gálvez, 2012). Su diseño debe ser flexible y escalable para adaptarse a los cambios tecnológicos y al crecimiento del volumen de datos.
En el corazón de estas arquitecturas se encuentran las bases de datos relacionales (BDR), que organizan los datos en tablas interconectadas mediante claves primarias y foráneas, permitiendo realizar consultas complejas mediante SQL (Codd, 1970). En el contexto de un sistema de emergencias 911, una BDR almacena información sobre llamadas, incidentes y recursos operativos, y constituye la capa transaccional sobre la que se construye la arquitectura analítica. Sin embargo, como se desarrolla en las secciones siguientes, las bases de datos relacionales orientadas a la operación diaria —sistemas OLTP— no están optimizadas para el análisis histórico y multidimensional, lo que justifica la incorporación de un Data Warehouse como capa analítica complementaria.
2.3.2. Sistemas de Procesamiento de Transacciones en Línea (OLTP) en entornos de emergencia
Los sistemas de información que soportan las operaciones diarias de un centro de emergencias 911 son típicamente sistemas de procesamiento de transacciones en línea (OLTP). Estos sistemas están diseñados para manejar un gran volumen de transacciones cortas y rápidas, como la inserción de un nuevo registro de llamada, la actualización del estado de un incidente o la consulta de la disponibilidad de un recurso (Inmon, 2002). La prioridad en los sistemas OLTP es la velocidad de respuesta y la integridad de los datos en tiempo real.
En un entorno 911, los sistemas OLTP son cruciales para:
•	Registrar cada llamada de emergencia con sus detalles (hora, ubicación, tipo de incidente).
•	Actualizar el estado de los recursos (disponible, en ruta, en escena).
•	Gestionar el despacho de unidades y el seguimiento de su intervención.
Sin embargo, los sistemas OLTP no están optimizados para el análisis complejo y multidimensional de grandes volúmenes de datos históricos. Su diseño, enfocado en la eficiencia transaccional, a menudo dificulta la extracción de patrones, tendencias o la realización de pronósticos a largo plazo. Es aquí donde los Data Warehouses complementan la arquitectura de datos, proporcionando un entorno optimizado para el análisis.
2.3.3. Data Warehouse: diseño, componentes (ETL) y modelos dimensionales
Un Data Warehouse (DW), o almacén de datos, es una base de datos centralizada y consolidada diseñada específicamente para el análisis y la toma de decisiones estratégicas (Inmon, 2002). A diferencia de los sistemas OLTP, un DW integra datos de múltiples fuentes operacionales, los depura, transforma y almacena de manera histórica, facilitando consultas complejas y análisis multidimensionales.
Los componentes clave en la construcción y operación de un DW incluyen:
•	Proceso ETL (Extracción, Transformación y Carga): Es la columna vertebral del DW.
○	Extracción: Consiste en la obtención de datos de diversas fuentes operacionales (sistemas OLTP, bases de datos externas, archivos planos).
○	Transformación: Implica la limpieza de datos (manejo de valores faltantes, corrección de inconsistencias), la estandarización de formatos, la agregación y la aplicación de reglas de negocio para asegurar la calidad y coherencia de la información.
○	Carga: Finalmente, los datos transformados se cargan en el DW, donde se almacenan de forma optimizada para el análisis.
•	Modelos dimensionales: Para facilitar el análisis, los DW suelen organizarse utilizando modelos dimensionales, siendo los más comunes el esquema estrella y el esquema copo de nieve.
o	Esquema Estrella: Consiste en una tabla de hechos central (que registra eventos o transacciones, como los incidentes de emergencia) rodeada por tablas de dimensiones (que describen los atributos de los hechos, como tiempo, ubicación, tipo de incidente, recursos involucrados). Este diseño es simple, intuitivo y optimizado para la velocidad de consulta.
o	Esquema Copo de Nieve: Es una variante del esquema estrella donde las tablas de dimensiones se normalizan aún más, dividiéndolas en subtables. Esto reduce la redundancia, pero puede aumentar la complejidad de las consultas al requerir más uniones.
La implementación de un DW en un centro de emergencias permite consolidar la información de incidentes, recursos y personal, habilitando análisis históricos, la identificación de patrones a largo plazo y la evaluación del desempeño del sistema desde múltiples perspectivas.
2.3.4. Beneficios y desafíos en la implementación de arquitecturas de datos avanzadas
La adopción de arquitecturas de datos avanzadas, como un Data Warehouse, en un centro de monitoreo de emergencias ofrece beneficios sustanciales:
•	Mejora en la toma de decisiones: Al consolidar y estructurar la información, se facilita el acceso a datos fiables y oportunos, lo que permite a los gestores tomar decisiones más informadas y rápidas durante situaciones críticas (Torres Gálvez, 2012; Dávila Rodríguez, 2017).
•	Eficiencia operativa: La capacidad de analizar patrones y tendencias históricas permite optimizar la asignación de recursos, mejorar la planificación y anticipar necesidades futuras.
•	Conciencia situacional: La integración de diversas fuentes de datos (llamadas, cámaras, sensores) en una plataforma unificada mejora la comprensión del contexto de una emergencia (Bennett, 2019).
•	Evaluación de desempeño: Facilita el monitoreo de indicadores clave de rendimiento (KPIs) y la evaluación de la efectividad de las intervenciones.
Sin embargo, la implementación de estas arquitecturas no está exenta de desafíos:
•	Calidad y heterogeneidad de los datos: La integración de datos de múltiples fuentes, a menudo con formatos y estándares diferentes, requiere procesos ETL robustos y una atención constante a la calidad de los datos (Dávila Rodríguez, 2017).
•	Infraestructura tecnológica: Requiere inversiones en hardware, software y herramientas de gestión de datos que puedan manejar grandes volúmenes de información y soportar análisis complejos.
•	Capacitación del personal: Es fundamental contar con personal cualificado en diseño de bases de datos, procesos ETL, modelado dimensional y herramientas de análisis. La resistencia al cambio organizacional también puede ser un obstáculo (Arboleda & Montoya, 2017).
•	Seguridad y gobernanza de datos: El manejo de información sensible en un entorno de emergencias exige políticas estrictas de seguridad, privacidad y cumplimiento normativo, como la Ley Nacional N.° 25.326 en Argentina.
A pesar de estos desafíos, los beneficios derivados de una gestión de datos avanzada justifican el esfuerzo, ya que permiten a los sistemas de emergencias evolucionar hacia modelos más proactivos, eficientes y basados en evidencia.
2.4. Modelado Predictivo y Análisis de Series Temporales en Emergencias
El análisis predictivo se ha consolidado como una disciplina fundamental en la ciencia de datos, cuyo objetivo principal es utilizar datos históricos y actuales para realizar pronósticos sobre eventos futuros o comportamientos potenciales (Davenport & Harris, 2007). En el contexto de la gestión de emergencias, esta capacidad predictiva es crucial para anticipar la demanda de servicios, optimizar la asignación de recursos y mejorar la planificación estratégica, permitiendo una transición de un enfoque meramente reactivo a uno más proactivo.
2.4.1. Fundamentos del análisis predictivo
El análisis predictivo utiliza modelos estadísticos y de aprendizaje automático para anticipar eventos futuros a partir de patrones identificados en datos históricos (Davenport & Harris, 2007). A diferencia del análisis descriptivo —que caracteriza lo ocurrido— el análisis predictivo proyecta comportamientos futuros, lo que en el contexto de los sistemas de emergencias se traduce en la posibilidad de anticipar volúmenes de demanda, tipos de incidentes predominantes y necesidades de recursos. Su implementación exitosa requiere datos de calidad, selección adecuada de modelos y una validación rigurosa de los pronósticos (Dávila Rodríguez, 2017). En las secciones siguientes se desarrollan los modelos de series temporales seleccionados para este trabajo.
2.4.2. Modelos de series temporales: ARIMA, SARIMA y SARIMAX
Para la predicción de fenómenos que evolucionan a lo largo del tiempo, como el volumen de llamadas de emergencia, los modelos de series temporales son herramientas analíticas de gran utilidad. Una serie temporal es una secuencia de observaciones de una variable, registradas en intervalos de tiempo sucesivos y ordenados (Chatfield, 2003). La particularidad de estas series es la dependencia temporal entre las observaciones, lo que requiere modelos específicos que capturen esta estructura.
Entre los modelos más utilizados para el análisis y pronóstico de series temporales se encuentran la familia ARIMA:
•	Modelo ARIMA (AutoRegressive Integrated Moving Average): Es un modelo estadístico que combina tres componentes principales (Box & Jenkins, 1976):
o	AR (AutoRegresivo): Utiliza una combinación lineal de observaciones pasadas de la propia serie para predecir valores futuros.
o	I (Integrado): Se refiere al proceso de diferenciación de la serie para hacerla estacionaria (eliminar tendencias o estacionalidad), lo cual es un requisito para la aplicación de los componentes AR y MA.
o	MA (Media Móvil): Modela la dependencia entre una observación y un error residual de un modelo de media móvil aplicado a observaciones pasadas.
El modelo ARIMA es adecuado para series temporales univariadas que exhiben tendencias y autocorrelación, pero no maneja explícitamente la estacionalidad.
•	Modelo SARIMA (Seasonal AutoRegressive Integrated Moving Average): Es una extensión del modelo ARIMA diseñada para series temporales que presentan patrones estacionales recurrentes (Morales, 2023). Incorpora componentes estacionales (AR estacional, I estacional, MA estacional) que permiten modelar la periodicidad observada en los datos (por ejemplo, patrones diarios, semanales o mensuales en el volumen de llamadas de emergencia). La capacidad de SARIMA para capturar la estacionalidad lo hace particularmente relevante en contextos donde la demanda de servicios varía sistemáticamente según el momento del día, el día de la semana o la época del año.
•	Modelo SARIMAX (Seasonal AutoRegressive Integrated Moving Average with eXogenous regressors): Es una evolución del SARIMA que permite incluir variables exógenas (o regresores externos) en el modelo (Artley, 2022). Estas variables pueden ser factores externos que influyen en la serie temporal, como eventos especiales (feriados, grandes eventos públicos), condiciones meteorológicas, o indicadores socioeconómicos. La inclusión de regresores exógenos puede mejorar significativamente la precisión de los pronósticos al incorporar información contextual relevante que no está contenida en la propia serie temporal.
La selección del modelo más adecuado (ARIMA, SARIMA o SARIMAX) depende de las características de la serie temporal (presencia de tendencia, estacionalidad) y de la disponibilidad de variables exógenas relevantes. La validación de estos modelos se realiza mediante métricas estándar como el Error Cuadrático Medio (RMSE), el Error Absoluto Medio (MAE) y el Error Porcentual Absoluto Medio (MAPE), que permiten comparar el desempeño predictivo de diferentes configuraciones de modelos (Morales, 2023).
2.4.3. Aplicaciones de modelos predictivos en la predicción de la demanda de servicios de emergencia
La aplicación de modelos predictivos, y en particular de series temporales, en la gestión de emergencias ha demostrado ser una herramienta valiosa para optimizar la planificación y la respuesta. Diversos estudios han explorado su utilidad en contextos similares:
•	Predicción del volumen de llamadas: Modelos SARIMA y SARIMAX se utilizan para pronosticar el número de llamadas de emergencia en diferentes horizontes temporales (horas, días, semanas), permitiendo a los centros 911 ajustar la dotación de personal y la disponibilidad de recursos (Gorr & Olligschlaeger, 2002; Taylor & Letham, 2018).
•	Anticipación de tipos de incidentes: Más allá del volumen total, estos modelos pueden predecir la probabilidad de ocurrencia de ciertos tipos de incidentes (médicos, policiales, viales), lo que facilita la pre-alocación de recursos especializados.
•	Optimización de la planificación: Los pronósticos generados sirven como insumo para la planificación de turnos de operadores y despachadores, la distribución de patrullas o ambulancias, y la gestión de inventarios de suministros críticos.
Cramer et al. (2011) aplicaron análisis espacial y regresión para predecir llamadas 911 en Portland, Oregón, identificando factores socioeconómicos y geográficos asociados a la demanda. Este tipo de estudios subraya la importancia de integrar el análisis temporal con el espacial para una comprensión más completa de los fenómenos de emergencia.
La implementación de estos modelos en el CMySE 911 de San Salvador de Jujuy permitirá transformar la gestión de la demanda, pasando de una respuesta basada en la experiencia histórica a una planificación informada por pronósticos cuantitativos, lo que redundará en una mayor eficiencia y eficacia del servicio.
2.5. Análisis geoespacial y Sistemas de Información Geográfica (SIG) en la gestión de incidentes
El análisis geoespacial se ha convertido en un componente central de la gestión moderna de emergencias y seguridad ciudadana, dado que la mayoría de los incidentes presentan una clara dimensión territorial. Comprender dónde ocurren los eventos, con qué frecuencia y bajo qué condiciones espaciales y temporales permite identificar patrones de riesgo, focalizar intervenciones y optimizar la distribución de recursos.
Los Sistemas de Información Geográfica (SIG) son herramientas diseñadas para capturar, almacenar, analizar y visualizar datos referenciados geográficamente. Longley et al. (2015) definen la ciencia de la información geográfica como el campo que estudia las propiedades y el uso de datos espaciales, así como los procesos técnicos y conceptuales asociados a su gestión. En el ámbito de los servicios de emergencia, los SIG permiten integrar datos de incidentes con capas de información territorial (barrios, vías, equipamiento urbano, datos sociodemográficos), ofreciendo una base sólida para el análisis de hotspots, la planificación de rutas y la definición de zonas prioritarias de intervención.
En el caso del CMySE 911, la incorporación de análisis geoespacial sobre los registros de incidentes constituye una oportunidad para construir mapas de riesgo dinámicos y apoyar la toma de decisiones tanto operativas como estratégicas.
2.5.1. Conceptos de análisis geoespacial: mapas de calor, densidad de kernel y clustering espacial
Entre las técnicas más utilizadas en el análisis geoespacial de incidentes se destacan:
•	Mapas de calor (heatmaps): Son representaciones gráficas que muestran la distribución de la intensidad o densidad de eventos sobre un territorio, utilizando gradientes de color. En el contexto de emergencias, permiten visualizar de manera intuitiva las zonas con mayor concentración de incidentes, facilitando la detección rápida de áreas problemáticas.
•	Estimación de densidad de kernel (Kernel Density Estimation, KDE): Es una técnica de análisis espacial que genera una superficie continua de densidad a partir de puntos de eventos. Cada incidente contribuye con una “influencia” en el entorno definido por un radio (bandwidth), y la suma de estas influencias produce un mapa de densidad suavizado. La KDE es ampliamente utilizada para identificar zonas críticas de ocurrencia de delitos, accidentes o emergencias médicas, ya que resalta patrones que no siempre son evidentes en simples mapas de puntos (Longley et al., 2015).
•	Clustering espacial: Se refiere a métodos que identifican agrupaciones significativas de eventos en el espacio, distinguiendo áreas de alta concentración de aquellas donde los incidentes se distribuyen de manera más dispersa. Entre los algoritmos más empleados se encuentran:
o	K-means espacial, que agrupa puntos en un número predefinido de clústeres, minimizando la distancia dentro de cada grupo.
o	DBSCAN (Density-Based Spatial Clustering of Applications with Noise), que detecta clústeres basados en densidad, permitiendo identificar estructuras de forma irregular y manejar puntos ruidosos o aislados.
En sistemas de emergencias, estas técnicas permiten no solo visualizar dónde se concentran los incidentes, sino también analizar cómo esas concentraciones varían según franjas horarias, días de la semana o estaciones del año, articulando así lo espacial con lo temporal.
2.5.2. Herramientas SIG para la visualización y análisis de patrones espacio-temporales
El desarrollo de software SIG de código abierto y librerías de análisis geoespacial en lenguajes como Python ha democratizado el uso de estas técnicas en instituciones públicas, incluso en contextos con recursos limitados. Entre las herramientas más relevantes se encuentran:
•	QGIS: Es un software SIG de código abierto ampliamente utilizado en gobiernos locales y organismos públicos. Permite trabajar con capas vectoriales y raster, realizar análisis de proximidad, estimaciones de densidad, geocodificación y generación de mapas temáticos. Su carácter abierto y su amplia comunidad de usuarios lo convierten en una herramienta adecuada para proyectos como el del CMySE 911.
•	Librerías de Python para análisis geoespacial:
o	GeoPandas: Extiende Pandas para operar con datos geoespaciales, facilitando operaciones como uniones espaciales, reproyecciones y análisis de geometrías.
o	Folium o Plotly: Permiten generar mapas interactivos integrados con datos analíticos.
o	Scikit-learn: Provee algoritmos de clustering (como K-means o DBSCAN) que pueden aplicarse sobre coordenadas geográficas o sobre variables derivadas de la ubicación.
La combinación de QGIS con estas librerías posibilita un flujo de trabajo integrado donde los datos del Data Warehouse se extraen, procesan en Python (para análisis espacio-temporal, KDE, clustering) y luego se visualizan en mapas de fácil interpretación para las autoridades.
En el caso del CMySE 911, este entorno tecnológico permitirá:
•	Generar mapas de calor de incidentes por tipo y período.
•	Identificar hotspots recurrentes de siniestros viales, delitos o emergencias médicas.
•	Analizar la relación entre la ubicación de recursos (patrulleros, ambulancias) y la distribución de la demanda.
2.5.3. Impacto del análisis geoespacial en la optimización de recursos y la planificación de la respuesta
La incorporación sistemática del análisis geoespacial en la gestión de emergencias ofrece múltiples beneficios:
•	Optimización de la localización de recursos: Al identificar zonas con alta concentración de incidentes, es posible definir ubicaciones estratégicas para bases operativas, patrullas o ambulancias en espera, reduciendo los tiempos de desplazamiento y, por ende, los tiempos de respuesta.
•	Apoyo a la planificación táctica y estratégica: Los mapas de riesgo y los análisis espacio-temporales permiten diseñar operativos focalizados, reforzar presencia en áreas críticas y orientar políticas de prevención específicas (por ejemplo, campañas de seguridad vial en tramos de alta siniestralidad).
•	Mejora de la coordinación interinstitucional: Cuando los datos georreferenciados son compartidos entre diferentes organismos (policía, salud, bomberos, defensa civil), se favorece una respuesta coordinada, basada en un diagnóstico territorial común.
•	Comunicación con la ciudadanía y transparencia: La visualización de información agregada en mapas puede utilizarse para rendir cuentas sobre la gestión de la seguridad y las emergencias, siempre respetando los principios de anonimización y confidencialidad de datos personales.
En síntesis, el análisis geoespacial constituye un pilar del enfoque propuesto en este trabajo, al complementar el análisis descriptivo y predictivo con una lectura territorial de los incidentes, lo que refuerza el potencial de la herramienta de analítica de datos a desarrollar para el CMySE 911.
2.6. Visualización de datos para la toma de decisiones
La visualización de datos es un componente indispensable de la analítica moderna, especialmente en contextos donde los decisores necesitan interpretar rápidamente información compleja para actuar en situaciones críticas. La visualización puede definirse como el proceso de representar datos de manera gráfica o visual con el propósito de facilitar su comprensión, identificar patrones y comunicar hallazgos de forma efectiva.
Few (2009) sostiene que la visualización de datos constituye una forma poderosa de revelar patrones ocultos, comunicar ideas complejas y proporcionar una comprensión intuitiva de la información. En sistemas de emergencias, donde los tiempos de respuesta son acotados y la carga cognitiva sobre los operadores y gestores es elevada, disponer de representaciones visuales claras y sintéticas de los datos resulta especialmente relevante.
2.6.1. Principios de visualización de datos efectiva
La literatura en visualización de datos enfatiza una serie de principios que orientan el diseño de representaciones gráficas eficaces (Few, 2009; Joyanes Aguilar, 2019):
•	Claridad y simplicidad: Los gráficos deben evitar elementos decorativos innecesarios y centrarse en comunicar el mensaje principal de manera directa. El uso adecuado de colores, escalas y leyendas es crucial para no inducir a errores de interpretación.
•	Adecuación al tipo de dato y objetivo analítico: La elección del tipo de gráfico (barras, líneas, mapas, diagramas de dispersión, etc.) debe responder a la naturaleza de los datos (temporales, categóricos, espaciales) y a las preguntas que se busca responder.
•	Consistencia y comparabilidad: Mantener criterios consistentes de diseño (colores, ejes, unidades) facilita la comparación entre distintos períodos, tipos de incidentes o zonas geográficas.
•	Interactividad cuando es pertinente: En entornos digitales, la posibilidad de filtrar, profundizar o modificar dinámicamente la visualización (por ejemplo, seleccionar períodos o tipos de incidentes) enriquece la exploración de datos y la toma de decisiones.
Aplicados al contexto del CMySE 911, estos principios orientan el diseño de gráficos que muestren, por ejemplo, la evolución del volumen de incidentes, la distribución por tipo de emergencia, los tiempos de respuesta y la intensidad espacial de los eventos.
2.6.2. Dashboards interactivos y su rol en la inteligencia operacional
Los dashboards o tableros de control son interfaces que integran múltiples visualizaciones e indicadores clave de desempeño (KPIs) en un mismo entorno, permitiendo a los usuarios monitorear el estado de un sistema y apoyar la toma de decisiones (Few, 2009). En el ámbito de los centros de emergencias, los dashboards pueden cumplir funciones tanto operativas como estratégicas:
•	En el plano operativo, permiten:
o	Monitorear en tiempo cercano al real el volumen de llamadas entrantes.
o	Visualizar incidentes activos y recursos desplegados.
o	Detectar rápidamente situaciones de saturación (picos de demanda, tiempos de espera elevados).
•	En el plano táctico y estratégico, facilitan:
o	Analizar tendencias históricas de incidentes por tipo, zona y franja horaria.
o	Evaluar el cumplimiento de metas de servicio (por ejemplo, tiempos de respuesta máximos).
o	Identificar patrones recurrentes que demanden intervenciones preventivas o ajustes organizacionales.
El diseño de dashboards efectivos requiere una definición clara de los indicadores clave de desempeño relevantes para la gestión del CMySE 911 (por ejemplo: número de incidentes por tipo, tiempo promedio de respuesta, distribución espacial de eventos, nivel de ocupación de operadores). Además, la integración con el Data Warehouse permite que los dashboards se alimenten automáticamente de datos consolidados y actualizados, reduciendo la dependencia de reportes manuales.
En este trabajo, la implementación de herramientas de visualización interactivas constituye uno de los objetivos específicos, en tanto se busca facilitar la interpretación de los resultados obtenidos mediante análisis descriptivos, predictivos y geoespaciales, y ponerlos a disposición de las autoridades del CMySE 911 en formatos comprensibles y accionables.
2.7. Metodologías de desarrollo: Hefesto y CRISP-DM
La construcción de soluciones de analítica de datos robustas y sostenibles requiere apoyarse en metodologías de desarrollo que estructuren las etapas del proyecto, desde la comprensión del problema hasta la implementación y evaluación de los resultados. En este contexto, el presente trabajo articula dos enfoques complementarios:
•	La metodología Hefesto, orientada al diseño e implementación de Data Warehouses.
•	El proceso CRISP-DM (Cross-Industry Standard Process for Data Mining), ampliamente utilizado en proyectos de ciencia de datos y minería de datos.
2.7.1. Metodología Hefesto para el desarrollo de Data Warehouse
La metodología Hefesto, propuesta por Bernabeu y García (2017), constituye un enfoque específico para el desarrollo de proyectos de Data Warehouse, diseñado con énfasis en la simplicidad, la participación activa del usuario final y la adaptabilidad a contextos con recursos limitados. A diferencia de metodologías clásicas como las de Inmon o Kimball, Hefesto busca ofrecer una guía más flexible y pragmática, adecuada para organizaciones públicas y equipos pequeños.
Aunque la formulación detallada de Hefesto incluye varias etapas, puede sintetizarse en cuatro fases principales (Bernabeu & García, 2017):
1.	Análisis de requerimientos: 
o	Identificación de las necesidades de información de los usuarios.
o	Definición de indicadores clave, dimensiones de análisis y preguntas de negocio que el DW debe responder.
2.	Análisis de fuentes de datos (OLTP): 
o	Relevamiento de los sistemas operacionales existentes.
o	Mapeo entre requerimientos de información e ítems de datos disponibles.
3.	Modelo lógico del Data Warehouse: 
o	Diseño del modelo dimensional (tablas de hechos y dimensiones).
o	Especificación de las relaciones y jerarquías de agregación.
4.	Integración de datos (ETL): 
o	Implementación de los procesos de extracción, transformación y carga.
o	Definición de rutinas de actualización y mecanismos de control de calidad de datos.
Una característica central de Hefesto es la participación permanente del usuario final (en este caso, el personal del CMySE 911) durante todas las etapas. Esta participación contribuye a asegurar que el diseño del DW responda efectivamente a las necesidades reales de gestión, y no solo a una lógica puramente técnica.
En el marco del presente trabajo, Hefesto proporciona el andamiaje metodológico para:
•	Relevar y formalizar los requerimientos de información del CMySE 911.
•	Diseñar el modelo dimensional del DW de incidentes de emergencia.
•	Implementar los procesos ETL necesarios para consolidar los datos históricos (2018–2025).
2.7.2. El proceso CRISP-DM en proyectos de ciencia de datos
El CRISP-DM (Cross-Industry Standard Process for Data Mining) es un modelo de referencia ampliamente adoptado en proyectos de minería de datos y ciencia de datos. Propone un ciclo de vida estructurado en seis fases interrelacionadas (Wirth & Hipp, 2000):
1.	Comprensión del negocio (Business Understanding):
o	Definición de los objetivos del proyecto desde la perspectiva organizacional.
o	Identificación de las preguntas clave a responder y de los criterios de éxito.
2.	Comprensión de los datos (Data Understanding):
o	Recolección inicial de datos.
o	Análisis exploratorio para identificar problemas de calidad, patrones preliminares y relaciones relevantes.
3.	Preparación de los datos (Data Preparation):
o	Selección, limpieza, transformación y construcción de los conjuntos de datos que se utilizarán para el modelado.
4.	Modelado (Modeling):
o	Selección de técnicas de modelado (por ejemplo, modelos de series temporales).
o	Entrenamiento, ajuste de parámetros y evaluación preliminar de desempeño.
5.	Evaluación (Evaluation):
o	Análisis crítico de los resultados obtenidos respecto de los objetivos de negocio.
o	Identificación de limitaciones, riesgos y posibles mejoras.
6.	Despliegue (Deployment):
o	Implementación de los modelos y resultados en el entorno operativo.
o	Planificación de su uso, mantenimiento y actualización.
CRISP-DM destaca por su carácter iterativo: los resultados de una fase pueden llevar a revisitar fases anteriores. En el contexto del CMySE 911, este modelo permite estructurar las actividades asociadas a:
•	El análisis exploratorio de los datos históricos de incidentes.
•	El desarrollo y validación de modelos de series temporales para la predicción de la demanda.
•	La integración de los resultados en dashboards y reportes para usuarios finales.
2.7.3. Justificación de la combinación metodológica para el proyecto
La articulación entre Hefesto y CRISP-DM en este trabajo responde a la necesidad de abordar dos dimensiones complementarias:
•	Dimensión de infraestructura de datos: Hefesto proporciona el marco para diseñar y construir el Data Warehouse que servirá como repositorio unificado de datos históricos del CMySE 911. Sin un DW bien diseñado, la capacidad de análisis descriptivo, predictivo y geoespacial se vería limitada por la fragmentación y baja calidad de los datos.
•	Dimensión analítica y de modelado: CRISP-DM estructura el proceso de ciencia de datos sobre la base de ese DW, guiando las etapas de comprensión del problema, análisis exploratorio, preparación de datos, modelado de series temporales, evaluación de resultados y despliegue en herramientas de visualización.
Esta combinación metodológica resulta especialmente adecuada para un proyecto de maestría que busca, simultáneamente:
•	Diseñar una solución tecnológica (DW + dashboards).
•	Aplicar técnicas de análisis de datos (series temporales, análisis geoespacial).
•	Evaluar el impacto de la herramienta desarrollada en la gestión de un sistema de emergencias real.
2.8. Ética y privacidad en el manejo de datos de emergencias
El manejo de datos en sistemas de emergencias como el 911 involucra información altamente sensible: datos personales de quienes llaman, ubicaciones precisas de incidentes, información sobre víctimas y presuntos agresores, así como detalles sobre situaciones de salud, violencia o delitos. Esto plantea desafíos éticos, legales y técnicos que no pueden ser soslayados en proyectos de analítica de datos y construcción de infraestructuras como un Data Warehouse.
Desde una perspectiva general, la ética en el uso de datos se relaciona con la responsabilidad de garantizar que la obtención, almacenamiento, procesamiento y utilización de la información respeten la dignidad, la privacidad y los derechos de las personas involucradas. En el ámbito público, esta responsabilidad se ve reforzada por el deber de cumplir con los marcos normativos vigentes y por la necesidad de preservar la confianza ciudadana en las instituciones encargadas de la seguridad y la atención de emergencias.
2.8.1. Consideraciones éticas y legales en el uso de datos sensibles
En Argentina, el marco jurídico central en materia de protección de datos personales es la Ley N.° 25.326 de Protección de los Datos Personales, reglamentada por el Decreto N.° 1558/2001. Esta normativa establece principios y obligaciones para el tratamiento de datos, que resultan plenamente aplicables a los sistemas de emergencias:
•	Principio de finalidad: Los datos deben ser recopilados con fines determinados, explícitos y legítimos, y no pueden ser utilizados para propósitos incompatibles con aquellos que motivaron su obtención.
•	Principio de calidad de los datos: La información debe ser exacta, adecuada, pertinente y no excesiva en relación con la finalidad para la cual se recolecta.
•	Consentimiento: En general, el tratamiento de datos requiere el consentimiento libre, expreso e informado del titular, salvo excepciones contempladas por la ley (por ejemplo, en casos de seguridad pública, investigaciones judiciales o cuando medien razones de emergencia que justifiquen el tratamiento sin consentimiento previo).
•	Derechos de los titulares: La normativa reconoce derechos de acceso, rectificación y supresión de datos, así como de confidencialidad.
•	Deber de seguridad y confidencialidad: Los responsables de bases de datos deben adoptar medidas técnicas y organizativas para garantizar la seguridad de la información y evitar su adulteración, pérdida, consulta o tratamiento no autorizado.
En el contexto de un centro 911, muchos de los datos tratados pueden considerarse datos sensibles, en la medida en que revelan información sobre salud, situaciones de violencia de género, antecedentes penales u otros aspectos íntimos de la vida de las personas. La ley establece que estos datos requieren una protección reforzada y un uso estrictamente limitado a los fines que justifican su tratamiento.
Más allá de la normativa, la ética de la gestión de datos en emergencias implica considerar:
•	El riesgo de estigmatización territorial si se difunden mapas o indicadores sin el debido contexto y anonimización.
•	El uso potencialmente discriminatorio de la información si no se establecen salvaguardas adecuadas.
•	La necesidad de transparencia institucional respecto de los usos que se harán de los datos, especialmente cuando se incorporan nuevas herramientas de analítica o visualización.
En este sentido, cualquier proyecto de ciencia de datos aplicado a sistemas de emergencias debe partir de una reflexión explícita sobre los valores que se busca preservar (privacidad, no discriminación, equidad) y sobre los límites de uso de la información disponible.
2.8.2. Estrategias de anonimización y protección de datos en proyectos de analítica
Para compatibilizar el aprovechamiento analítico de los datos con la protección de la privacidad de las personas, es necesario aplicar estrategias de anonimización y seguridad desde el diseño del proyecto (“privacy by design”). Entre las prácticas recomendadas se encuentran:
•	Anonimización o seudonimización de identificadores personales:
o	Eliminación o enmascaramiento de nombres, documentos de identidad, teléfonos y cualquier dato que permita identificar directamente a una persona.
o	Uso de identificadores internos o códigos que no puedan ser fácilmente revertidos a la identidad real (seudonimización), especialmente en entornos de prueba o desarrollo.
•	Agregación de datos para análisis y visualización:
o	Presentar los resultados en forma agregada (por ejemplo, conteos de incidentes por barrio, cuadrante o franja horaria) en lugar de mostrar casos individuales.
o	Evitar la publicación de mapas con puntos exactos de domicilios o ubicaciones sensibles cuando ello pueda poner en riesgo la seguridad de víctimas o comunidades.
•	Control de acceso y perfiles de usuario:
o	Definir distintos niveles de acceso a la información según el rol (operador, analista, directivo), de modo que solo el personal estrictamente necesario pueda visualizar datos con mayor nivel de detalle.
o	Registrar y auditar los accesos y operaciones realizadas sobre las bases de datos para detectar usos indebidos.
•	Seguridad técnica de la infraestructura:
o	Implementar medidas de seguridad informática adecuadas (cifrado en tránsito y en reposo, copias de seguridad seguras, parches y actualizaciones, segmentación de redes).
o	Establecer políticas de resguardo y destrucción segura de datos, especialmente en entornos de desarrollo y pruebas.
•	Documentación y gobernanza de datos:
o	Definir políticas claras de uso de datos y de conservación de información histórica (qué se guarda, por cuánto tiempo, con qué propósito).
o	Asegurar que el proyecto de Data Warehouse y analítica esté alineado con la normativa interna de la institución y con los principios de la Ley N.° 25.326.
En el marco del presente trabajo, estas consideraciones se traducen en decisiones de diseño concretas, tales como:
•	Trabajar con datos anonimizados o seudonimizados, sin acceso a identificadores directos de personas.
•	Utilizar niveles de agregación espacial (barrios, radios, cuadrantes) que permitan el análisis territorial de la demanda sin exponer ubicaciones individuales sensibles.
•	Orientar las visualizaciones y reportes a la mejora de la gestión institucional y la planificación de políticas públicas, evitando cualquier uso que pudiera derivar en discriminación o vulneración de derechos.
De este modo, la propuesta de Data Warehouse y de herramientas de analítica para el CMySE 911 busca no solo mejorar la eficiencia y efectividad del sistema de emergencias, sino hacerlo respetando los principios éticos y legales que rigen el tratamiento de datos personales en el contexto argentino.








Desarrollo de la Propuesta
3. Descripción de la institución y contexto operativo del CMySE 911
El Centro de Monitoreo y Sistema de Emergencias 911 (CMySE 911) de la ciudad de San Salvador de Jujuy constituye el núcleo operativo de recepción, gestión y coordinación de respuestas ante situaciones de emergencia en el ámbito urbano y su área de influencia. Está ubicado en el predio de la Terminal de Ómnibus “General Manuel Eduardo Arias”, en la intersección de las rutas nacionales 9 y 66, lo que le otorga una localización estratégica en términos de accesibilidad a las principales vías de circulación. Esta ubicación favorece la articulación con las distintas unidades operativas de seguridad y servicios de emergencia que intervienen en el territorio.
La misión del CMySE 911 es brindar a la ciudadanía un servicio centralizado de atención, derivación y seguimiento de incidentes relacionados con la seguridad pública, siniestros viales, emergencias médicas y otros eventos que puedan afectar la integridad de las personas y el orden social. En este marco, el centro actúa como punto único de entrada para las demandas de auxilio, articulando la respuesta de diferentes organismos (policía, bomberos, servicios de salud, entre otros) a través de protocolos y procedimientos estandarizados.
3.1 Estructura organizativa y unidades operativas
El funcionamiento del CMySE 911 se apoya en una estructura organizativa que combina áreas de gestión, coordinación y operación directa sobre el territorio. A nivel macro, el sistema se articula con distintas Unidades Regionales de la Policía de la Provincia, a las cuales se encuentran adscritas comisarías y subcomisarías que cubren zonas específicas de la ciudad y localidades cercanas. Esta organización territorial permite distribuir la carga operativa y mejorar los tiempos de respuesta, al asignar incidentes a las dependencias con jurisdicción sobre cada sector.
Internamente, el centro se estructura en torno a una Coordinación C.M. y S.E. 911, de la que dependen la Jefatura de Operaciones y diversas áreas funcionales: Investigación y Desarrollo (I+D), Centro de Monitoreo, Área Técnica, Atención Ciudadana y Auditoría y Calidad. Esta distribución refleja la necesidad de integrar capacidades tecnológicas, operativas y de gestión de calidad en un entorno altamente sensible al tiempo de respuesta y a la confiabilidad de la información.
•	Coordinación: define objetivos institucionales, asigna recursos y establece las políticas internas que regulan el funcionamiento del sistema.
•	Jefatura de Operaciones: gestiona las emergencias y la administración del personal operativo, centralizando la toma de decisiones durante la atención de incidentes.
•	Investigación y Desarrollo (I+D): evalúa e incorpora nuevas tecnologías orientadas a la prevención del delito y la mejora de los procesos de alerta y seguimiento, incluyendo soluciones móviles y de escritorio.
•	Centro de Monitoreo: gestiona la red de cámaras de videovigilancia urbana, realizando la captación y supervisión de imágenes que permiten detectar eventos en tiempo real y apoyar la labor policial.
•	Atención Ciudadana: opera como primera línea de contacto con la comunidad, a través de los operadores que reciben, clasifican y registran las llamadas de emergencia.
•	Área Técnica: se encarga del mantenimiento, reparación y despliegue de la infraestructura tecnológica (equipamiento informático, redes, cámaras, etc.).
•	Auditoría y Calidad: supervisa el cumplimiento de protocolos, el correcto registro de los incidentes y la mejora continua de los procesos, elaborando documentación normativa y de apoyo (protocolos, instructivos, manuales, entre otros).
Esta estructura multidisciplinaria, que combina personal civil y policial en distintos roles (operadores, supervisores, despachadores, personal de base consola), conforma un ecosistema organizacional complejo, donde la disponibilidad y la calidad de la información son factores críticos para garantizar una respuesta oportuna y efectiva.
3.2 Procesos clave: recepción y despacho de emergencias
El flujo operativo del CMySE 911 puede sintetizarse en dos procesos principales: recepción de llamadas de emergencia y despacho de recursos.
En la recepción, los operadores atienden las comunicaciones provenientes de diferentes canales (llamadas al 911, aplicaciones móviles específicas, alertas desde sistemas de videovigilancia u otras líneas especializadas), registrando cada incidente en el sistema mediante una ficha estandarizada. Esta ficha contiene datos esenciales como el número de teléfono del requirente, la tipificación del incidente, la ubicación geográfica y otros detalles relevantes. El procedimiento se rige por modelos de atención predefinidos, que orientan el diálogo con el ciudadano, la priorización del evento y la calidad mínima de la información a registrar.
En la etapa de despacho, la información registrada es evaluada por el personal despachante, que determina el curso de acción conforme a los protocolos vigentes y la disponibilidad de recursos. A partir de ello, se coordina la movilización de unidades policiales u otros servicios de emergencia hacia el lugar del hecho, manteniendo una comunicación permanente hasta la resolución del incidente. El despachante también es responsable de actualizar el sistema con las novedades, estados intermedios y cierre del evento, lo que da lugar a un conjunto de datos históricos que reflejan tanto la demanda de emergencias como el desempeño operativo.
Estos procesos, junto con la labor del área de monitoreo y de base consola, generan un volumen significativo de información estructurada y semiestructurada, que incluye atributos temporales, espaciales y categóricos. No obstante, gran parte de estos datos ha sido tradicionalmente utilizada solo para fines administrativos o de reporte estático, sin un aprovechamiento sistemático desde la perspectiva de la analítica avanzada.
3.3 Políticas, lineamientos y relevancia para la analítica de datos
Entre las políticas institucionales relevantes para esta tesis se destacan, por un lado, la protección de datos personales, en cumplimiento de la Ley Nacional N.° 25.326, y por otro, la orientación a la mejora continua de procesos y tecnologías. La primera impone restricciones y lineamientos específicos sobre el tratamiento de información sensible, lo que condiciona el diseño de la arquitectura de datos y las estrategias de anonimización o seudonimización que deben aplicarse. La segunda se alinea directamente con la propuesta de esta investigación, al promover la incorporación de herramientas que permitan una mejor explotación de la información disponible.
En este contexto, el desarrollo de un Data Warehouse institucional y de un sistema de analítica de datos asociado se presenta como una respuesta concreta a necesidades ya identificadas por la propia organización: contar con reportes dinámicos, mejorar la trazabilidad de los incidentes, analizar patrones temporales y espaciales de la demanda, y disponer de indicadores que apoyen la toma de decisiones operativas y estratégicas. La existencia de procedimientos formales de registro, tipificación y cierre de incidentes, así como de estructuras organizativas dedicadas a la calidad y auditoría, provee una base adecuada para la consolidación y explotación analítica de los datos históricos del CMySE 911.
En síntesis, el CMySE 911 ofrece un entorno institucional y operativo en el cual la incorporación de soluciones de inteligencia de datos puede tener un impacto directo en la eficiencia del sistema de respuesta a emergencias. La presente tesis se inserta en este contexto, proponiendo el diseño e implementación de un sistema integral de analítica que aproveche la información generada por los procesos de recepción y despacho, articulando infraestructura de datos, modelos predictivos y análisis geoespacial para fortalecer la capacidad de planificación y gestión del centro.
4. Aplicación de la Metodología Hefesto para la construcción del Data Warehouse
Dado el volumen, heterogeneidad y criticidad de la información generada por el Centro de Monitoreo y Sistema de Emergencias 911 (CMySE 911), se adoptó la metodología Hefesto como marco de referencia para el diseño e implementación del Data Warehouse institucional. Hefesto provee un conjunto de fases claramente definidas, con énfasis en la simplicidad, adaptabilidad y orientación a los requerimientos de negocio, lo que resulta adecuado para un entorno público con restricciones tecnológicas y organizativas.
La metodología es independiente de la plataforma de software y hardware utilizada, y se centra en la correcta identificación de necesidades de información, el análisis de los sistemas origen, el diseño del modelo dimensional y la implementación de procesos de extracción, transformación y carga (ETL). En el contexto de esta tesis, Hefesto se aplica focalizando el estudio en el subproceso de Recepción de llamadas del área de Atención Ciudadana, dado que constituye la puerta de entrada principal a la cadena de respuesta de emergencias y genera la mayor parte de los datos históricos disponibles para el análisis.
4.1 Análisis de requerimientos
4.1.1 Identificación de preguntas de negocio
El análisis de requerimientos se focaliza en el subproceso de Recepción de llamadas, identificado como la fuente primaria de datos para el estudio de la demanda de emergencias. A partir de las entrevistas realizadas con personal operativo y directivo del CMySE 911, y del relevamiento de las necesidades de información existentes, se definieron tres preguntas de negocio que el sistema analítico debe ser capaz de responder:

Nº	Preguntas De Negocio	Propósito
P1	¿Cuál es la cantidad de llamadas que se reportan por zona geográfica en un período de tiempo determinado?	Identificar la distribución espacial de los incidentes para detectar zonas críticas y apoyar la planificación territorial de los recursos.
P2	¿Cuál es la cantidad de llamadas que se reportan por código de incidente en un período de tiempo determinado?	Analizar la frecuencia de los tipos de eventos según la codificación técnica del sistema, permitiendo categorizar la demanda operativa.
P3	¿Cuál es la cantidad de llamadas que se reportan por clasificador en un período de tiempo determinado?	Evaluar la naturaleza de las emergencias según su clasificación temática, facilitando una visión macro de las problemáticas de seguridad que atiende el centro.
.
4.1.2 Identificación de indicadores y dimensiones
A partir de las preguntas de negocio definidas, se identifican los elementos técnicos que conformarán el modelo multidimensional: el indicador (medida cuantitativa central) y las dimensiones (perspectivas de análisis desde las cuales se lo examina).
El indicador fundamental del modelo es la cantidad de llamadas o incidentes, que representa el conteo total de registros de recepción procesados por el sistema en un período dado. Este indicador responde de manera directa a las tres preguntas planteadas, variando únicamente la perspectiva de corte aplicada.
Indicador	Cantidad de Llamadas
Perspectiva	Zona. 
Código. 
Clasificador. 
Tiempo

La dimensión Tiempo es transversal a las tres preguntas, dado que todas ellas requieren acotar el análisis a un período determinado. La dimensión Zona permite desagregar el volumen de llamadas por territorio, habilitando la identificación de zonas críticas. La dimensión Código y Clasificador, describen la naturaleza del evento desde distintos niveles de detalle y provienen de la misma fuente de datos operacional.


4.1.3 Modelo Conceptual
El modelo conceptual sintetiza los resultados del análisis de requerimientos en una representación gráfica que articula las dimensiones identificadas con el hecho central del análisis. Siguiendo la notación de la metodología Hefesto, el modelo expresa que las dimensiones Zona, Código, Clasificador y Tiempo convergen sobre el hecho Atención, cuya medida resultante es la Cantidad de Llamadas.

[Figura X — Modelo Conceptual. Fuente: Elaboración propia]



Este modelo refleja que cualquier análisis del volumen de atenciones del CMySE 911 puede ser examinado desde cuatro perspectivas independientes o combinadas: el territorio donde ocurrió el incidente, su naturaleza técnica, su clasificación temática y el momento en que fue registrado. La combinación de estas perspectivas constituye la base sobre la cual se construirá el modelo lógico del Data Warehouse en la etapa siguiente.

4.2 Análisis de los OLTP
El análisis de las fuentes de datos operacionales (OLTP) tiene por objetivo establecer la correspondencia entre los requerimientos de información definidos en la etapa anterior y los datos efectivamente disponibles en el sistema transaccional del CMySE 911. Este paso es fundamental en la metodología Hefesto, ya que determina la viabilidad del modelo conceptual y orienta las decisiones de diseño del Data Warehouse.
4.2.1 Conformar indicadores
A partir del modelo conceptual, se define la forma en que el indicador central será calculado en el DW:
Indicador	Hecho	Función de agregación
Cantidad de llamadas	Atención	COUNT

Cada registro en la tabla de hechos representará una atención individual procesada por el sistema, y el indicador se obtendrá contando el número de registros agrupados según las dimensiones seleccionadas.
4.2.2 Establecer correspondencia con el sistema origen
El sistema transaccional del CMySE 911 almacena los registros de atención en una base de datos relacional operacional. A partir del análisis del esquema de datos disponible, se identificaron las tablas fuente que proveen los atributos necesarios para poblar cada dimensión del modelo conceptual:
Las relaciones identificadas fueron las siguientes: 
La tabla “Distrito” se relaciona con la perspectiva “Zona”. 
La tabla “Código” se relaciona con la perspectiva “Código”. 
La tabla “Clasificador” se relaciona con la perspectiva “Clasificador”. 
El campo “Fecha” se relaciona con la perspectiva “Tiempo”. 
La tabla “Llamada” se relaciona con el indicador “Cantidad de llamada”.

El detalle completo del esquema relacional del sistema OLTP, incluyendo el diagrama entidad-relación y el diccionario de datos de las tablas fuente, se presenta en el Anexo A.
4.2.3 Nivel de granularidad
El nivel de granularidad define la unidad mínima de análisis que almacenará la tabla de hechos del DW. Para este trabajo se adoptó una granularidad a nivel de atención individual, donde cada registro representa una llamada o incidente procesado por el sistema. Esta decisión responde a dos criterios: en primer lugar, preservar la máxima flexibilidad analítica, permitiendo agregaciones a cualquier nivel temporal o geográfico sin pérdida de información; en segundo lugar, habilitar el análisis de patrones en franjas horarias específicas, que requiere datos desagregados a nivel de registro individual.
4.2.4 Modelo conceptual ampliado
Establecida la correspondencia con el sistema origen y definida la granularidad, se construye el modelo conceptual ampliado, que incorpora los atributos específicos de cada dimensión derivados del análisis OLTP. Este modelo constituye el insumo directo para el diseño del modelo lógico del DW.



[Figura X — Modelo Conceptual Ampliado. Fuente: Elaboración propia]



4.3 Modelo Lógico del Data Warehouse
El modelo lógico traduce el modelo conceptual ampliado en una estructura de tablas implementable, siguiendo un esquema estrella. Este esquema organiza los datos en una tabla de hechos central vinculada a tablas de dimensiones mediante claves foráneas, optimizando el rendimiento de las consultas analíticas y simplificando la navegación multidimensional.
4.3.1 Tabla de Dimensiones
A continuación, se presentan el diseño de la tabla de dimensión Zona, a modo de ejemplo
Perspectiva “Zona”
 ➢ La nueva tabla de dimensión tiene el nombre “Dimension_Zona”. 
➢ Se agrego una clave principal con el nombre “Cod_ZonaKey”. 
➢ Se modifico el nombre del campo “Id” por “Distrito_Id”. 
➢ Se modifico el nombre del campo “Nombre” por “Zona”.
4.3.3 Tablas de Hechos 
La tabla de hechos tiene el nombre “Hechos_Atencion”. Su clave principal es la combinación de las claves principales de las tablas de dimensiones antes definidas: “Cod_ZonaKey” “Cod_CodigoKey” “CodClasificadorKey” y “FechaKey”
Los hechos creados se corresponden con los indicadores para cálculos y son renombrados: “Cantidad de Llamadas” por “CantidadLlamada”. Además, se agregan campos para el análisis de la información: Latitud y Longitud.
4.3.4 Uniones



[Figura X — Tabla Union. Fuente: Elaboración propia]











4.4 Integración de Datos
La etapa de integración de datos comprende el diseño e implementación de los procesos de Extracción, Transformación y Carga (ETL) que permiten migrar y consolidar la información operacional del CMySE 911 desde el sistema fuente hacia el Data Warehouse diseñado. Este proceso constituye el puente entre la base de datos transaccional (OLTP) y el modelo dimensional del DW, garantizando que los datos almacenados sean consistentes, íntegros y aptos para el análisis.
En la actualidad existen diversas herramientas de software orientadas a la integración de datos de alto rendimiento. Dado que el CMySE 911 mantiene sus datos operacionales en un servidor SQL Server, se analizaron las alternativas disponibles y se decidió continuar con el ecosistema Microsoft, lo que garantiza compatibilidad nativa entre el sistema fuente y el DW destino, simplifica la administración de los entornos y reduce la curva de aprendizaje operativo. Las herramientas utilizadas fueron las siguientes:
•	SQL Server 2022 Developer Edition: motor de base de datos relacional sobre el cual se alojan tanto la base de datos fuente como el Data Warehouse destino. La edición Developer provee todas las funcionalidades de la edición Enterprise en un entorno de desarrollo y prueba, lo que resulta adecuado para el contexto de esta investigación.
•	SQL Server Management Studio (SSMS) v22: entorno gráfico de administración utilizado para la gestión de las bases de datos, la ejecución de consultas de validación y la supervisión del estado de los datos antes y después de cada carga.
•	Microsoft Visual Studio Professional 2022: entorno de desarrollo integrado desde el cual se diseñaron y ejecutaron los paquetes ETL mediante la extensión de SQL Server Integration Services.
•	SQL Server Integration Services (SSIS): componente de SQL Server orientado a la integración de datos, utilizado para diseñar los flujos de extracción desde las tablas OLTP, aplicar las transformaciones necesarias y cargar los datos resultantes en las tablas de dimensiones y de hechos del DW. La conjunción de SSIS con Visual Studio y SQL Server permite un desarrollo ágil e intuitivo de los procesos de integración, con flujos de datos configurables visualmente mediante tareas de control y componentes de transformación.
4.4.1 Carga inicial — Creación del esquema estrella
Una vez definido el modelo lógico, se procedió a la implementación física del Data Warehouse mediante la creación de las tablas en SQL Server. El proceso respetó el orden de dependencia del esquema estrella: primero se crearon las tablas de dimensiones (Dimension_Zona, Dimension_Codigo, Dimension_Clasificador, Dimension_Tiempo) y posteriormente la tabla de hechos (FACT_ATENCION), cuyas claves foráneas referencian a cada dimensión previamente creada. Las sentencias SQL de creación de cada tabla se incluyen en el Anexo B.
El resultado de esta etapa es el esquema estrella implementado en SQL Server, que constituye la estructura base del Data Warehouse del CMySE 911 sobre la cual se ejecutarán los procesos ETL y las consultas analíticas.
[Figura X — Esquema Estrella del DW. Fuente: Elaboración propia]



4.4.2 Generación de los procesos ETL
El proceso ETL fue diseñado e implementado en Microsoft Visual Studio mediante SQL Server Integration Services (SSIS). El flujo desarrollado consta de cinco pasos ejecutados de manera secuencial:
1.	Preparación de las tablas staging: se deshabilitan las restricciones de integridad referencial del motor de base de datos y se eliminan los registros de las tablas intermedias (staging), dejándolas disponibles para recibir los datos del ciclo de carga en curso.
2.	Carga de dimensiones: se comparan los registros del sistema fuente contra los existentes en cada tabla de dimensión. Los registros nuevos se insertan directamente en la dimensión; los registros modificados se almacenan en la tabla staging correspondiente para su posterior actualización; los registros sin cambios son descartados del proceso.
3.	Actualización en modo Batch: los registros almacenados en las tablas staging durante el paso anterior son aplicados a las tablas de dimensiones mediante un Stored Procedure, actualizando en bloque los atributos modificados.
4.	Carga de la tabla de hechos: una vez actualizadas las dimensiones, se procede a la carga de la tabla FACT_ATENCION con los registros del período correspondiente, resolviendo las claves foráneas a partir de las dimensiones ya consolidadas.
5.	Rehabilitación de restricciones: finalizada la carga, se vuelven a habilitar las restricciones de integridad referencial, garantizando la consistencia del esquema estrella.
El resultado de la ejecución del paquete SSIS se muestra en la figura siguiente.



[Figura X — Proceso ETL. Fuente: Elaboración propia]










4.5 Cubos Multidimensionales 
En esta sección se crea un cubo multidimensional llamado “Cubo DW2026” basado en el modelo lógico diseñado anteriormente.
4.5.1 Creación de Jerarquías 
Se definió la “Jerarquía Tiempo”. Los períodos se aplicarán sobre los atributos “Año”, “Trimestre”, “Mes” y “Dia”, en donde: 
•	Un año está compuesto por trimestres. 
•	Un trimestre está compuesto por meses. 
•	Un Mes está compuesto por días.


[Figura X — Cubo Multidimensional. Fuente: Elaboración propia]














4.5 Análisis Predictivo
El análisis predictivo constituye una de las contribuciones centrales de este trabajo, orientada a anticipar el comportamiento futuro de la demanda de emergencias a partir de los datos históricos consolidados en el Data Warehouse. A diferencia del análisis descriptivo, que caracteriza lo ocurrido, el análisis predictivo permite proyectar tendencias y patrones futuros, proporcionando al CMySE 911 una base objetiva para la planificación anticipada de recursos y la toma de decisiones operativas.
4.5.1 Herramientas y entorno de desarrollo
Para el desarrollo del análisis predictivo se utilizó el siguiente conjunto de herramientas:
Visual Studio Code v 1.141: editor de código fuente multiplataforma y de distribución libre, utilizado como entorno principal de desarrollo. Cuenta con soporte para depuración y un amplio ecosistema de extensiones que permiten la ejecución interactiva de código mediante Jupyter Notebook.
Python 3.13: lenguaje de programación multipropósito, multiplataforma y de distribución libre, ampliamente adoptado en el campo de la ciencia de datos por su legibilidad, flexibilidad y riqueza de bibliotecas especializadas.
Las bibliotecas de Python utilizadas fueron:
NumPy 1.26.0: biblioteca fundamental para la computación científica en Python, que provee soporte para operaciones matriciales, álgebra lineal y funciones matemáticas de alto rendimiento.
Pandas: biblioteca orientada a la manipulación y análisis de datos, que proporciona estructuras de datos flexibles y eficientes para el tratamiento de series temporales y conjuntos de datos tabulares.
Matplotlib: biblioteca de visualización de datos que permite generar gráficos estáticos e interactivos a partir de datos en Python.
statsmodels: biblioteca estadística que provee la implementación de modelos de series temporales, incluyendo la función SARIMAX() utilizada para el ajuste de los modelos ARIMA, SARIMA y SARIMAX en este trabajo.
4.5.2 Datos de entrada
Los datos utilizados para el análisis predictivo fueron extraídos directamente del Data Warehouse implementado en la etapa anterior, mediante una consulta SQL que recupera los registros del período comprendido entre el 1 de enero de 2018 y el 31 de diciembre de 2025. La información es agrupada en dos niveles de análisis: cantidad de llamadas por día, para el análisis de la serie temporal global, y cantidad de llamadas por día según clasificador, para el análisis por tipo de incidente en el caso práctico.
La conexión al servidor del DW se realiza desde el entorno Python, permitiendo la transferencia directa de los datos hacia las estructuras de análisis sin necesidad de exportaciones intermedias.
4.5.3 Preprocesamiento
Previo al modelado, se realizó un proceso de análisis, limpieza y preparación de los datos con el fin de garantizar su calidad y adecuación para el análisis de series temporales. Si bien el proceso ETL ejecutado en la etapa de integración de datos resolvió las principales inconsistencias del sistema fuente, en esta etapa se aplicaron transformaciones adicionales orientadas específicamente al análisis temporal: verificación de continuidad de la serie (ausencia de fechas faltantes), tratamiento de valores atípicos y generación del índice temporal sobre el campo de fecha, lo que permite una navegación eficiente sobre la serie.
El resultado de esta etapa es una serie temporal diaria que registra el volumen de llamadas recibidas por el CMySE 911 para cada día del período analizado. La representación gráfica de esta serie, obtenida mediante Matplotlib, permite observar el comportamiento general de la demanda a lo largo de los ocho años considerados, identificando visualmente tendencias, ciclos estacionales y posibles anomalías.
[Figura X — Serie temporal de llamadas diarias al CMySE 911 (2018–2025). Fuente: Elaboración propia]

4.5.4 Modelado
Durante esta fase se implementaron y compararon tres modelos de la familia ARIMA con el objetivo de identificar el que mejor representa el comportamiento de la serie temporal y ofrece mayor precisión en el pronóstico.
Los modelos evaluados fueron:
ARIMA(p,d,q): modelo autorregresivo integrado de media móvil, adecuado para series temporales estacionarias sin componente estacional explícita.
SARIMA(p,d,q)(P,D,Q)s: extensión estacional de ARIMA, que incorpora componentes autorregresivos y de media móvil estacionales para capturar patrones cíclicos recurrentes en la serie.
SARIMAX(p,d,q)(P,D,Q)s: variante de SARIMA que admite la incorporación de variables exógenas, permitiendo enriquecer el modelo con información contextual externa a la propia serie temporal, como feriados, eventos especiales o condiciones estacionales particulares.
Los tres modelos fueron implementados mediante la función SARIMAX() de la biblioteca statsmodels.api, que provee una interfaz unificada para la familia de modelos ARIMA y sus extensiones. La selección del modelo óptimo se realizó mediante el criterio de información de Akaike (AIC): se evaluaron sistemáticamente las combinaciones posibles de parámetros (p, d, q) y sus variantes estacionales, seleccionando aquella configuración que produce el valor de AIC más bajo, lo que indica el mejor equilibrio entre ajuste y parsimonia del modelo.
Una vez identificados los parámetros óptimos, se procedió al ajuste del modelo seleccionado y al diagnóstico de sus residuos mediante la función plot_diagnostics() de Matplotlib, con el fin de verificar el cumplimiento de los supuestos del modelo: ausencia de autocorrelación en los residuos, distribución aproximadamente normal y homocedasticidad.
[Figura X — Diagnóstico del modelo seleccionado. Fuente: Elaboración propia]
4.5.5 Validación y pronóstico
Para evaluar la capacidad predictiva del modelo, se compararon los valores pronosticados con los valores reales observados en la serie temporal, estableciendo predicciones desde el inicio del período de análisis hasta el final de los datos disponibles. El gráfico de líneas resultante permite contrastar visualmente la alineación entre los valores observados y las predicciones del modelo, constituyendo una primera evidencia de la calidad del ajuste.
[Figura X — Valores observados vs. predichos (2018–2025). Fuente: Elaboración propia]

A partir del modelo ajustado y validado, se generó un pronóstico para los próximos 36 meses, donde cada paso del modelo corresponde a un período mensual. El pronóstico permite anticipar el volumen esperado de llamadas para el horizonte de predicción, incorporando tanto la tendencia general de la serie como los patrones de estacionalidad identificados por el modelo.
[Figura X — Pronóstico de llamadas (2026–2028). Fuente: Elaboración propia]

4.5.6 Evaluación de desempeño
La calidad del modelo fue evaluada mediante la métrica Raíz del Error Cuadrático Medio (RMSE), que mide la desviación promedio entre los valores pronosticados y los valores reales observados. El RMSE se expresa en las mismas unidades que la variable de interés —cantidad de llamadas diarias— lo que facilita su interpretación en el contexto operativo del CMySE 911.
Formalmente, el RMSE se define como:

RMSE = √[ (1/n) · Σ (ŷᵢ − yᵢ)² ]

donde ŷᵢ representa el valor pronosticado, yᵢ el valor real observado y n el número de observaciones del conjunto de prueba. Un valor de RMSE menor indica mayor precisión del modelo, siendo deseable minimizarlo en el proceso de selección y ajuste.
4.5.7 Caso práctico
A fin de ilustrar la aplicabilidad del sistema predictivo desarrollado en un escenario operativo concreto, se presenta un caso práctico centrado en un clasificador de incidentes específico con un horizonte de predicción de 12 meses. Este ejercicio permite demostrar la capacidad del modelo para desagregar el análisis por tipo de evento, lo que resulta de particular utilidad para la planificación de recursos orientada a categorías de incidentes con comportamiento estacional diferenciado.
El análisis sobre el clasificador seleccionado permite identificar la tendencia general de la demanda para esa categoría de eventos, los períodos de mayor y menor concentración de llamadas a lo largo del año, y los meses en que la demanda muestra incrementos asociados a patrones estacionales propios de la región —como el receso invernal de julio o el período estival del primer trimestre.
[Figura X — Serie temporal y pronóstico por clasificador seleccionado. Fuente: Elaboración propia]

Los resultados del análisis predictivo por clasificador constituyen un insumo directo para la toma de decisiones operativas: permiten anticipar la demanda diferenciada por tipo de incidente, orientar la asignación de recursos en función de los períodos de mayor presión y planificar estrategias de intervención focalizada —como refuerzos de personal, puntos de control vial o coordinación interinstitucional— con base en evidencia cuantitativa y no en criterios exclusivamente reactivos.








5. Visualizaciones
5.1 QGIS
5.2 DASHBOARD




















6. Análisis de Resultados
7. Evaluación del proyecto
8. Conclusión

Crear documento extenso
