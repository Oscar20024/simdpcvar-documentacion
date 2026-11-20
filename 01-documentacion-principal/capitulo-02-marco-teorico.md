# CAPÍTULO II. MARCO TEÓRICO

## 2.1. Antecedentes internacionales

En Italia, Checola et al. \[5\] realizaron una investigación en el campo de la vigilancia fitosanitaria aplicada a la viticultura, cuyo objetivo era monitorear los insectos que son vectores de la Flavescencia dorada. Estos insectos eran Scaphoideus titanus y Orientus ishidae, los cuales fueron atrapados con trampas adhesivas amarillas. Los escritores elaboraron un conjunto de datos públicos, que contenía más de 600 imágenes, y examinaron los algoritmos empleados para detectar objetos, comparando Faster R-CNN con YOLOv8s. Para Faster R-CNN, el <mAP@0.5> fue de 0.86 y para YOLOv8s, de 0.92 \[5\]. Este precedente es importante ya que evidencia que los modelos de detección de objetos pueden aplicarse a la vigilancia automatizada de insectos vinculados con cultivos de vid.

Petre et al. \[4\], por su parte, utilizaron imágenes RGB e infrarrojas térmicas para detectar plagas y patógenos en viñedos de Rumanía, obtenidas a través de teléfonos inteligentes y sensores FLIR. Además de un procesamiento de imágenes a través de cuadrículas de distintos tamaños, su propuesta empleó RetinaNet y SSD con las arquitecturas ResNet50 y ResNet152. Los autores informaron de intersecciones sobre uniones que superaban 0.5 y notaron que ciertas configuraciones hacían posible disminuir el impacto del fondo en las imágenes \[4\]. Este estudio es un precedente próximo al proyecto, ya que muestra la viabilidad de examinar imágenes adquiridas en la vegetación de la vid a través de dispositivos móviles.

Hakim y otros \[9\] hicieron una comparación de diferentes versiones de la familia YOLO para detectar insectos pequeños. Para esto, usaron el conjunto de datos Five-Pest, que contiene 17 251 imágenes, y analizaron modelos que van desde YOLOv7 hasta YOLOv13. Los resultados mostraron que YOLOv9 obtuvo el mayor <mAP@0.5>, con 0.929; en cambio, YOLOv8 mostró beneficios vinculados a su velocidad de inferencia y a una latencia más baja \[9\]. Este trabajo proporciona pruebas para elegir modelos que puedan equilibrar velocidad de procesamiento y capacidad de detección, lo cual es relevante al trabajar con plagas pequeñas en imágenes agrícolas.

Por otra parte, los autores Doan et al. \[8\] dieron forma a un modelo llamado Attention-PestNet, el cual tiene como meta lograr que la detección de plagas sea óptima incluso para situaciones complicadas como las diferencias morfológicas, similitudes visuales de la misma especie y los complejos fondos. La opción propuesta es dar lugar a métodos de atención jerárquica y espacial, los cuales fueron analizados para los conjuntos de datos Pest24, IP102 y R2000; en el caso del IP102, el modelo logró alcanzar un <mAP@0.5> hasta alcanzar el 68.67 %, mientras que para el R2000 uno del 81.54 % \[8\]. Este ejemplo muestra que es valioso el hecho de conseguir extraer rasgos visuales relevantes cuando hay insectos que aparecen en entornos complejos con componentes que dificultan la identificación.

Además, Lee et al. \[7\] idearon PestDetectSim, una herramienta que combina la detección de objetos y la recuperación de imágenes semejantes para identificar 30 tipos de plagas agrícolas. Para localizar las muestras, el sistema utilizó YOLOv8 y completó la detección con métodos de búsqueda por similitud visual y extracción de características. Los autores informaron que la exactitud combinada del diagnóstico es del 98.82 % y que el tiempo de inferencia por imagen es de alrededor de 60 ms \[7\]. La contribución de esta labor consiste en evidenciar que el reconocimiento automatizado tiene la posibilidad de ser complementado con información visual de referencia, lo cual sirve para respaldar la revisión de los resultados adquiridos.

De otro lado, la investigación de Adão et al. \[10\] se centró en la utilización de datos sintéticos para abordar la escasez de imágenes etiquetadas en los usos de viticultura de precisión. Los autores elaboraron imágenes a través de procesamiento digital y herramientas de inteligencia artificial generativa, usando YOLOv8s para identificar la polilla del racimo (Lobesia botrana) y modelos de clasificación para enfermedades de la vid. Los resultados obtenidos presentaron métricas superiores a 0.97 en ciertas evaluaciones efectuadas con los conjuntos analizados en la investigación \[10\]. Este precedente es importante porque demuestra que, cuando hay escasez de imágenes de campo, los datos sintéticos pueden ser un complemento a la información real en la etapa inicial de los modelos.

En términos generales, los antecedentes internacionales demuestran un progreso significativo en la detección de objetos, el análisis automatizado de imágenes aplicadas a la agricultura y la viticultura, el reconocimiento de insectos pequeños y la visión artificial

\[4\], \[5\], \[7\], \[8\], \[9\], \[10\]. No obstante, los estudios revisados se enfocan sobre todo en aspectos particulares, tales como la detección, la clasificación, la comparación de modelos o la creación de conjuntos de datos. No se observa en estos antecedentes una integración conjunta de todos los elementos operativos propuestos en el proyecto actual. Dichos elementos incluyen la captura de evidencia durante la exploración de campo, la operación con conectividad limitada, la asociación del registro con lote, fila, zona y lado, el procesamiento a través de visión artificial, la trazabilidad del hallazgo y el manejo posterior desde una plataforma tecnológica. En este contexto, la propuesta para Agrícola Riachuelo S.A.C. tiene como objetivo unificar estos componentes en un único proceso de monitoreo fitosanitario, conservando la validación humana de los hallazgos potenciales.

## 2.2. Antecedentes nacionales

Huarote Zegarra realizó una investigación en el distrito de Villa El Salvador, Lima, con el objetivo de detectar plagas en hojas de lúcuma utilizando visión artificial y redes neuronales integradas a un sistema móvil \[11\]. La investigación empleó 4500 imágenes captadas en el campo y una red neuronal autoorganizativa (SOM), la cual fue complementada con algoritmos de visión por computadora para extraer las características. Los valores de F1-score fueron altos para las clases evaluadas, y la precisión fue del 99.32

% \[11\]. Este hecho previo es importante para el estudio actual, ya que demuestra que se puede utilizar dispositivos móviles y técnicas de visión artificial para ayudar con la identificación de problemas fitosanitarios directamente mediante imágenes tomadas en un ambiente agrícola.

Córdova Pérez, por su parte, realizó una investigación enfocada en la identificación y categorización automática de insectos agrícolas que se encuentran en trampas pegajosas, utilizando métodos de aprendizaje profundo \[12\]. El planteamiento utilizó los modelos preentrenados YOLOv4 y Faster R-CNN, utilizando el aprendizaje por transferencia para identificar tres especies de insectos plaga: mosca minadora, pulgón verde del melocotonero y mosca blanca. El estudio también integró métodos de segmentación, generación y aumento de imágenes para constituir el conjunto empleado durante la capacitación debido a que las imágenes eran escasas.Mientras que YOLOv4 llegó al 95.82

% \[12\], Faster R-CNN logró un mAP del 94.06 %. Este antecedente proporciona pruebas de que los modelos para detectar objetos tienen la habilidad de automatizar la

identificación de insectos agrícolas a través de imágenes, un elemento directamente vinculado con el componente de visión artificial propuesto en este proyecto.

Igualmente, con el objetivo de evaluar un dron inspector con visión artificial para detectar tempranamente enfermedades como la quemazón y el moteado amarillo \[13\], Lozada Moncada y Tineo Olivares realizaron una investigación en cultivos de arroz ubicados en la región San Francisco-Querecotillo, Piura. La investigación empleó un diseño preexperimental y fue de tipo aplicado, teniendo en cuenta dieciséis evaluaciones de vuelo. Se utilizó YOLOv5 para el reconocimiento de imágenes, y se registraron las coordenadas GPS de las zonas perjudicadas, lo que a su vez posibilitó que se clasificara la gravedad de las enfermedades. Los resultados mostraron valores de precisión entre 0.47 y 0.89, con un promedio de 0.82, mientras que en la mayoría de los vuelos se reportó una detección correcta del 100 % \[13\]. Este estudio resulta relevante porque combina visión artificial con información geográfica para relacionar los hallazgos detectados con su ubicación en el terreno, aspecto importante para el posterior seguimiento de incidencias fitosanitarias.

En conjunto, estos antecedentes nacionales muestran la aplicación de dispositivos móviles, aprendizaje profundo, detección de objetos, visión artificial y geolocalización para apoyar diferentes actividades de monitoreo agrícola \[11\], \[12\], \[13\]. Sin embargo, los estudios revisados se concentran principalmente en componentes particulares, como la identificación de plagas mediante una aplicación móvil, el reconocimiento automatizado de insectos en trampas o la inspección aérea de enfermedades mediante drones. No se evidencia en estos antecedentes una propuesta que reúna, dentro de un mismo flujo operativo, la captura de evidencia durante el recorrido terrestre, el funcionamiento ante conectividad limitada, la asociación de los hallazgos con lote, hilera, zona y lado, su procesamiento mediante visión artificial y el seguimiento posterior mediante una plataforma tecnológica. En este contexto, el presente proyecto plantea integrar dichos componentes para apoyar el monitoreo fitosanitario de los cultivos de vid en Agrícola Riachuelo S.A.C.

## 2.3. Antecedentes sectoriales

En lo que respecta a la vigilancia fitosanitaria en viñedos, Ortis et al. investigaron algunos métodos innovadores de seguimiento de Scaphoideus titanus, vector asociado a la Flavescencia dorada, en viñedos de la región de Véneto, Italia \[14\]. La investigación llevó

a cabo la captura y el envío de imágenes periódicamente para detectar insectos, comparando indirectamente trampas electrónicas con cámaras con trampas adhesivas amarillas tradicionales. Durante el periodo de la investigación, \[14\] uno de los sistemas contrastados se acercaba a ofrecer la misma capacidad de captar y de seguir que las técnicas convencionales. Este precedente es un ejemplo del potencial de la incorporación de imágenes fotográficas y de métodos de observación remota para apoyar la vigilancia fitosanitaria en viñedos.

Con respecto a Prazaru et al., este grupo realizó un estudio de distribución temporal y espacial de poblaciones de Scaphoideus titanus a lo largo de cuatro campañas agrarias que se desarrollaron entre 2021 y 2024 en distintas áreas vitícolas italianas \[15\]. La investigación se basó, además de en trampas adhesivas amarillas e inspecciones visuales, en métodos de análisis espacial para la identificación de patrones de dispersión y concentración. Los resultados lograron identificar agrupamientos espaciales y vínculos entre la existencia de ninfas y la dispersión de adultos registrada en campañas anteriores \[15\]. Este estudio es relevante para la investigación en la que se encuadra el presente trabajo, ya que demuestra la importancia del acoplamiento de los resultados fitosanitarios con su posición y evolución temporal, permitiendo así identificar las áreas del viñedo que necesitan o requieren mayor atención.

Keskin y Turanlı examinaron el comportamiento de Planococcus ficus en un viñedo comercial de uva de mesa situado en Manisa, Turquía \[16\], durante un periodo de tres años. Con el propósito de examinar un sistema de confusión sexual a través de feromonas, se llevó a cabo un seguimiento de machos adultos mediante trampas delta y observaciones visuales de los racimos antes del momento de cosechar. En el terreno de control se observaron porcentajes más elevados que en las parcelas tratadas, donde los autores informaron una disminución gradual del porcentaje de racimos afectados a lo largo de los tres años analizados: 12.3 %, 3.0 % y 2.3 % \[16\]. Este antecedente aporta evidencia sectorial directamente relacionada con *Planococcus ficus* y muestra la importancia de realizar un seguimiento sistemático de sus poblaciones para evaluar la evolución de la plaga y las acciones de manejo aplicadas.

A modo de conclusión, los antecedentes sectoriales afirman que el monitoreo fitosanitario de la vid no debe restringirse a la detección de una plaga, sino que es necesario comprobar la dispersión de esta plaga en el espacio y en el tiempo y llevar a cabo su seguimiento en

el cultivo \[14\], \[15\], \[16\], así como también han puesto de manifiesto cuán necesario es llevar a cabo, de manera periódica, el registro de la información, el seguimiento de las trampas, las inspecciones en el campo y el lugar en el que se realizan los hallazgos para la toma de decisiones gerenciales. Estos antecedentes determinan la necesidad de incluir en el proyecto de Agrícola Riachuelo S.A.C. los métodos que permitan poder conectar la evidencia visual con el entorno del lote, la hilera, la zona y el lado del campo, garantizando la trazabilidad de cada hallazgo para facilitar su revisión y verificación más adelante.

## 2.4. Bases teóricas

### 2.4.1. Monitoreo fitosanitario en cultivos de vid

El monitoreo fitosanitario se basa en la vigilancia sistemática \[2\] de los cultivos y consiste precisamente en detectar la presencia, distribución y evolución de los organismos que puedan comprometer su estado fitosanitario. En el caso de los viñedos, este procedimiento tiene una importancia especial, ya que la difusión de una plaga puede variar tanto en el tiempo como en el espacio y el monitoreo periódico ayuda a reconocer las zonas en las que aparece con mayor frecuencia y orienta las prácticas de manejo \[15\].

La supervisión puede ejecutarse mediante trampas, inspecciones visuales u otras técnicas de registro que permitan la obtención de información sobre el comportamiento y la presencia de las poblaciones. La investigación llevada a cabo en los viñedos ha demostrado ser útil la combinación entre las técnicas más tradicionales y los instrumentos basados en seguimiento remoto con captura fotográfica para así obtener información en el tiempo sin tener que depender únicamente de las inspecciones presenciales \[14\]. De la misma manera, el estudio de las poblaciones de forma temporal y espacial favorece la detección de núcleos o concentraciones estables en las zonas cultivadas \[15\].

De esta manera, el monitoreo no solo consiste en comprobar si existe o no la plaga objeto de estudio, sino que además consiste en registrar en dónde fue detectada, en qué momento fue hallada y cómo se manifiesta en las evaluaciones posteriores. Estos aspectos son relevantes para el estudio que ahora se realiza porque la evidencia de los procesos tiene que seguir conectada con el contexto específico en el que fue recogida.

### 2.4.2. Chanchito blanco de la vid (*Planococcus ficus*)

Planococcus ficus, conocido también como chanchito blanco de la vid, es una plaga que aparece en áreas clave de la producción de uva a nivel mundial. La observación de este organismo y su control pueden verse limitados por el carácter críptico de este ser vivo, su gran capacidad reproductiva y que pueda estar presente en zonas protegidas de la planta \[2\].

P. ficus, además de causar un perjuicio directamente relacionado con su presencia, tiene relevancia fitosanitaria debido a que puede funcionar como vector del virus asociado al enrollamiento de la hoja de la vid. En una investigación efectuada en viñedos situados en el noroeste español durante tres décadas se encontró una correlación entre la proliferación de poblaciones de P. ficus y el aumento en la propagación del Grapevine leafroll-associated virus 3 (GLRaV-3), lo cual demuestra la importancia del insecto para mantener saludable el cultivo \[3\].

Su seguimiento puede incluir trampas de feromonas y revisiones a la vista de las plantas y los racimos para ver cómo sus poblaciones evolucionan y valorar las medidas de control \[16\]. Además, se ha indicado que para ubicar los focos de infestación y concentrar las medidas de control en las áreas donde son requeridas es importante saber la distribución y la densidad del chanchito blanco \[2\]. Por lo tanto, en el proyecto actual se restringirá la detección a individuos o signos que sean visibles dentro del campo de captura, sin suponer que es posible identificar ejemplares escondidos bajo la corteza, entre racimos cerrados o fuera de la imagen.

### 2.4.3. Agricultura y viticultura de precisión

La agricultura de precisión utiliza los datos y la tecnología digital para conocer mucho mejor las circunstancias que se producen en una explotación agrícola y para tomar decisiones conformadas a cada parte del espacio. A la vez, este punto de vista busca hallar datos que surgen del uso de imágenes, sensores o reconociendo información espacial, con el fin de observar cambios que se podrían escapar si no se estudia cada cultivo en su conjunto.

En la viticultura, se han utilizado estas técnicas para detectar plagas y enfermedades a través de imágenes tomadas directamente en los viñedos. Petre et al. emplearon imágenes térmicas y RGB para estudiar las señales vinculadas a problemas fitosanitarios de la vid, lo que evidenció el uso de instrumentos digitales para respaldar la identificación en

circunstancias agrícolas \[4\]. De manera similar, la creación de conjuntos de imágenes especializados ha posibilitado el uso de métodos de aprendizaje profundo para el monitoreo automático de insectos vinculados a los viñedos \[5\].

Contar con datos que sean representativos de las condiciones reales del campo es igualmente necesario para la viticultura de precisión. Se han investigado, en esta línea, métodos para mejorar conjuntos de imágenes limitados con datos sintéticos. El propósito es hacer más fácil el desarrollo inicial de modelos cuando hay escasez de ejemplos auténticos \[10\]. Para este proyecto, este punto de vista es pertinente ya que la información visual deberá reflejar las condiciones específicas de los cultivos de vid en los cuales se llevará a cabo el estudio.

### 2.4.4. Visión artificial aplicada al monitoreo agrícola

Se denomina visión artificial a la capacidad que tienen los sistemas informáticos de obtener información útil a partir de imágenes digitales. En el ámbito de la agricultura, permite obtener información sobre las características visuales de las plantas, detectar plagas y enfermedades, así como patrones inherentes a ciertos problemas fitosanitarios. En cuanto al modo de funcionar de las mismas, la visión artificial comienza con la obtención de una imagen, continúa con el análisis de dicha imagen y concluye extrayendo las propiedades o componentes relevantes.

La visión artificial ha posibilitado la automatización de labores que antes necesitaban una observación manual de las imágenes en el seguimiento de plagas. En el sector vitícola, se han creado modelos y conjuntos de datos que pueden identificar insectos que se encuentran en trampas empleadas para la supervisión de plagas \[5\]. A la vez, se han utilizado imágenes de agricultura tomadas a través de dispositivos móviles en estudios realizados en Perú para detectar problemas fitosanitarios en hojas \[11\]. Por otro lado, otros trabajos han empleado visión artificial para distinguir y clasificar insectos hallados en trampas adhesivas \[12\].

La calidad de la imagen constituye un elemento relevante para este tipo de procesamiento, debido a que factores como iluminación, movimiento, distancia, tamaño del objeto y presencia de elementos que lo oculten parcialmente pueden modificar la información visual disponible \[4\], \[5\]. En consecuencia, la evidencia fotográfica empleada para el

análisis debe conservar suficiente información visual para que los modelos puedan localizar los elementos de interés.

### 2.4.5. Aprendizaje profundo y detección de objetos

El aprendizaje profundo puede considerarse una estrategia basada en redes neuronales que tienen efectivamente la capacidad de aprender rasgos a partir de grandes conjuntos de datos, y estas técnicas permiten precisamente identificar estructuras visuales complejas en el estudio de imágenes agrícolas sin limitarse a rasgos previamente establecidos, y con esta técnica se han podido crear modelos de detección para diversas especies de plagas y enfermedades en imágenes agrícolas \[5\], \[8\].

En el ámbito de la visión artificial, es necesario diferenciar la clasificación de imágenes con la detección de objetos. La detección de objetos además de clasificar una imagen según la clase que la imagen representa, además, también sabremos dónde se encuentra el elemento de interés en la fotografía. Para ello, el modelo tendrá la posibilidad de realizar un cuadro delimitador o zona delimitada asociada con una clase y el nivel de confianza concreto. Esta segunda opción será especialmente importante cuando el insecto esté en un lugar muy pequeño en una foto con otras partes del entorno, tales como hojas, racimos, ramas, etc. \[5\],\[9\].

Entre las arquitecturas utilizadas para este propósito se encuentra la familia YOLO (You Only Look Once), empleada en diferentes investigaciones para la detección de insectos agrícolas \[5\], \[7\], \[9\], \[12\]. Su enfoque permite localizar objetos mediante un proceso de detección de una sola etapa, favoreciendo el equilibrio entre capacidad de reconocimiento y velocidad de procesamiento. Sin embargo, el rendimiento puede variar según el modelo utilizado, el tamaño de los objetos, la calidad y diversidad del conjunto de entrenamiento y las condiciones presentes en las imágenes \[8\], \[9\].

Por este motivo, en el proyecto actual no se asume que la detección sea un diagnóstico final solo por el hecho de utilizar aprendizaje profundo. El modelo producirá un resultado que reflejará un hallazgo potencial, el cual debe conservar su evidencia visual y puede ser sometido a revisión por parte de seres humanos, manteniendo separada la detección automatizada de la confirmación fitosanitaria.

### 2.4.6. Georreferenciación y seguimiento espacial de hallazgos

Dado que facilita la vinculación de un descubrimiento con el sitio donde se observó, la información espacial desempeña un papel fundamental en el monitoreo agrícola. Se han empleado coordenadas GPS y sistemas de visión artificial en estudios nacionales para localizar áreas agrícolas donde se detectaron potenciales enfermedades \[13\]. El análisis de la distribución espacial de las poblaciones de insectos en el área vitícola también ha hecho posible identificar zonas de concentración y observar su comportamiento a través de distintos periodos \[15\].

El valor de esta información no se limita a marcar puntos en un mapa. Es posible cotejar datos, observar concentraciones y facilitar actividades futuras de revisión o gestión cuando se vinculan los hallazgos con áreas concretas del cultivo. Las investigaciones acerca de la distribución de plagas en viñedos demuestran que ciertos focos pueden persistir a lo largo de múltiples campañas y que su identificación espacial ayuda a guiar las tácticas de monitoreo \[15\].

En el contexto del presente proyecto, la coordenada geográfica no se va a entender como un identificador de plantas en sí mismo, sino que se va a usar como un punto de referencia espacial. De este tipo de explicación de la coordenada geográfica, así como fue indicado, se va a proponer complementarla con datos específicos de la estructura del campo: lote, hilera, zona, lado y marcadores de referencia. De esta manera la evidencia visual podrá estar de nuevo vinculada con el contexto apropiado para que se pueda tener mayor facilidad en la localización y el seguimiento del hallazgo posteriormente.

Los principios analizados indican que, para el monitoreo fitosanitario de la vid, es necesario combinar la observación de las plagas con datos sobre su propagación y desarrollo dentro del cultivo \[2\], \[15\]. A la vez, el progreso en aprendizaje profundo, visión artificial y agricultura de precisión posibilita la obtención y análisis de evidencia fotográfica para respaldar la detección de eventuales dificultades fitosanitarias \[4\], \[5\], \[7\], \[8\], \[9\]. Al permitir conectar los descubrimientos con sectores concretos del campo \[13\], \[15\], la inclusión de información espacial complementa este proceso.

La propuesta del proyecto actual se fundamenta en esta base conceptual y consiste en un proceso de monitoreo integrado que incluye la captura de imágenes, el procesamiento a través de visión artificial y la vinculación de los descubrimientos con su contexto espacial.

Más tarde, estos conceptos básicos se vincularán con las dimensiones y variables específicas de la investigación.

## 2.5. Variable independiente

El sistema de monitoreo fitosanitario, que como un sistema de tecnologías de información y en el cual se integran unas herramientas digitales, unos aparatos y un conjunto de mecanismos de procesamiento, es la variable independiente del trabajo que se plantea. Tal sistema busca captar, almacenar, analizar y gestionar información vinculada a la existencia y expansión de problemas fitosanitarios de los cultivos. La incorporación de cámaras, dispositivos móviles, sistemas de recolección y de procesamiento automatizados puede servir de apoyo a la tarea de inspección agrícola e impedir una dependencia exclusiva de controles manuales, tal como lo han corroborado diversos estudios \[11\], \[12\], \[14\].

En el proyecto actual, esta variable no se restringe solamente a la utilización de inteligencia artificial, sino que incluye un flujo tecnológico constituido por diversos componentes interrelacionados:

**Captura de evidencia.** Comprende la obtención de fotografías o información visual directamente en el entorno agrícola mediante dispositivos digitales. En distintas investigaciones se han utilizado teléfonos inteligentes, cámaras incorporadas en sistemas de monitoreo y plataformas aéreas para obtener evidencia visual destinada al análisis fitosanitario \[11\], \[13\], \[14\]. En el proyecto, este componente corresponde a la obtención de evidencia fotográfica durante el recorrido de los cultivos de vid.

**Registro de datos y localización.** Se trata de vincular la evidencia recopilada con datos contextuales que faciliten el reconocimiento de cuándo y dónde se registró. El empleo de datos espaciales y coordenadas geográficas ha posibilitado establecer la asociación entre detecciones y sectores concretos del terreno, así como examinar su distribución en zonas agrícolas \[4\], \[13\]. Para este estudio, la ubicación se complementará con términos específicos del área, como lado, zona, hilera y lote.

**Sincronización y almacenamiento.** Los métodos de almacenamiento son imprescindibles para almacenar imágenes y registros, adquiridos a través del monitoreo, que se necesitarán y se procesarán con posterioridad. Para centralizar los datos derivados de los dispositivos de campo, los sistemas digitales de monitoreo agrícola recurren a

servicios y repositorios de almacenamiento \[11\], \[14\]. En este proyecto la captura se puede almacenar a nivel local de forma que se sincronice cuando las capacidades de la red lo permitan.

**Procesamiento a través de visión artificial.** Este componente se refiere a la evaluación automatizada de las imágenes mediante métodos de procesamiento digital y aprendizaje profundo para detectar, distinguir o categorizar elementos fitosanitarios relevantes. Modelos de detección de objetos y redes neuronales se han utilizado en estudios anteriores para identificar una variedad de insectos y plagas agrícolas \[7\], \[8\], \[12\]. En este estudio, se empleará tal procesamiento para detectar eventuales hallazgos visibles que luego serán revisados por una persona encargada.

**Gestión de los descubrimientos.** Por último, los resultados logrados deben convertirse en datos que se puedan consultar y emplear a lo largo del seguimiento fitosanitario. Los sistemas que se han creado anteriormente han añadido interfaces, información espacial, resultados de detección y métodos de consulta para simplificar la interpretación de los hallazgos en el ámbito agrícola \[7\], \[11\], \[13\]. Este componente, en el proyecto actual, posibilitará que la evidencia sea organizada y que se mantenga el seguimiento de los posibles hallazgos desde su registro hasta su revisión y posterior atención.

Como resultado, se considera que el sistema tecnológico de monitoreo fitosanitario es una variable integral que abarca la recolección de evidencia, la documentación contextual y espacial, la sincronización y almacenamiento de información, el procesamiento a través de visión artificial y la gestión subsiguiente de los descubrimientos. Estos componentes simbolizan la intervención tecnológica que se llevará a cabo para analizar más tarde sus impactos en el desempeño del proceso de monitoreo fitosanitario.

## 2.6. Variable dependiente

El desempeño del proceso de monitoreo fitosanitario constituye la variable dependiente de la presente investigación. En este estudio, se define como la capacidad del proceso para obtener, registrar, organizar y conservar información útil relacionada con la presencia y distribución de problemas fitosanitarios en el cultivo. En el sector vitícola, el monitoreo requiere observaciones periódicas que permitan identificar la presencia de plagas, analizar su comportamiento a lo largo del tiempo y reconocer las zonas donde se concentran los hallazgos \[14\], \[15\].

El monitoreo no se limita únicamente a establecer la presencia o ausencia de una plaga. También comprende la capacidad de vincular cada hallazgo con su ubicación, conservar información que permita comparar registros entre diferentes periodos y realizar un seguimiento posterior. La importancia de identificar focos y estudiar su evolución para orientar las actividades de vigilancia y manejo fitosanitario ha sido evidenciada mediante análisis espaciales y temporales de poblaciones de insectos en viñedos \[15\]. Asimismo, el monitoreo periódico mediante trampas e inspecciones de campo permite evaluar cambios en las poblaciones y los efectos de las medidas de manejo implementadas \[16\].

El proceso puede complementarse mediante herramientas digitales que permitan capturar, procesar y organizar la información obtenida durante las actividades de campo. Diversos estudios en el ámbito agrícola han empleado dispositivos móviles, información geográfica y visión artificial para apoyar la identificación y localización de problemas fitosanitarios \[11\], \[13\]. De esta manera, la información generada puede mantenerse organizada y facilitar la consulta, localización y seguimiento posterior de los hallazgos.

Para efectos de esta investigación, el desempeño del proceso de monitoreo fitosanitario será evaluado mediante tres dimensiones: eficiencia operativa, relacionada con el tiempo y los recursos humanos empleados durante el monitoreo; efectividad del monitoreo, vinculada con la cobertura alcanzada, la utilidad de la evidencia obtenida y la correcta documentación de los registros; y trazabilidad y seguimiento, orientada a la capacidad de localizar posteriormente los hallazgos y mantener su historial desde la detección hasta su revisión, atención, verificación o cierre. Estas dimensiones y sus respectivos indicadores se desarrollan en el apartado 2.7.

## 2.7. Dimensiones e indicadores

La variable dependiente, correspondiente al desempeño del proceso de monitoreo fitosanitario, será evaluada mediante tres dimensiones: **eficiencia operativa, efectividad del monitoreo y trazabilidad y seguimiento**. Estas dimensiones permiten analizar el proceso desde tres perspectivas complementarias: los recursos y el tiempo empleados durante la inspección, la capacidad del proceso para realizar y documentar adecuadamente el monitoreo programado, y la posibilidad de localizar y dar seguimiento a los hallazgos desde su detección hasta su posterior atención o cierre.

### 2.7.1. Eficiencia operativa

Esta dimensión evalúa el tiempo y los recursos humanos necesarios para desarrollar las actividades de monitoreo fitosanitario. Su análisis permitirá determinar la capacidad del proceso para completar las unidades de inspección previstas utilizando los recursos disponibles.

Se consideran los siguientes aspectos:

- Tiempo empleado: duración requerida para completar el monitoreo de una unidad de evaluación determinada.

- Uso de recursos humanos: cantidad de trabajo requerida considerando el número de personas participantes y el tiempo dedicado.

- Capacidad de inspección: cantidad de plantas o segmentos que pueden ser monitoreados dentro de un periodo determinado.

Sus indicadores serán:

*Tabla 1. Indicadores de eficiencia operativa*

<table style="width:100%;">
<colgroup>
<col style="width: 25%" />
<col style="width: 53%" />
<col style="width: 21%" />
</colgroup>
<thead>
<tr class="header">
<th><blockquote>
<p>Indicador</p>
</blockquote></th>
<th><blockquote>
<p>Qué permitirá evaluar</p>
</blockquote></th>
<th><blockquote>
<p>Unidad de</p>
<p>medida</p>
</blockquote></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p>Tiempo total de</p>
<p>monitoreo</p>
</blockquote></td>
<td><blockquote>
<p>Duración necesaria para completar el</p>
<p>recorrido o área establecida.</p>
</blockquote></td>
<td><blockquote>
<p>Minutos u horas</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>Personas-hora</p>
<p>empleadas</p>
</blockquote></td>
<td><blockquote>
<p>Cantidad total de trabajo humano utilizado</p>
<p>durante el monitoreo.</p>
</blockquote></td>
<td><blockquote>
<p>Personas-hora</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>Plantas o segmentos inspeccionados por</p>
<p>hora</p>
</blockquote></td>
<td><blockquote>
<p>Capacidad de inspección alcanzada durante el proceso.</p>
</blockquote></td>
<td><blockquote>
<p>Plantas/hora o segmentos/hora</p>
</blockquote></td>
</tr>
</tbody>
</table>

Fuente: Elaboración propia.

### 2.7.2. Efectividad del monitoreo

Esta dimensión evalúa la capacidad del proceso para completar las actividades de monitoreo programadas y generar información útil y correctamente documentada sobre los posibles hallazgos fitosanitarios. No se limita únicamente a completar el recorrido establecido, sino que considera la cobertura alcanzada, la utilidad de la evidencia obtenida y la calidad de los registros generados.

Los aspectos considerados son:

- Cobertura del monitoreo: grado en que se completan las unidades programadas para inspección.

- Calidad de la evidencia: disponibilidad de fotografías o registros suficientemente útiles para su análisis.

- Calidad del registro: grado en que la información generada contiene los datos requeridos y se encuentra libre de inconsistencias o duplicidades.

Sus indicadores serán:

*Tabla 2. Indicadores de efectividad del monitoreo*

| Indicador | Qué permitirá evaluar | Unidad de medida |
|---|---|---|
| Cobertura alcanzada | Proporción de segmentos monitoreados respecto del total de segmentos programados. | Porcentaje |
| Evidencia utilizable | Proporción de fotografías o registros que cumplen las condiciones necesarias para su análisis. | Porcentaje |
| Registros correctamente documentados | Proporción de registros que contienen la información requerida sin inconsistencias o duplicidades. | Porcentaje |

Fuente: Elaboración propia.

### 2.7.3. Trazabilidad y seguimiento

Esta dimensión evalúa la capacidad del proceso para mantener cada hallazgo vinculado con su contexto de campo, facilitar su localización posterior y conservar su historial desde la detección hasta su revisión, atención, verificación o cierre.

Su evaluación considera tres aspectos:

- Localización posterior: capacidad para regresar al sector relacionado con un hallazgo registrado.

- Vinculación espacial: disponibilidad de datos como lote, hilera, zona, lado y referencia de ubicación.

- Continuidad del seguimiento: conservación del historial del hallazgo desde su detección hasta su atención o cierre.

Sus indicadores serán:

*Tabla 3. Indicadores de trazabilidad y seguimiento*

| Indicador | Qué permitirá evaluar | Unidad de medida |
|---|---|---|
| Hallazgos correctamente localizables | Proporción de hallazgos que pueden ser ubicados nuevamente mediante la información registrada | Porcentaje |
| Tiempo para localizar un hallazgo | Tiempo requerido para regresar al sector relacionado con un hallazgo registrado | Minutos |
| Casos con seguimiento completo | Proporción de casos que mantienen registro desde su detección hasta su atención, verificación o cierre | Porcentaje |

Fuente: Elaboración propia.

## 2.8. Definición de términos

**Monitoreo fitosanitario:** Proceso sistemático de observación, registro y evaluación de información relacionada con la presencia, abundancia y distribución de organismos que pueden afectar un cultivo, con la finalidad de apoyar las actividades de vigilancia y manejo fitosanitario \[14\].

**Planococcus ficus:** Es un insecto hemíptero de la familia Pseudococcidae, también llamado cochinilla harinosa de la vid o chanchito blanco. Se lo tiene por una plaga importante del cultivo a causa de su alimentación sobre la planta, generación de melaza y otros impactos que se producen por su presencia en los viñedos \[2\].

**Agricultura de precisión:** Enfoque de gestión agrícola que incorpora información espacial y temporal junto con herramientas tecnológicas para reconocer variaciones dentro del cultivo y apoyar la toma de decisiones orientadas a sectores específicos del campo \[4\], \[10\].

**Visión artificial:** Conjunto de técnicas computacionales orientadas a adquirir, procesar y analizar imágenes digitales con el propósito de extraer información y reconocer características o patrones de interés presentes en una escena \[11\].

Aprendizaje profundo: Enfoque del aprendizaje automático basado en redes neuronales con múltiples niveles de procesamiento, capaces de aprender automáticamente características complejas a partir de los datos y aplicarlas al análisis de imágenes \[12\].

**Detección de objetos:** Tarea de visión artificial que permite identificar uno o varios elementos presentes en una imagen y determinar su localización mediante regiones delimitadoras o bounding boxes, asociándolos con una categoría determinada \[5\].

**Geolocalización:** Un evento, imagen o registro digital se asocia con datos de localización geográfica, como las coordenadas de longitud y latitud adquiridas a través del sistema GPS, que se emplean como referencia espacial en el área evaluada \[13\]. Esta información en el proyecto actual se completará con una hilera, un lote, un lado, una zona y marcadores de campo.

**Trazabilidad:** En esta investigación, se considera como la capacidad de mantener un hallazgo fitosanitario vinculado con su evidencia, localización, condición y las acciones que se llevaron a cabo mientras se lo seguía, desde el primer momento en que fue detectado hasta su verificación, atención o cierre. Definición operativa exclusiva del proyecto.

**Hallazgo fitosanitario:** Registro de una posible presencia de plaga o signo asociado identificado durante el monitoreo y vinculado con evidencia y contexto de campo. Definición operativa propia del proyecto.

**Evidencia fitosanitaria:** Fotografía o registro digital utilizado para documentar y posteriormente revisar un posible hallazgo durante el monitoreo. Definición operativa propia del proyecto.
