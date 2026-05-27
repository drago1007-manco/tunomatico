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
<img width="3551" height="1172" alt="444619757-ef4a83da-4246-44e8-9b8a-0de62471b0a7" src="https://github.com/drago1007-manco/tunomatico/blob/main/diagrama%20de%20clases.png" />



## 🧩 Justificación Arquitectónica y Patrones Aplicados

### Selección de patrones
La elección de los patrones de diseño no fue arbitraria, sino estratégica y alineada a las necesidades específicas del sistema y sus desafíos técnicos:

### **1. Singleton**
#### Justificación:
El patrón Singleton fue implementado en la clase GestorConexion, permitiendo centralizar y controlar el acceso único a la conexión de base de datos.

Su utilización evita:
  - múltiples conexiones innecesarias
  - consumo excesivo de recursos
  - inconsistencias de acceso

#### Intención arquitectónica:
- Centralizar el control de la configuración.
- Facilitar la escalabilidad futura permitiendo la consulta distribuida.
- Evitar múltiples puntos de configuración que pudieran provocar errores operacionales.

---

### **2. Prototype**
#### Justificación:
El patrón Prototype fue aplicado mediante la clase TurnoPrototype, permitiendo clonar configuraciones base de turnos sin recrear objetos complejos desde cero.

Esto mejora:
 - rendimiento
 - reutilización de estructuras
 - flexibilidad para generar distintos tipos de turnos.

---

### **3. Adapter**
#### Justificación:
El patrón Adapter fue utilizado para integrar servicios externos de mensajería SMS que poseen interfaces incompatibles con el sistema interno.

La clase AdaptadorSMS actúa como intermediario entre el sistema Tunomático y el proveedor externo de notificaciones.

Esto permite:
 - desacoplar integraciones externas
 - facilitar cambios futuros de proveedor
 - mantener estabilidad del sistema

---

### **4. Bridge**
#### Justificación:
El patrón Bridge fue implementado en el sistema de notificaciones para desacoplar los tipos de notificación de los canales de envío.

Gracias a esta estructura, el sistema puede enviar mensajes mediante:
 - SMS
 - correo electrónico
 - futuros canales adicionales

---

## 🔹 3. Diagrama de Implementación UML
<img width="1089" height="496" alt="image" src="https://github.com/user-attachments/assets/b70c0542-76b2-4d0f-96d2-6162cd389915" />

### Despliegue Físico y decisiones técnicas:
El sistema Tunomático fue diseñado bajo una arquitectura cliente-servidor distribuida, compuesta por terminales de atención, servidor de aplicaciones, base de datos centralizada y servicios externos de notificación.

Los clientes y recepcionistas acceden al sistema mediante interfaces web o terminales conectadas al servidor principal, donde se ejecuta la lógica de negocio y la administración de turnos.

La información se almacena en una base de datos centralizada para garantizar consistencia, seguridad y acceso concurrente. Además, el sistema se integra con servicios externos de SMS y correo electrónico mediante API REST para el envío de notificaciones automáticas.

Entre las principales decisiones técnicas destacan:
 - uso de arquitectura por capas para separar responsabilidades,
 - implementación de patrones de diseño para mejorar modularidad y mantenimiento,
 - centralización de conexiones mediante <<Singleton>>,
 - integración desacoplada con servicios externos utilizando <<Adapter>>,
 - soporte para múltiples canales de notificación mediante <<Bridge>>.

Esta estructura permite que el sistema sea escalable, mantenible y preparado para futuras ampliaciones funcionales.

---

## 🧩 Reflexiones Finales del Modelado

El desarrollo del modelado UML para el sistema Tunomático permitió comprender la importancia de una correcta planificación arquitectónica antes de la implementación del software.

La utilización de diagramas UML facilitó:
 - visualizar responsabilidades,
 - identificar dependencias,
 - organizar componentes,
 - anticipar problemas de diseño.

La aplicación de patrones de diseño GOF permitió construir una arquitectura más flexible, reutilizable y mantenible, alineada con buenas prácticas de ingeniería de software.

Asimismo, la separación por capas y el uso de componentes desacoplados favorecen la futura evolución del sistema, permitiendo incorporar nuevas funcionalidades sin afectar significativamente la estructura existente.

Finalmente, el modelado realizado demuestra cómo UML y los patrones de diseño constituyen herramientas fundamentales para desarrollar sistemas escalables, organizados y preparados para escenarios reales de producción.



