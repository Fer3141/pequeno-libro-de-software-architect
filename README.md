# Pequeño libro de Software Architect

Guía rápida del rol de arquitecto de software: qué hace, niveles de arquitectura, estilos y patrones y las áreas de conocimiento que cubre el camino.

## Índice

- [¿Qué es un arquitecto de software?](#qué-es-un-arquitecto-de-software)
- [Niveles de arquitectura](#niveles-de-arquitectura)
- [Atributos de calidad: qué aspectos tener en cuenta](#atributos-de-calidad-qué-aspectos-tener-en-cuenta)
- [Paradigmas de programación](#paradigmas-de-programación)
- [Principios de diseño](#principios-de-diseño)
- [Estilos y patrones arquitectónicos](#estilos-y-patrones-arquitectónicos)
- [Modelos y vistas de arquitectura](#modelos-y-vistas-de-arquitectura)
- [Cómo tomar decisiones de arquitectura](#cómo-tomar-decisiones-de-arquitectura)
- [Diseño orientado al dominio y sistemas distribuidos](#diseño-orientado-al-dominio-y-sistemas-distribuidos)
- [APIs e integración](#apis-e-integración)
- [Datos](#datos)
- [Infraestructura, redes y despliegue](#infraestructura-redes-y-despliegue)
- [Seguridad](#seguridad)
- [Calidad y prácticas de ingeniería](#calidad-y-prácticas-de-ingeniería)
- [Metodologías y marcos de trabajo](#metodologías-y-marcos-de-trabajo)
- [Habilidades blandas y liderazgo técnico](#habilidades-blandas-y-liderazgo-técnico)
- [Cómo seguir creciendo](#cómo-seguir-creciendo)

---

## ¿Qué es un arquitecto de software?

Un arquitecto de software toma las **decisiones estructurales de alto nivel** de un sistema: las que son caras o casi imposibles de cambiar después (el estilo arquitectónico, cómo se dividen los componentes, qué tecnologías base se usan, cómo se comunican los servicios entre sí), y se asegura de que esas decisiones sigan sirviendo a los requisitos del negocio a medida que el sistema crece.

No es "el que más sabe programar", aunque suele tener background técnico sólido: el foco del rol se corre de escribir código a **diseñar el sistema y comunicar ese diseño** a equipos, a otros arquitectos y a stakeholders no técnicos. Responsabilidades típicas:

- Definir la arquitectura general y los límites entre componentes/servicios.
- Elegir (y justificar) tecnologías, patrones y estándares para todo el equipo u organización.
- Balancear atributos de calidad en tensión entre sí: rendimiento, seguridad, mantenibilidad, costo, velocidad de entrega: casi nunca se puede maximizar todo a la vez, así que el trabajo real es decidir **qué se sacrifica** conscientemente.
- Documentar decisiones de diseño y sus razones (no solo el "qué", también el "por qué"), para que el equipo no las repita mal ni las revierta sin entender el contexto original.
- Mentorear a otros desarrolladores y revisar diseños antes de que se conviertan en código.

---

## Niveles de arquitectura

El trabajo de arquitectura ocurre en distintas escalas, que no son excluyentes: muchas veces la misma persona transita entre ellas según el tamaño de la organización.

- **Arquitectura de aplicación (Application Architecture)**: el diseño interno de un sistema puntual, sus capas, módulos, cómo se organiza el código y se comunican sus componentes.
- **Arquitectura de solución (Solution Architecture)**: cómo una solución de negocio específica (que puede incluir varias aplicaciones, servicios e integraciones) resuelve un problema puntual, encajando con lo que ya existe alrededor.
- **Arquitectura empresarial (Enterprise Architecture)**: la vista de más alto nivel, cómo todos los sistemas de una organización se alinean entre sí y con la estrategia del negocio. Suele apoyarse en marcos formales como **TOGAF**.

---

## Atributos de calidad: qué aspectos tener en cuenta

Antes de elegir un estilo, un patrón o una tecnología, hay que tener claro qué se le pide al sistema más allá de "que funcione". Estos son los **atributos de calidad** (también llamados requisitos no funcionales, o coloquialmente las *"-ilities"*), y casi ninguna decisión de arquitectura tiene sentido sin priorizarlos primero:

- **Rendimiento (performance)**: qué tan rápido responde el sistema bajo carga normal (latencia, throughput).
- **Escalabilidad**: qué tan bien el sistema absorbe más carga agregando recursos, ya sea escalando verticalmente (una máquina más grande) u horizontalmente (más instancias).
- **Disponibilidad (availability)**: qué porcentaje del tiempo el sistema responde correctamente, típicamente expresado en "nueves" (99.9%, 99.99%...).
- **Confiabilidad (reliability)**: que el sistema haga lo que tiene que hacer de forma consistente, sin corromper datos ni comportarse de forma impredecible ante fallos parciales.
- **Seguridad**: que solo acceda a los datos y funciones quien está autorizado, y que los datos estén protegidos en tránsito y en reposo (ver la sección [Seguridad](#seguridad) más abajo).
- **Mantenibilidad**: qué tan fácil es entender, modificar y extender el sistema con el tiempo, sobre todo por gente que no lo escribió originalmente.
- **Testabilidad**: qué tan fácil es verificar automáticamente que el sistema funciona como se espera, sin depender de pruebas manuales.
- **Usabilidad**: qué tan fácil es de usar para quien lo opera o lo consume, ya sea un usuario final o un desarrollador integrando una API.
- **Costo y operabilidad**: cuánto cuesta correr el sistema (infraestructura, licencias) y cuánto esfuerzo humano hace falta para mantenerlo operativo día a día.

La parte difícil no es la lista: es que **estos atributos compiten entre sí**. Más disponibilidad casi siempre cuesta más dinero y complejidad. Más seguridad casi siempre agrega fricción y algo de latencia. Optimizar mucho el rendimiento suele complicar la mantenibilidad del código. El trabajo real de un arquitecto es priorizar explícitamente dos o tres de estos atributos para un sistema dado (los que el negocio realmente necesita) y aceptar conscientemente que los demás quedan en un nivel "suficiente", no óptimo.

---

## Paradigmas de programación

Un **paradigma** es una forma de pensar y estructurar el código, no un lenguaje puntual (muchos lenguajes modernos permiten mezclar varios). La distinción más alta es entre dos formas de decirle a la computadora qué hacer:

- **Imperativo**: el código describe **paso a paso cómo** llegar al resultado (una secuencia de instrucciones que van cambiando el estado del programa). Es el estilo más cercano a cómo funciona el hardware por debajo, y el más intuitivo al empezar a programar.
- **Declarativo**: el código describe **qué** resultado se quiere, dejando que el lenguaje o el motor decida cómo lograrlo. SQL es el ejemplo clásico: se declara qué datos se necesitan, no el algoritmo para buscarlos.

Dentro de esos dos grandes grupos, los paradigmas más relevantes para un arquitecto:

- **Programación orientada a objetos (OOP)**: imperativo en la práctica. Modela el dominio como objetos con estado y comportamiento agrupados, el paradigma dominante en sistemas empresariales grandes (Java, C#, gran parte del ecosistema .NET/Spring).
- **Programación funcional**: declarativo. Se basa en funciones puras (mismo input, mismo output, sin efectos secundarios), inmutabilidad y composición de funciones pequeñas. Cada vez más presente incluso dentro de lenguajes orientados a objetos, sobre todo para lógica de negocio compleja donde el estado mutable compartido es fuente frecuente de bugs difíciles de reproducir.
- **Programación reactiva**: modela el sistema como flujos de datos y eventos asíncronos que se propagan y se transforman con operadores (map, filter, merge...), en vez de pedir el dato de forma bloqueante. Natural para UIs interactivas y sistemas con mucho I/O concurrente.
- **Programación orientada a eventos (event-driven)**: el sistema reacciona a eventos que van ocurriendo (un pedido creado, un pago confirmado) en vez de seguir un flujo de control lineal. Es más un paradigma a nivel de arquitectura que de lenguaje, y se apoya en colas de mensajes (ver [APIs e integración](#apis-e-integración)).

Un arquitecto no necesita dominar todos los lenguajes, pero sí entender estos paradigmas lo suficiente como para elegir el que mejor encaje con el problema (por ejemplo, funcional para lógica de negocio con muchas reglas y poco estado compartido, orientado a eventos para procesos de negocio que naturalmente ocurren en pasos desacoplados en el tiempo) y para poder leer y cuestionar diseños escritos en cualquiera de ellos.

---

## Principios de diseño

Reglas y heurísticas que atraviesan cualquier paradigma, pensadas para que el código siga siendo manejable a medida que el sistema crece:

- **SOLID**: cinco principios (responsabilidad única, abierto/cerrado, sustitución de Liskov, segregación de interfaces, inversión de dependencias) que buscan código más fácil de extender sin romper lo existente. Nacieron en el mundo orientado a objetos, pero varias de sus ideas (responsabilidad única, bajo acoplamiento) aplican igual en otros paradigmas.
- **Patrones de diseño** (Gang of Four y otros): soluciones ya probadas a problemas recurrentes de diseño, útiles como vocabulario común entre desarrolladores, peligrosos si se aplican por moda en vez de porque el problema realmente los necesita.
- **DRY (Don't Repeat Yourself) y KISS (Keep It Simple)**: evitar duplicar la misma lógica en varios lugares (para no tener que sincronizar cambios a mano en cada copia), y preferir la solución más simple que resuelva el problema real.
- **Simplicidad primero**: la habilidad de resistir la sobre-ingeniería es tan importante como conocer los patrones. Un arquitecto que mete abstracciones para un problema que no existe todavía genera más deuda técnica que uno que no sabía el patrón.

---

## Estilos y patrones arquitectónicos

La decisión de más alto impacto y más difícil de revertir: cómo se estructura el sistema a grandes rasgos.

- **Cliente-servidor**: el estilo base de casi todo sistema en red, donde un cliente pide y un servidor responde. Casi todos los demás estilos son variaciones o extensiones de esta idea.
- **Arquitectura en capas (Layered)**: el sistema se organiza en capas horizontales (presentación, lógica de negocio, acceso a datos), cada una hablando solo con la capa inmediata. Simple de entender, pero puede volverse rígida si las capas se acoplan más de lo debido.
- **MVC / MVP / MVVM**: patrones para separar la lógica de presentación (Model-View-Controller/Presenter/ViewModel), típicos en aplicaciones con interfaz de usuario, cada uno con una forma distinta de conectar la vista con el estado.
- **Arquitectura orientada a servicios (SOA)**: el sistema se divide en servicios de negocio reutilizables, expuestos generalmente vía un bus de integración (ESB) y protocolos como SOAP, el antecesor directo de los microservicios, con más énfasis en el bus centralizado.
- **Microservicios**: cada servicio es pequeño, se despliega de forma independiente, y tiene su propia base de datos. A cambio de flexibilidad y escalado independiente, se paga en complejidad operativa (observabilidad, comunicación de red, consistencia de datos entre servicios).
- **Microfrontends**: la misma idea que microservicios, pero aplicada al frontend: distintos equipos pueden desplegar partes de la misma interfaz de forma independiente.
- **Serverless**: el código corre en funciones administradas por un proveedor de nube (ver el [pequeño libro de AWS](https://github.com/Fer3141/pequeno-libro-de-aws#lambda-serverless)), sin gestionar servidores; se paga por ejecución, no por tiempo de servidor encendido.
- **CQRS y consistencia eventual**: separar el modelo de **escritura** (comandos) del de **lectura** (queries), a veces con almacenamiento distinto para cada uno. Suele venir acompañado de consistencia eventual: los datos de lectura pueden estar unos instantes desactualizados respecto a la última escritura, a cambio de mejor rendimiento y escalabilidad.
- **Service mesh**: una capa de infraestructura dedicada a la comunicación entre servicios (reintentos, circuit breakers, mTLS, observabilidad) sacada del código de la aplicación: típico en clusters de Kubernetes con muchos microservicios (ver el [pequeño libro de Kubernetes](https://github.com/Fer3141/pequeno-libro-de-kubernetes)).

> No hay un estilo "correcto" en abstracto: la elección depende del tamaño del equipo, la escala esperada, y cuánta complejidad operativa la organización puede sostener. Empezar con microservicios en un equipo de 3 personas suele ser un error tan grande como quedarse con un monolito gigante en una organización de 500 ingenieros.

---

## Modelos y vistas de arquitectura

Un sistema no se puede explicar con un solo diagrama: un ejecutivo necesita ver otra cosa que un desarrollador que va a tocar el código mañana. Los **modelos de arquitectura** son formas estandarizadas de representar el mismo sistema desde distintos niveles de abstracción, para que cada audiencia vea lo que necesita sin ahogarse en detalle irrelevante:

- **Modelo C4** (de Simon Brown): cuatro niveles de zoom, pensados para "empezar grande e ir agrandando el detalle a demanda":
  1. **Contexto**: el sistema como una caja única, mostrando con qué usuarios y sistemas externos interactúa. Para audiencias no técnicas.
  2. **Contenedores**: abre la caja y muestra las aplicaciones/servicios de alto nivel que la componen (una API, una base de datos, una app web) y cómo se comunican.
  3. **Componentes**: abre un contenedor puntual y muestra sus piezas internas principales (controladores, servicios, repositorios).
  4. **Código**: el nivel de detalle de clases o módulos, generalmente autogenerado por el IDE en vez de mantenido a mano.
  
  La idea central es no mezclar niveles en el mismo diagrama: cada uno responde una pregunta distinta.

- **Modelo de las 4+1 vistas** (Kruchten): describe el sistema desde cuatro perspectivas independientes, todas ancladas a un quinto elemento central, los casos de uso: la **vista lógica** (funcionalidad para el usuario final, típicamente un diagrama de clases/objetos), la **vista de procesos** (concurrencia y comunicación en tiempo de ejecución), la **vista de desarrollo** (cómo se organiza el código en módulos/paquetes) y la **vista física** (cómo se despliega el software sobre hardware/infraestructura real).

- **arc42**: una plantilla de documentación de arquitectura de código abierto, con secciones predefinidas (restricciones, contexto, estrategia de solución, vista de bloques, vista de runtime, decisiones, riesgos...) pensada para que ningún equipo tenga que inventar desde cero cómo documentar su sistema. Más información: https://arc42.org/

- **UML**: el estándar más antiguo y formal para diagramar software (diagramas de clases, secuencia, componentes, despliegue...). En la práctica, la mayoría de los equipos usan solo un subconjunto liviano de UML combinado con C4, en vez de UML completo, porque mantener diagramas UML exhaustivos actualizados suele costar más de lo que aportan.

> Cualquiera de estos modelos falla si nadie lo mantiene actualizado. Un diagrama desactualizado que el equipo sigue consultando genera más daño que no tener diagrama, porque transmite una falsa sensación de entender el sistema.

---

## Cómo tomar decisiones de arquitectura

La mayor parte del trabajo de un arquitecto no es dibujar diagramas, es decidir entre opciones con información incompleta y hacerlo de forma que el resto del equipo pueda entender y confiar en esa decisión después.

**Un marco simple para clasificar la decisión antes de tomarla**, popularizado por Amazon: preguntarse si es una decisión de **una sola vía** (*one-way door*, difícil o imposible de revertir, como elegir la base de datos principal o el proveedor de nube) o de **doble vía** (*two-way door*, fácil de revertir si sale mal, como el nombre de una variable de configuración). Las decisiones de una sola vía merecen análisis cuidadoso, consulta con otros y documentación formal; las de doble vía se pueden tomar rápido y ajustar después. Tratar toda decisión como si fuera de una sola vía es la forma más común de paralizar a un equipo.

**Análisis de trade-offs**: dado que los [atributos de calidad](#atributos-de-calidad-qué-aspectos-tener-en-cuenta) compiten entre sí, casi ninguna decisión de arquitectura es "la mejor" en abstracto, solo es la mejor **para las prioridades de este sistema en particular**. El ejercicio práctico es, para cada opción sobre la mesa, listar explícitamente qué atributo mejora y cuál empeora, en vez de evaluar solo el atributo que más le importa a quien está decidiendo.

**ATAM** (*Architecture Tradeoff Analysis Method*): un método formal, típicamente reservado para decisiones grandes o de alto riesgo, donde se definen escenarios concretos por cada atributo de calidad prioritario (por ejemplo, "el sistema debe seguir respondiendo si un nodo de la base de datos cae") y se evalúa cómo cada arquitectura candidata se comporta frente a esos escenarios, con stakeholders de distintas áreas presentes en la evaluación.

**Matriz de decisión ponderada**: cuando hay varias opciones y varios criterios, una tabla simple ayuda a que la decisión no dependa solo de quién habló más fuerte en la reunión: se listan las opciones, se listan los criterios relevantes con un peso según su importancia para este proyecto, se puntúa cada opción por criterio, y se suma. No reemplaza el criterio humano, pero hace explícitos los criterios que si no quedan implícitos y sin discutir.

**Documentar con un ADR** (*Architecture Decision Record*): un archivo corto por decisión importante, versionado junto con el código, con una estructura típica de: título, estado (propuesta/aceptada/reemplazada), contexto (qué problema fuerza la decisión), la decisión en sí, y las consecuencias (qué se gana y qué se sacrifica). El valor no está en el archivo en sí, está en que dentro de un año, cuando alguien pregunte "¿por qué elegimos esto?", la respuesta esté escrita en vez de perdida en la memoria de quien ya no está en el equipo.

**Involucrar a los stakeholders correctos según el tipo de decisión**: una decisión que afecta el costo operativo necesita a quien paga la infraestructura en la conversación; una que afecta la seguridad necesita al equipo de seguridad; una que afecta la experiencia de otros equipos de desarrollo necesita su feedback antes de cerrarla, no después de implementada.

---

## Diseño orientado al dominio y sistemas distribuidos

- **Domain-Driven Design (DDD)**: modelar el software calcando el vocabulario y las reglas del negocio real (el "lenguaje ubicuo"), dividiendo el sistema en **bounded contexts**, límites claros donde un mismo término puede significar algo distinto en cada contexto (un "Cliente" en Ventas no es lo mismo que un "Cliente" en Soporte). Es la base conceptual más común detrás de cómo se dividen los microservicios en la práctica.
- **Teorema CAP**: en un sistema distribuido, ante una partición de red (**P**, que tarde o temprano ocurre), hay que elegir entre **consistencia** (**C**, todos ven el mismo dato) y **disponibilidad** (**A**, el sistema sigue respondiendo). No es una elección teórica: define en qué falla cada base de datos distribuida cuando algo se rompe.
- **Propiedades ACID**: atomicidad, consistencia, aislamiento y durabilidad, las garantías clásicas de una transacción en una base de datos relacional. Sistemas distribuidos grandes muchas veces las relajan a propósito (ver CQRS/consistencia eventual arriba) porque garantizarlas a escala global es costoso.
- **Sistemas distribuidos**: entender que la red puede fallar, los relojes de distintas máquinas no están perfectamente sincronizados, y los mensajes pueden llegar duplicados, fuera de orden o no llegar. Diseñar asumiendo eso (idempotencia, reintentos con backoff, timeouts) en vez de asumir que "la red siempre funciona".

---

## APIs e integración

- **REST**: el estilo dominante para APIs HTTP. Ver el [pequeño libro de diseño de APIs](https://github.com/Fer3141/pequeno-libro-de-diseno-de-apis) para el detalle completo (métodos, códigos de estado, versionado, paginación).
- **GraphQL**: el cliente pide exactamente los campos que necesita en una sola consulta, evitando over-fetching/under-fetching, algo útil cuando distintos clientes (web, mobile) necesitan formas muy distintas de los mismos datos.
- **gRPC**: comunicación binaria de alto rendimiento sobre HTTP/2 con contratos fuertemente tipados (`.proto`), la elección típica para comunicación interna entre microservicios, no para APIs públicas.
- **Colas de mensajes (message queues)**: comunicación **asíncrona** entre servicios (RabbitMQ, Kafka, SQS): un servicio publica un mensaje y sigue sin esperar respuesta inmediata; otro lo consume cuando puede. Desacopla productores de consumidores en el tiempo, y absorbe picos de carga.
- **ESB / SOAP**: el enfoque más antiguo de integración empresarial, con un bus centralizado (ESB) que enruta y transforma mensajes entre sistemas, típicamente en XML/SOAP. Sigue presente en muchas organizaciones grandes con sistemas legados, aunque en arquitecturas nuevas se prefieren APIs REST/gRPC y colas de mensajes.

---

## Datos

- **Bases de datos SQL vs. NoSQL**: relacionales (PostgreSQL, MySQL) cuando el dato tiene estructura fija y relaciones importantes, con garantías ACID fuertes; NoSQL (documentales, clave-valor, columnares, de grafos) cuando el esquema es flexible o la escala horizontal importa más que las relaciones estrictas. La elección no es "cuál es mejor" en general, sino cuál encaja con la forma real de los datos y los patrones de acceso.
- **Data warehousing**: almacenar datos históricos ya transformados y optimizados para análisis (no para las operaciones del día a día del negocio), separados de las bases de datos transaccionales.
- **ETL**: el proceso de **extraer** datos de sistemas de origen, **transformarlos** a un formato consistente, y **cargarlos** en el destino analítico (el data warehouse), la tubería que alimenta a los sistemas de reporting y BI.
- **Big data (Hadoop, Spark)**: herramientas para procesar volúmenes de datos que no entran ni se procesan razonablemente en una sola máquina, distribuyendo el cómputo entre un cluster.

---

## Infraestructura, redes y despliegue

Un arquitecto no necesita operar la infraestructura día a día, pero sí entender lo suficiente para diseñar sistemas que funcionen bien sobre ella:

- **Modelo OSI / TCP-IP**: el marco conceptual de capas de red (física, enlace, red, transporte, aplicación). Entender en qué capa vive cada problema (¿es de DNS? ¿de TCP? ¿del protocolo de aplicación?) acelera muchísimo el diagnóstico de un incidente.
- **HTTP/HTTPS**: el protocolo base de casi toda comunicación entre servicios modernos, con sus métodos, códigos de estado, headers y TLS. Ver el [pequeño libro de diseño de APIs](https://github.com/Fer3141/pequeno-libro-de-diseno-de-apis) para el detalle aplicado a diseño de APIs.
- **Proxies y balanceadores de carga**: un proxy inverso reparte tráfico entrante entre varias instancias de un servicio, habilita TLS termination centralizado, y puede cachear respuestas: pieza casi obligatoria en cualquier arquitectura con más de una instancia de un servicio.
- **Contenedores**: empaquetar una aplicación con sus dependencias para que corra igual en cualquier entorno. Ver el [pequeño libro de Docker](https://github.com/Fer3141/pequeno-libro-de-docker).
- **Orquestación de contenedores**: gestionar el ciclo de vida de muchos contenedores a escala (reinicio automático, escalado, actualización sin downtime). Ver el [pequeño libro de Kubernetes](https://github.com/Fer3141/pequeno-libro-de-kubernetes).
- **Infraestructura como código (IaC)**: declarar la infraestructura (servidores, redes, bases de datos) en archivos de texto versionados, en vez de crearla a mano en una consola. Ver [CloudFormation en el pequeño libro de AWS](https://github.com/Fer3141/pequeno-libro-de-aws#cloudformation-infraestructura-como-código) o herramientas como Terraform.
- **Proveedores de nube**: AWS, Azure, GCP, cada uno con su propio catálogo de servicios gestionados, pero resolviendo en el fondo los mismos problemas (cómputo, almacenamiento, redes, bases de datos gestionadas).
- **CI/CD**: automatizar la integración continua (compilar y testear en cada cambio) y el despliegue continuo (llevar ese cambio a producción de forma segura y repetible), para que desplegar deje de ser un evento manual riesgoso.

---

## Seguridad

- **OWASP Top 10**: la lista de referencia de los riesgos más críticos en aplicaciones web, como inyección, control de acceso roto, fallas criptográficas, etc. Ver el detalle en el [pequeño libro de DevSecOps](https://github.com/Fer3141/pequeno-libro-de-devsecops#owasp-top-10-referencia-rápida).
- **Estrategias de autenticación**: tokens (JWT), OAuth2/OIDC para delegar autenticación a un proveedor externo, API keys para integraciones servidor-a-servidor. Ver el detalle en el [pequeño libro de diseño de APIs](https://github.com/Fer3141/pequeno-libro-de-diseno-de-apis#autenticación-y-autorización).
- **PKI (infraestructura de clave pública)**: el sistema de certificados y autoridades certificadoras que sostiene HTTPS/TLS. Permite verificar que un servidor es quien dice ser y cifrar la comunicación.
- **Algoritmos de hashing**: transformar datos (contraseñas, archivos) en una huella de tamaño fijo, de forma que no se pueda revertir. Fundamental distinguir hashing (contraseñas, con sal y algoritmos lentos a propósito como bcrypt/argon2) de cifrado (reversible, para datos que sí hace falta recuperar).
- **Firewalls**: controlan qué tráfico de red puede entrar o salir de un sistema, a nivel de red: la primera línea de defensa perimetral, complementaria (no sustituta) de la seguridad a nivel de aplicación.

Para el detalle práctico de cómo automatizar estos controles en un pipeline (escaneo de dependencias, contenedores, secretos, IaC), ver el [pequeño libro de DevSecOps](https://github.com/Fer3141/pequeno-libro-de-devsecops).

---

## Calidad y prácticas de ingeniería

- **Control de versiones (Git)**: la base de cualquier flujo de trabajo en equipo. Ver el [pequeño libro de Git](https://github.com/Fer3141/pequeno-libro-de-git).
- **TDD (desarrollo guiado por pruebas)**: escribir el test antes que el código que lo hace pasar. Más allá de la disciplina en sí, obliga a pensar el diseño de la interfaz de un componente antes de implementarlo.
- **Documentación de arquitectura**: registrar no solo el diseño final, sino las alternativas consideradas y por qué se descartaron con un **ADR** (ver la sección [Cómo tomar decisiones de arquitectura](#cómo-tomar-decisiones-de-arquitectura)). Así el equipo evita repetir discusiones ya cerradas o revertir una decisión sin entender el contexto que la motivó.
- **Herramientas y flujo de trabajo del equipo**: sistemas de tickets, wikis internas, herramientas de diagramación (UML y notaciones más livianas como C4). Un diagrama que nadie mantiene actualizado es peor que no tener diagrama, porque genera confianza falsa.

---

## Metodologías y marcos de trabajo

Un arquitecto no gestiona el día a día del equipo, pero sí necesita entender el marco de trabajo bajo el que se entrega el software, porque afecta directamente cómo y cuándo se pueden introducir cambios de diseño:

- **Ágiles**: **Scrum** (sprints con roles definidos), **Kanban** (flujo continuo con límites de trabajo en progreso), **XP** (Extreme Programming, con foco en prácticas técnicas como TDD y pair programming), **SAFe/LeSS** (frameworks para escalar ágil a muchos equipos coordinados).
- **Tradicionales/formales**: **RUP** (Rational Unified Process), **PRINCE2**, **PMI/PMBOK**, más comunes en organizaciones grandes o sectores regulados, con énfasis en documentación y fases formales.
- **ITIL**: marco de gestión de servicios de TI (incidentes, cambios, problemas), relevante para arquitectura cuando el sistema diseñado entra en operación y soporte continuo.
- **TOGAF**: el marco más usado específicamente para arquitectura **empresarial**, con un proceso formal (el ADM, *Architecture Development Method*) para llevar la arquitectura de una organización desde el estado actual al estado deseado.

---

## Habilidades blandas y liderazgo técnico

A partir de cierto nivel de seniority, estas habilidades pesan tanto como el conocimiento técnico:

- **Comunicación**: explicar una decisión de arquitectura a una audiencia técnica y a una no técnica (ejecutivos, product managers) requiere dos formas distintas de contar la misma historia: la habilidad central del rol.
- **Toma de decisiones bajo incertidumbre**: casi ninguna decisión arquitectónica tiene información completa; el trabajo es decidir con lo que hay, documentar los supuestos, y estar dispuesto a revisar la decisión cuando aparece información nueva.
- **Mentoría y consultoría interna (consult & coach)**: ayudar a que otros desarrolladores tomen mejores decisiones de diseño por sí mismos, en vez de convertirse en el único punto de aprobación de cada cambio, algo que no escala y crea un cuello de botella.
- **Estimación y evaluación**: dar una idea realista de esfuerzo y riesgo de una iniciativa de arquitectura, sabiendo que toda estimación de software es incertidumbre disfrazada de número.
- **Balance (technical vs. business trade-offs)**: la habilidad de traducir una restricción de negocio (presupuesto, plazo) en una restricción de diseño aceptable, y viceversa, explicando en términos de negocio por qué cierta decisión técnica cuesta lo que cuesta.

---

## Cómo seguir creciendo

- El roadmap interactivo original, con recursos enlazados por tema: https://roadmap.sh/software-architect
- **TOGAF** (marco de arquitectura empresarial): https://www.opengroup.org/togaf
- Practicar diseño real: participar en decisiones de arquitectura del equipo actual, escribir ADRs propios, y revisar diseños ajenos: el rol se aprende diseñando y recibiendo feedback, no solo leyendo.
- Profundizar en las áreas de este libro que tengan su propia guía dedicada: [Git](https://github.com/Fer3141/pequeno-libro-de-git), [AWS](https://github.com/Fer3141/pequeno-libro-de-aws), [Docker](https://github.com/Fer3141/pequeno-libro-de-docker), [Kubernetes](https://github.com/Fer3141/pequeno-libro-de-kubernetes), [Redis](https://github.com/Fer3141/redis_book), [Node.js](https://github.com/Fer3141/pequeno-libro-de-nodejs), [diseño de APIs](https://github.com/Fer3141/pequeno-libro-de-diseno-de-apis) y [DevSecOps](https://github.com/Fer3141/pequeno-libro-de-devsecops).
