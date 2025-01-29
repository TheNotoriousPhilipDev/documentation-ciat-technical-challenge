# Sistema de Monitoreo de Actividades Agronómicas

## Alcance de la solución

El sistema de monitoreo de actividades agronómicas permitirá a los campesinos registrar y monitorear sus actividades agrícolas, facilitando la toma de decisiones informadas basadas en datos recopilados. La solución consistirá en una API RESTful que podrá ser consumida posteriormente por clientes como React.js o Angular. Se abordó el problema mediante dos microservicios que se comunican entre sí utilizando peticiones HTTP bajo una arquitectura REST.

Para el desarrollo del backend, se utilizó Java 21 con Amazon Corretto, lo que facilita el despliegue en entornos en la nube, como AWS. Cada microservicio gestionará su propia persistencia de datos de forma independiente mediante MySQL 8. Los microservicios se desplegarán en contenedores Docker para facilitar su implementación y escalabilidad.

## Requerimientos debidamente clasificados

### Requerimientos funcionales:

#### Gestión de Actividades Agronómicas:
- Obtener todas las actividades agronómicas registradas.
- Obtener una actividad agronómica específica por su ID.
- Crear nuevas actividades agronómicas, incluyendo detalles como la fecha, insumos, tipo de actividad y duración.
- Actualizar actividades agronómicas existentes.
- Eliminar actividades agronómicas.
- Obtener actividades agronómicas asociadas a parcelas específicas.

#### Gestión de Parcelas de la Finca:
- Listar todas las parcelas disponibles.
- Obtener una parcela específica por su ID.
- Crear nuevas parcelas con información como nombre, ubicación (latitud y longitud), tipo de cultivo, y tamaño en hectáreas.
- Actualizar la información de parcelas existentes.
- Eliminar parcelas de la base de datos.

#### Relación entre Actividades y Parcelas:
- Asignar actividades agronómicas a parcelas específicas.
- Crear nuevas actividades y asignarlas a parcelas de forma simultánea.
- Eliminar actividades asignadas a una parcela.
- Eliminar una actividad específica de una parcela.

### Requerimientos no funcionales:
- **Escalabilidad**: El sistema debe poder escalar para gestionar múltiples parcelas y actividades.
- **Despliegue con Docker**: Los microservicios deben estar contenizados para permitir un despliegue fácil y rápido.
- **Tolerancia a fallos**: Los microservicios deben ser independientes y auto-recuperables en caso de fallos.
- **Autenticación y Autorización**: Solo los usuarios autenticados deben tener acceso a las funciones de gestión.
- **Facilidad de uso**: La API debe seguir principios REST para facilitar la interacción desde interfaces frontend o aplicaciones móviles.

### Requerimientos técnicos:
- APIs **RESTful** para cada uno de los microservicios.
- **Persistencia de datos** en bases de datos MySQL 8 independientes para cada microservicio (Actividades y Parcelas).
- Uso de **Java 21 con Amazon Corretto** como base del backend.
- Comunicación entre microservicios utilizando **Feign** (para la interacción entre los servicios de parcelas y actividades).
- **Validación y manejo de errores** en las interacciones entre microservicios (usando `FeignException` para manejar errores en el servicio proxy).

## Arquitectura de software de la solución propuesta

La arquitectura propuesta sigue un enfoque de **microservicios** desacoplados para gestionar las actividades agronómicas y las parcelas de la finca de forma independiente. Cada microservicio tiene su propio almacenamiento en una base de datos MySQL 8. La comunicación entre microservicios se realiza a través de **APIs RESTful**, usando **Feign** para la interacción entre ellos.

- **Microservicio de Actividades Agronómicas**: Responsable de la gestión de actividades agronómicas (creación, actualización, eliminación y consulta).
- **Microservicio de Parcelas**: Responsable de la gestión de parcelas (creación, actualización, eliminación y consulta) y de la asignación de actividades a parcelas.
- **Comunicación**: Los microservicios se comunican a través de peticiones HTTP y manejan errores utilizando `FeignException`.
- **Contenedores Docker**: Ambos microservicios están empaquetados como imágenes Docker para facilitar el despliegue y la escalabilidad en entornos de nube como AWS.

## Modelo de la base de datos

Cada microservicio tiene su propia base de datos **MySQL 8** independiente. A continuación, se describe el modelo de datos básico para cada uno:

- **Microservicio de Actividades**:
  - `activities`: 
    - `id` (PK), 
    - `name`, 
    - `date`, 
    - `activity_type`, 
    - `activity_duration`, 
    - `agronomic_inputs_list`.

- **Microservicio de Parcelas**:
  - `farm_plots`: 
    - `id` (PK),
    - `name`, 
    - `latitude`, 
    - `longitude`, 
    - `crop_type`, 
    - `hectare_size`.

Ambas bases de datos se mantienen separadas para asegurar la independencia y evitar el acoplamiento.

## Sistema de información que ayude a la solución

El sistema de información está basado en **APIs RESTful** que permiten a los usuarios (campesinos o administradores) interactuar con el backend para gestionar las actividades y parcelas. Estas APIs son fácilmente consumibles por aplicaciones frontend como **React.js** o **Angular**, ofreciendo las siguientes funcionalidades:

- Registro, actualización, consulta y eliminación de actividades agronómicas.
- Gestión de parcelas y sus detalles geoespaciales.
- Relación entre actividades y parcelas, permitiendo la asignación y desasignación de actividades.
- Fácil integración con aplicaciones móviles o web mediante llamadas API REST.

La API está diseñada para ser escalable y eficiente, con soporte para despliegue en contenedores Docker.
