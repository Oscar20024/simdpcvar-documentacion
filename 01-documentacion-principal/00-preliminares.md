![Logotipo de la Universidad Tecnológica del Perú (portada)](media/image1.png)

> **Figura transcrita (image1.png):** Logo institucional en la portada del informe. No aporta información técnica. [Transcripción completa](../06-figuras/informe-principal/img01-logotipo-de-la-universidad-tecnologica-del-peru-portada.md)

**Facultad:**

Ingeniería de Sistemas e Informática

**Curso:**

Curso Integrador II: Sistemas

**Tema del Proyecto:**

Sistema inteligente para el monitoreo y detección de plagas en cultivos de vid en Agrícola Riachuelo

**Integrantes:**

Gutiérrez Rojas, Oscar Antonio Peña Reyes, Grace Lucia Guerrero Figari, Oscar Manuel Zuñiga Matos, María Fernanda

**Docente**

Pacheco Tamariz, Jean Pierre

**Ciclo:**

IX

**Ica, noviembre de 2026**

**Dedicatoria**

Este trabajo está principalmente dedicado a Dios, quien nos dio la vida y nos permitió llevarlo a cabo de la mejor manera posible. Asimismo, expresamos nuestro agradecimiento a los compañeros y compañeras que contribuyeron al desarrollo del producto académico actual, pues sin su colaboración no hubiéramos podido completar el trabajo presente.

**Agradecimientos**

A Dios, por guiar nuestro camino, brindarnos la fortaleza y la sabiduría necesarias para superar cada reto a lo largo de nuestra formación académica.

A la Universidad Tecnológica del Perú y a nuestro cuerpo docente, por la formación académica brindada, los conocimientos transmitidos y los valores que hoy fundamentan nuestro ejercicio en la ingeniería.

A la empresa Agrícola Riachuelo, por brindarnos las facilidades, el espacio y la confianza para la ejecución y aplicación de este proyecto de investigación.

Asimismo, extendemos nuestra gratitud a los compañeros del equipo de investigación, por su compromiso, constante esfuerzo compartido y dedicación para culminar con éxito este trabajo.

**Resumen**

En la actualidad, el seguimiento fitosanitario de los cultivos de vid en Agrícola Riachuelo S.A.C., Ica, se lleva a cabo por medio de visitas al campo y registros en hojas de cálculo. Esto complica la gestión de las pruebas, la ubicación de los descubrimientos y su monitoreo posterior. El propósito del estudio es determinar en qué medida la implementación de un sistema tecnológico de monitoreo fitosanitario mejora el desempeño de este proceso, considerando la eficiencia operativa, la efectividad del monitoreo y la trazabilidad y seguimiento. El estudio, de tipo aplicado, tiene un enfoque cuantitativo, con un nivel explicativo y un diseño preexperimental de grupo único que incluye una prueba previa y otra posterior. La intervención tecnológica incluye un prototipo de captura bilateral a través de dispositivos móviles, almacenamiento local en caso de conectividad escasa, sincronización posterior, utilización de visión artificial para analizar imágenes y detectar potenciales indicios visibles de Planococcus ficus y una plataforma web para consultar, ubicar, revisar y gestionar los hallazgos. La solución se implementó con una aplicación Android (React Native y Expo) que opera con tres celulares —un controlador y dos cámaras en un soporte tipo T—, una plataforma web y API en Django con base de datos PostgreSQL en Supabase y fotografías en Cloudinary, un modelo YOLO ejecutado por un worker de Django y avisos por Telegram. La evaluación comparará el proceso normal con el que es respaldado por el sistema utilizando indicadores como: el tiempo de monitoreo, las personas-hora, la cobertura, la evidencia utilizable, la calidad de los registros, la localización de hallazgos y el seguimiento de casos. El objetivo del proyecto es producir evidencia cuantitativa acerca de las transformaciones asociadas a la intervención tecnológica en situaciones reales, sin dejar de validar los resultados fitosanitarios con el criterio humano.

**Abstract**

Phytosanitary monitoring of grapevine crops at Agrícola Riachuelo S.A.C., Ica, is currently carried out through field inspections and spreadsheet-based records, which hinders the organization of evidence, the localization of findings, and their subsequent follow-up. The objective of this research is to determine to what extent the implementation of a technological phytosanitary monitoring system improves the performance of this process, considering operational efficiency, monitoring effectiveness, and traceability and follow-up. The study is applied research, with a quantitative approach, an explanatory level, and a one-group pre-experimental pretest-posttest design. The technological intervention includes a prototype for bilateral image capture using mobile devices, local storage under limited connectivity conditions, subsequent synchronization, image processing through computer vision to identify possible visible signs of *Planococcus ficus*, and a web platform for consulting, locating, reviewing, and managing findings. The solution was implemented with an Android application (React Native and Expo) running on three smartphones (one controller and two cameras on a T-shaped mount), a Django web platform and API with a PostgreSQL database on Supabase and photographs stored in Cloudinary, a YOLO model run by a Django worker, and notifications via Telegram. The evaluation will compare the conventional monitoring process with the technology-supported process using indicators such as monitoring time, person-hours, coverage, usable evidence, record quality, localization of findings, and case follow-up. The project seeks to generate quantitative evidence on the changes associated with the technological intervention under real field conditions while maintaining human validation of phytosanitary findings.
