# Documentación del Modelo de Datos

Este diagrama representa la estructura de usuarios, permisos y la logística electoral.

```mermaid
erDiagram
    %% Módulo de Seguridad y Usuarios
    USERS ||--o{ USER_ROLES : has
    ROLES ||--o{ USER_ROLES : assigned_to
    ROLES ||--o{ ROLE_PERMISSIONS : contains
    PERMISSIONS ||--o{ ROLE_PERMISSIONS : granted_to

    %% Módulo de Personas y Contacto
    PERSONA ||--o{ TELEFONO : tiene
    PERSONA ||--o{ EMAIL : tiene
    PERSONA ||--o{ DIRECCION : reside_en
    PERSONA }|--|| GENERO : pertenece
    
    %% Módulo Político
    PERSONA }|--|| PARTIDO_POLITICO : milita
    PERSONA }|--|| CANDIDATO : apoya
    CANDIDATO }|--|| PARTIDO_POLITICO : pertenece_a

    %% Módulo Electoral Ajustado
    PERSONA }|--|| MESA : vota_en
    MESA }|--|| PUESTO_VOTACION : ubicada_en
    PUESTO_VOTACION }|--|| ZONA_ELECTORAL : pertenece_a
    ZONA_ELECTORAL }|--|| CIUDAD : asignada_a

    %% Módulo Geográfico
    DIRECCION }|--|| CIUDAD : ubicada_en
    CIUDAD }|--|| DEPARTAMENTO : pertenece_a
    DEPARTAMENTO }|--|| PAIS : pertenece_a

    USERS {
        int id PK
        string username
        string email
        string password
        datetime created_at
    }

    ROLES {
        int id PK
        string name
        string description
    }

    PERMISSIONS {
        int id PK
        string name
        string module
        string description
    }

    PERSONA {
        int id PK
        string nombre_completo
        string identificacion_oficial UK
        int edad
        int genero_id FK
        int mesa_id FK
        int partido_id FK
        int candidato_id FK
    }

    GENERO {
        int id PK
        string nombre
    }

    DIRECCION {
        int id PK
        int persona_id FK
        int ciudad_id FK
        string nomenclatura
        string barrio
        string comuna
        boolean es_principal
    }

    ZONA_ELECTORAL {
        int id PK
        int ciudad_id FK
        string codigo_zona "VARCHAR - Ej: '01', 'A1'"
        string nombre_zona
    }

    PUESTO_VOTACION {
        int id PK
        int zona_id FK
        string nombre_puesto
    }

    MESA {
        int id PK
        int puesto_id FK
        string numero_mesa "VARCHAR - Ej: '001'"
    }

    CIUDAD {
        int id PK
        string nombre
        int departamento_id FK
    }

    DEPARTAMENTO {
        int id PK
        string nombre
        int pais_id FK
    }

    PAIS {
        int id PK
        string nombre
    }

    PARTIDO_POLITICO {
        int id PK
        string nombre
        string siglas
    }

    CANDIDATO {
        int id PK
        string nombre
        int partido_id FK
    }

    TELEFONO {
        int id PK
        int persona_id FK
        string numero
        string tipo
    }

    EMAIL {
        int id PK
        int persona_id FK
        string correo
    }

    USER_ROLES {
        int user_id FK
        int role_id FK
    }

    ROLE_PERMISSIONS {
        int role_id FK
        int permission_id FK
    }
```