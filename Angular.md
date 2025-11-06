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

