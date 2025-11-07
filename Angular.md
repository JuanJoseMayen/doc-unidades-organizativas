# Módulo Mantenimiento Unidades Organizativas - Documentación

## Descripción General

El módulo de **Unidades Organizativas** permite la gestion estructurada del organigrama institucional.
Incluye el mantenimiento (CRUD) de unidades, departamentos, ocupaciones, ocupaciones por departamento y plazas, brindando herramientas para gestionar su jerarquía, relaciones internas y datos asociados de forma centralizada.

### Características Principales
- Layout responsivo con sidebar colapsable
- Sistema de drawer para configuraciones
- Barra de estado con información de usuario, estación de trabajo y empresa
- Routing anidado para gestión de vistas
- Protección de rutas con guards

---

## Arquitectura del Módulo

El módulo sigue una arquitectura basada en componentes con un layout principal que encapsula todas las vistas secundarias:

```
LayoutUnidadesOrganizativas (Contenedor Principal)
├── Sidebar (Navegación lateral)
├── Header (Encabezado del módulo)
├── Settings Drawer (Panel de configuración)
├── Router Outlet (Vistas dinámicas)
└── Footer/Barra de Estado
```

---
## Componentes Principales

### LayoutUnidadesOrganizativasComponent

**Descripción:**  
Componente contenedor principal del módulo Mantenimiento de Unidades Organizativas. Se encarga de mantener todo el contenido del módulo y se renderiza según el display seleccionado por el usuario.

#### Propiedades

| Propiedad | Tipo | Descripción |
|-----------|------|-------------|
| `headerText` | `string` | Texto del encabezado del módulo |
| `usuario` | `string` | Nombre del usuario actual |
| `estacionTrabajo` | `string` | Descripción de la estación de trabajo |
| `empresa` | `string` | Nombre de la empresa |
| `drawerVisible` | `boolean` | Controla la visibilidad del drawer de configuraciones |

### MenuUnidadesComponent

**Descripción:**  
Funciona como el panel principal de navegación dentro del módulo de Unidades Organizativas.
Desde este menú, el usuario puede acceder fácilmente a los distintos submódulos del sistema, tales como:
- Unidades
- Departamentos
- Ocupaciones
- Ocupaciones por Departamento
- Plazas

#### Componentes y Módulos Relacionados

- ListaUnidadesComponent → Mantenimiento CRUD de unidades.
- ListaDepartamentosComponent → Mantenimiento CRUD de departamentos.
- ListaOcupacionesComponent → Mantenimiento CRUD de ocupaciones.
- ListaOcupacionDeptoComponent → Gestión de la relación entre ocupaciones y departamentos.
- ListaPlazasComponent → Administración de plazas.

### OrganigramaComponent

**Descripción:**  
Este componente representa la vista principal del organigrama completo.
Su objetivo es renderizar la estructura jerárquica completa de las unidades organizativas, departamentos, ocupaciones y plazas dentro de la institución.

Actúa como un contenedor visual, ya que no posee mucha lógica interna; más bien delega la gestión de datos y la construcción jerárquica al componente centralizado <app-organigrama-view>, que es el encargado de renderizar dinámicamente los niveles del organigrama.

#### Propiedades
| Propiedad                        | Tipo                                | Descripción                                                             |
| -------------------------------- | ----------------------------------- | ----------------------------------------------------------------------- |
| `mostrarFormulario`              | `boolean`                           | Controla si el formulario de edición se muestra o no.                   |
| `accionFormulario`               | `'agregar' \| 'actualizar' \| null` | Define la acción actual del formulario (agregar o actualizar un nodo).  |
| `nodoSeleccionado`               | `any`                               | Nodo sobre el cual se realiza la acción.                                |
| `estructuraOrganizativa`         | `OrganigramaNodo[]`                 | Estructura jerárquica de unidades, departamentos, ocupaciones y plazas. |
| `cargandoEstructuraOrganizativa` | `boolean`                           | Indica si la estructura se está cargando.                               |
#### Flujo general
1. Al iniciar (ngOnInit), se llama a obtenerEstructuraOrganizativa().
2. Se muestra el spinner de carga mientras se consultan los datos.
3. Una vez obtenidos, se renderiza la estructura en <app-organigrama-view>.

### OrganigramaUnidadesComponent

**Descripción:**  
Componente encargado de la gestión CRUD de las Unidades Organizativas.
Permite crear, editar y visualizar las diferentes unidades que conforman la estructura base del organigrama.

#### Responsabilidades principales
- Mostrar el árbol jerárquico de las unidades organizativas.
- Permitir agregar y actualizar unidades dentro de la estructura.
- Manejar eventos de apertura/cierre de formularios.
- Sincronizar la vista con el backend a través del servicio UnidadesOrganizativasService.

#### Propiedades
| Propiedad            | Tipo                                | Descripción                                 |
| -------------------- | ----------------------------------- | ------------------------------------------- |
| `estructuraUnidades` | `any[]`                             | Estructura jerárquica de las unidades.      |
| `mostrarFormulario`  | `boolean`                           | Controla la visualización del formulario.   |
| `accionFormulario`   | `'agregar' \| 'actualizar' \| null` | Define la acción actual sobre la unidad.    |
| `nodoSeleccionado`   | `any`                               | Unidad seleccionada para edición o adición. |

#### Flujo general
1. Carga inicial de las unidades desde el servicio.
2. Renderizado del organigrama correspondiente a las unidades.
3. Al seleccionar una unidad, se abre el formulario de edición.
4. Tras guardar, se actualiza la estructura visual.

### OrganigramaDepartamentosComponent

**Descripción:**  
Encargado de gestionar los departamentos pertenecientes a las unidades organizativas.
Opera con la misma estructura y lógica que el módulo de unidades, utilizando el mismo sistema CRUD y de renderizado jerárquico.

#### Responsabilidades principales
- Mostrar los departamentos asociados a cada unidad organizativa.
- Permitir crear y actualizar departamentos.
- Reutilizar la vista de organigrama centralizada para mantener coherencia visual.
- 
#### Propiedades
| Propiedad                 | Tipo                                | Descripción                                     |
| ------------------------- | ----------------------------------- | ----------------------------------------------- |
| `estructuraDepartamentos` | `any[]`                             | Lista o estructura jerárquica de departamentos. |
| `mostrarFormulario`       | `boolean`                           | Controla si se muestra el formulario.           |
| `accionFormulario`        | `'agregar' \| 'actualizar' \| null` | Define la acción activa.                        |
| `nodoSeleccionado`        | `any`                               | Departamento seleccionado para acción.          |

#### Flujo general
1. Carga de departamentos asociados a las unidades.
2. Renderizado de la estructura mediante el componente visual compartido.
3. Edición o adición de departamentos según la acción seleccionada.

### OrganigramaOcupacionComponent

**Descripción:**  
Este componente maneja la administración de ocupaciones dentro de cada departamento.
Sigue el mismo patrón lógico y estructural del módulo, utilizando la jerarquía común del organigrama para mostrar los niveles de ocupación.

#### Responsabilidades principales
- Gestionar el catálogo de ocupaciones.
- Permitir agregar desde cero y actualizar registros.
- Integrarse al sistema jerárquico para mantener consistencia visual.

#### Propiedades
| Propiedad           | Tipo                                | Descripción                                     |
| ------------------- | ----------------------------------- | ----------------------------------------------- |
| `listaOcupaciones`  | `any[]`                             | Registros de ocupaciones cargados desde la API. |
| `mostrarFormulario` | `boolean`                           | Indica si el formulario está visible.           |
| `accionFormulario`  | `'agregar' \| 'actualizar' \| null` | Acción en curso.                                |
| `nodoSeleccionado`  | `any`                               | Ocupación actual en edición.                    |


#### Flujo general
1. Carga inicial de ocupaciones desde la API.
2. Renderizado del organigrama visual.
3. Acciones CRUD según interacción del usuario.

### ListaOcupacionDeptoComponent

**Descripción:**  
Componente encargado de mostrar y gestionar la relación entre ocupaciones y departamentos.
En lugar de renderizar una jerarquía, utiliza el componente centralizado <app-list-items> para mostrar los registros en formato de lista, conservando las funcionalidades CRUD.

#### Responsabilidades principales
- Listar las relaciones ocupación–departamento.
- Permitir alta, modificación y eliminación de registros.
- Reutilizar la lógica de mantenimiento general del módulo.
  
#### Propiedades
| Propiedad                    | Tipo                                | Descripción                                     |
| ---------------------------- | ----------------------------------- | ----------------------------------------------- |
| `ocupacionDepto`             | `OcupacionDepto[]`                  | Lista de registros cargados desde el backend.   |
| `ocupacionDeptoSeleccionada` | `OcupacionDepto \| null`            | Registro actual seleccionado.                   |
| `mostrarFormulario`          | `boolean`                           | Controla la visibilidad del formulario.         |
| `accionFormulario`           | `'agregar' \| 'actualizar' \| null` | Define la acción en curso.                      |
| `isVisibleModal`             | `boolean`                           | Muestra el modal de confirmación al eliminar.   |
| `isVisibleExito`             | `boolean`                           | Controla la visualización de mensajes de éxito. |

### ListaPlazasComponent

**Descripción:**  
Gestiona el catálogo de plazas laborales dentro de la estructura organizativa.
También utiliza el componente reutilizable <app-list-items>, que centraliza la visualización y el sistema CRUD, adaptado a las propiedades específicas de las plazas.

#### Responsabilidades principales
- Mostrar la lista de plazas disponibles.
- Permitir agregar, editar o eliminar plazas.
- Integrar el mantenimiento al flujo general del organigrama.
  
#### Propiedades
| Propiedad           | Tipo                                | Descripción                                               |
| ------------------- | ----------------------------------- | --------------------------------------------------------- |
| `plaza`             | `Plaza[]`                           | Lista de plazas cargadas desde el backend.                |
| `plazaSeleccionada` | `Plaza \| null`                     | Registro actual en edición o eliminación.                 |
| `mostrarFormulario` | `boolean`                           | Controla la visibilidad del formulario de mantenimiento.  |
| `accionFormulario`  | `'agregar' \| 'actualizar' \| null` | Indica la acción que se está realizando.                  |
| `cargandoPlazas`    | `boolean`                           | Indica si los datos se están obteniendo desde el backend. |
| `isVisibleModal`    | `boolean`                           | Controla la visibilidad del modal de confirmación.        |
| `isVisibleExito`    | `boolean`                           | Muestra mensajes de éxito tras operaciones CRUD.          |


### Formularios del Módulo de Unidades Organizativas
El módulo cuenta con un conjunto de formularios modales reutilizables, diseñados para manejar las operaciones CRUD (Crear, Leer, Actualizar y Eliminar) de cada entidad del sistema.
Estos formularios corresponden a las siguientes entidades:

- FormUnidadesComponent
- FormDepartamentosComponent
- FormOcupacionComponent
- FormOcupacionDeptoComponent
- FormPlazasComponent

A nivel funcional, todos comparten la misma lógica de operación, diferenciándose únicamente en las propiedades, modelos de datos y servicios que consumen.

#### Lógica compartida
Cada formulario se implementa siguiendo un patrón estructural similar, garantizando consistencia y mantenibilidad entre entidades.

| **Aspecto**                             | **Descripción general**                                                                                                                                                                    |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Estructura base**                     | Todos los formularios contienen variables para controlar el estado del formulario (`mostrarFormulario`, `accionFormulario`, `elementoSeleccionado`, etc.).                                 |
| **Acciones principales (CRUD)**         | Se implementan los métodos `onAgregar`, `onActualizar`, `onEliminar` y `onCerrarFormulario`, los cuales manejan las operaciones básicas de mantenimiento sobre la entidad correspondiente. |
| **Interacción con servicios**           | Cada formulario utiliza el servicio `UnidadesOrganizativasService`, con sus métodos específicos (`getX`, `addX`, `updateX`, `deleteX`) según la entidad que representa.                    |
| **Manejo de estados y carga**           | Se controlan indicadores visuales como `cargando`, `isVisibleModal`, `isVisibleExito` y `mensajeExito` para mejorar la experiencia de usuario.                                             |
| **Mensajes de confirmación y éxito**    | Todos los formularios implementan métodos estándar como `manejarMensajeExito()` y `mostrarModalConfirmacion()` para feedback al usuario.                                                   |
| **Comunicación con componentes padres** | Emisión de eventos mediante `@Output()` para notificar al componente padre (por ejemplo, `OrganigramaXComponent` o `ListaXComponent`) sobre actualizaciones o cierres.                     |
| **Reutilización visual**                | Se usan componentes comunes como `app-spinner-carga`, `app-modal-confirmacion` y `app-formulario-base`, manteniendo una interfaz coherente.                                                |


### Servicios del Módulo de Unidades Organizativas
El módulo cuenta con un servicio centralizado llamado UnidadesOrganizativasService, encargado de manejar todas las peticiones HTTP hacia la API relacionadas con las diferentes entidades que conforman la estructura organizativa: Unidades, Departamentos, Ocupaciones, Ocupaciones por Departamento y Plazas.

Este servicio actúa como capa de comunicación entre el frontend y el backend.
Cada grupo de métodos implementa las operaciones CRUD (crear, leer, actualizar y eliminar) para cada entidad del módulo, siguiendo una estructura estandarizada que permite mantener el código limpio, reutilizable y de fácil mantenimiento.

#### Estructura de los Métodos

Cada conjunto de métodos sigue la misma estructura:

- **`getX()`** → Obtiene todos los registros.  
- **`addX(model: any)`** → Agrega un nuevo registro.  
- **`updX(model: any)`** → Actualiza un registro existente.  
- **`delX(id: number | model: any)`** → Elimina un registro específico.  

---
#### Funcionalidad Común de los Métodos

Cada método realiza las siguientes tareas:

1. **Construye la URL** del endpoint correspondiente.  
2. **Incluye el token JWT** en los encabezados de la solicitud.  
3. **Efectúa la petición HTTP** (`GET` o `POST`).  
4. **Aplica operadores RxJS** (`map`, `catchError`) para procesar la respuesta o manejar errores.  
5. En caso de error, **retorna un objeto estructurado** con información del fallo:
   - Mensaje descriptivo.  
   - Nombre del procedimiento o acción.  
   - Marca de tiempo (`timestamp`).
     
#### Secciones Principales del Servicio
#### 🔹 Mantenimiento de Unidades Organizativas

Incluye métodos que permiten la **gestión completa de las unidades** dentro del organigrama:

- **`getUnidadesOrganizativas()`** → Obtiene todas las unidades organizativas.  
- **`addUnidadOrganizativa(model)`** → Agrega una nueva unidad.  
- **`updUnidadOrganizativa(model)`** → Actualiza una unidad existente.  
- **`delUnidadOrganizativa(idUnidad)`** → Elimina una unidad específica.  

---

#### 🔹 Mantenimiento de Departamentos

Se encarga del **CRUD de departamentos** vinculados a las unidades organizativas:

- **`getDepartamento()`** → Obtiene todos los departamentos.  
- **`addDepartamento(model)`** → Crea un nuevo departamento.  
- **`updDepartamento(model)`** → Modifica un departamento existente.  
- **`delDepartamento(pDepartamento)`** → Elimina un departamento determinado.  

---

#### 🔹 Mantenimiento de Ocupaciones

Define las operaciones para **gestionar los cargos u ocupaciones** existentes:

- **`getOcupacion()`** → Recupera todas las ocupaciones registradas.  
- **`addOcupacion(model)`** → Agrega una nueva ocupación.  
- **`updOcupacion(model)`** → Actualiza los datos de una ocupación.  
- **`delOcupacion(pOcupacion)`** → Elimina una ocupación específica.  

---

#### 🔹 Mantenimiento de Ocupaciones por Departamento

Permite **vincular las ocupaciones con departamentos específicos** dentro de la estructura:

- **`getOcupacionDepto()`** → Obtiene las ocupaciones asociadas a cada departamento.  
- **`addOcupacionDepto(model)`** → Crea una nueva relación ocupación–departamento.  
- **`updOcupacionDepto(model)`** → Modifica una vinculación existente.  
- **`delOcupacionDepto(model)`** → Elimina una relación registrada.  

---

#### 🔹 Mantenimiento de Plazas

Gestiona el **número y detalle de plazas asignadas** a las distintas áreas:

- **`getPlaza()`** → Obtiene todas las plazas registradas.  
- **`addPlaza(model)`** → Agrega una nueva plaza.  
- **`updPlaza(model)`** → Actualiza la información de una plaza existente.  
- **`delPlaza(pPlaza)`** → Elimina una plaza específica.  

---

#### 🔹 Estructura Organizativa Completa

Método especializado para **obtener la estructura jerárquica completa** del organigrama institucional:

- **`getEstructuraOrganizativa()`** → Devuelve la jerarquía completa de unidades, departamentos, ocupaciones y plazas.  

---

#### Manejo de Errores

Todos los métodos implementan una **gestión uniforme de errores** mediante `catchError`.  
En caso de fallo en la petición, se retorna un objeto con la siguiente estructura:

```typescript
{
  message: 'Descripción del error',
  error: 'Tipo o causa principal',
  procedimiento: 'Procedimiento almacenado afectado',
  timestamp: 'Fecha y hora del incidente'
}
