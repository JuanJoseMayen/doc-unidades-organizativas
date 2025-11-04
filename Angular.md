# Módulo MAntenimiento Unidades Organizativas - Documentación

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
