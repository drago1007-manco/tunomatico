# Sistema de Gestión de Turnos Digitales (Tunomático)

## ✅ Descripción General del Sistema
El presente proyecto corresponde al modelado arquitectónico de un Sistema de Gestión de Turnos Digitales (Tunomático), desarrollado con el objetivo de optimizar la administración de atención de clientes en instituciones públicas y privadas mediante la automatización del proceso de asignación y control de turnos.

El sistema permite que los usuarios soliciten turnos de manera digital, seleccionen servicios específicos, reciban notificaciones y visualicen el estado de atención en tiempo real. Además, proporciona herramientas administrativas para la gestión de usuarios, servicios, reportes y monitoreo general del flujo de atención.

La solución fue diseñada utilizando principios de programación orientada a objetos y patrones de diseño reconocidos, buscando garantizar:

escalabilidad,
mantenibilidad,
reutilización de componentes,
desacoplamiento arquitectónico,
facilidad de integración con servicios externos.

El modelado contempla tanto la visión funcional del sistema mediante diagramas UML, como también su representación lógica y física, reflejando una arquitectura robusta y organizada por capas.

---

## 🔍 Objetivos del Modelado
- Identificar los actores y funcionalidades principales del sistema.
- Representar las relaciones entre usuarios y procesos mediante diagramas de casos de uso.
- Diseñar la estructura lógica de clases aplicando patrones de diseño orientados a buenas prácticas de software.
- Organizar el sistema siguiendo una arquitectura modular y desacoplada.
- Modelar la infraestructura física necesaria para la ejecución del sistema.
- Facilitar la comprensión, mantenimiento y futura escalabilidad del software.
- Aplicar patrones de diseño GOF para mejorar reutilización y flexibilidad arquitectónica.

---

## 🔹 1. Diagrama de Casos de Uso UML
<img width="1216" height="961" alt="444632011-0d7847bb-b6b0-468a-aecc-8855e2240684" src="https://github.com/drago1007-manco/tunomatico/blob/main/caso%20de%20uso%20uml.png" />




### Descripción general
El análisis funcional permitió identificar con claridad los actores involucrados y las funcionalidades críticas del sistema. Además, se aplicaron correctamente **relaciones de `<<include>>` y `<<extend>>`** para reflejar flujos obligatorios y opcionales en el proceso.

#### Actores identificados:
- **Cliente**: Representa al usuario final que solicita atención mediante el sistema.
- **Recepcionista**: Es el encargado de administrar la atención presencial y controlar el flujo de turnos dentro de la institución.
- **Administrador**: Actor con privilegios avanzados sobre el sistema.
- **Sistema de Pantalla**: Corresponde al módulo encargado de mostrar visualmente los turnos llamados y la información de atención en tiempo real.
- **Servicio Externo de Notificaciones**: Representa plataformas externas utilizadas para el envío de mensajes SMS o correos electrónicos a los clientes.

#### Casos de uso destacados y relaciones aplicadas:
- **Solicitar Turno**
  - `<<include>>` **Seleccionar Servicio**: para generar un turno es obligatorio seleccionar previamente el tipo de atención requerida.
  - `<<include>>` **Validar Disponibilidad**: el sistema debe verificar obligatoriamente la disponibilidad de atención antes de asignar el turno.
  - `<<include>>` **Registrar Cliente Preferencial**: opcionalmente puede identificarse al cliente como preferencial para otorgar prioridad de atención.
- **Llamar Turno**
  - `<<include>>` **Mostrar Turno en Pantalla**: cada vez que se llama un turno, este debe visualizarse obligatoriamente en las pantallas del sistema.
  - `<<include>>` **Enviar Notificación**: el sistema puede notificar automáticamente al cliente mediante SMS o correo electrónico.
- **Cancelar Turno**
  - `<<extend>>` **Enviar Notificación**: opcionalmente el sistema puede informar al cliente sobre la cancelación realizada.
  - `<<extend>>` **Generar Registro de Cancelación**: el sistema puede almacenar un registro adicional para fines administrativos y de auditoría.
- **Reagendar Turno**:
  - `<<extend>>` **Solicitar Turno**: el reagendamiento reutiliza parte del flujo de creación de turnos, pero ocurre únicamente cuando el cliente desea modificar su atención previamente registrada.

#### Justificación de las relaciones aplicadas:
Se utilizaron relaciones `<<include>>` en funcionalidades donde el caso de uso principal depende obligatoriamente de procesos secundarios para completarse correctamente, como ocurre en:

validación de disponibilidad,
generación de tickets,
visualización de turnos,
consulta de historiales.

Estas funcionalidades forman parte esencial del flujo principal y siempre deben ejecutarse.

Las relaciones `<<extend>>` fueron aplicadas en funcionalidades opcionales o condicionadas por determinadas situaciones del sistema o decisiones del usuario, como:

registrar clientes preferenciales,
cancelar turnos,
reagendar atenciones,
generar registros administrativos adicionales.

Estas acciones complementan el comportamiento principal, pero no son obligatorias en todos los escenarios.

La utilización de `<<include>>` y `<<extend>>` permite:
mejorar la modularidad del sistema,
representar de manera más clara las dependencias funcionales,
evitar duplicidad de procesos,
facilitar el mantenimiento y escalabilidad del modelo UML.

#### Relación destacada:
Solicitar Turno `<<include>>` Validar Disponibilidad

Esta relación es una de las más importantes del sistema debido a que garantiza la consistencia y disponibilidad de atención antes de emitir un turno.

Sin esta validación podrían producirse:

sobrecarga de atención,
duplicidad de turnos,
conflictos de horarios,
errores operacionales.


## 🔹 2. Diagrama de Clases UML con Patrones Aplicados
<img width="3551" height="1172" alt="444619757-ef4a83da-4246-44e8-9b8a-0de62471b0a7" src="https://github.com/user-attachments/assets/0ef32c13-22b8-4aca-99b8-d022ddcfa93f" />



## 🧩 Justificación Arquitectónica y Patrones Aplicados

### Selección de patrones
La elección de los patrones de diseño no fue arbitraria, sino estratégica y alineada a las necesidades específicas del sistema y sus desafíos técnicos:

### **1. Singleton (`ConfiguracionSistema`)**
#### Justificación:
Se seleccionó Singleton para la **gestión centralizada de parámetros críticos del sistema**, como tiempos de vencimiento, stock mínimo, tipos de alerta, entre otros.  
Este patrón permite garantizar que **exista una única instancia accesible globalmente**, evitando inconsistencias y facilitando la administración de la configuración desde cualquier módulo del sistema.

#### Intención arquitectónica:
- Centralizar el control de la configuración.
- Facilitar la escalabilidad futura permitiendo la consulta distribuida.
- Evitar múltiples puntos de configuración que pudieran provocar errores operacionales.

---

### **2. Prototype (`PlantillaMovimiento`)**
#### Justificación:
La operación hospitalaria exige rapidez y eficiencia en la gestión de movimientos repetitivos.  
Se implementó Prototype para **permitir la clonación de plantillas de movimientos frecuentes**, como egresos de insumos comunes o ingresos masivos programados, permitiendo a los operadores agilizar las operaciones sin recrear movimientos desde cero.

#### Intención arquitectónica:
- Reducir la complejidad y tiempo de operaciones manuales.
- Permitir la creación rápida de nuevas instancias de movimientos desde plantillas base, manteniendo flexibilidad y control.
- Facilitar la parametrización de campañas o protocolos especiales (ej.: vacunación masiva).

---

### **3. Adapter (`AdaptadorERP`)**
#### Justificación:
Dado que el sistema debe integrarse con el ERP hospitalario, el uso de Adapter fue clave para **desacoplar el núcleo del sistema de la API del ERP externo**, permitiendo mantener la flexibilidad en caso de cambios o actualizaciones en el sistema ERP.

#### Intención arquitectónica:
- Asegurar la independencia tecnológica del sistema interno.
- Facilitar el mantenimiento y evolución del sistema de integración.
- Permitir la adaptación a distintos sistemas externos (ERP, contabilidad, etc.) sin impactar el dominio.

---

### **4. Bridge (`InterfazUsuario` + `VistaBodeguero`, `VistaSupervisor`)**
#### Justificación:
Considerando la diversidad de usuarios y dispositivos (bodeguero móvil, supervisor en escritorio, etc.), se aplicó Bridge para **separar la interfaz de usuario de la lógica de negocio**, permitiendo adaptar la experiencia según el perfil del usuario y el dispositivo utilizado, sin afectar la lógica central del sistema.

#### Intención arquitectónica:
- Flexibilizar las vistas según necesidades operativas.
- Garantizar independencia entre interfaz y lógica.
- Facilitar futuras integraciones con nuevas plataformas (app, web, terminal de autoservicio).

---

## 🔹 3. Diagrama de Implementación UML
<img width="2862" height="478" alt="444619988-e78fb473-d493-4c4a-b916-c470f34e63e7" src="https://github.com/user-attachments/assets/6b90e530-cce8-4858-8dbb-f638a0d439ee" />



### Despliegue Físico y decisiones técnicas:
- **Nodos físicos diferenciados** para reforzar seguridad, escalabilidad y disponibilidad.
- Separación clara de responsabilidades entre servidores de aplicaciones, configuración, integración ERP, y bases de datos.
- Uso de protocolos estándar (REST, SOAP) en las integraciones externas.
- Aislamiento de componentes críticos (como `ConfiguracionSistema`) en nodos dedicados para reforzar el control de cambios y configuración.

---

## 🧩 Reflexiones Finales del Modelado

Este ejercicio refleja una aproximación arquitectónica profesional donde:
- Cada patrón fue seleccionado según necesidades específicas y no como elemento decorativo.
- La transición entre **caso de uso ➡ diagrama de clases ➡ implementación** permitió mantener una trazabilidad clara desde el negocio hasta la infraestructura.
- La modularización y aplicación de patrones permitieron diseñar un sistema robusto, flexible, mantenible y alineado a buenas prácticas de ingeniería de software.

Este repositorio tiene como propósito servir de **referencia formal para futuros trabajos de modelado arquitectónico**, mostrando estándares exigidos en entornos profesionales.

---

## ⚠️ Nota
Este repositorio es exclusivamente documental.  
No se incluye código fuente, ya que el foco es el modelado arquitectónico.
