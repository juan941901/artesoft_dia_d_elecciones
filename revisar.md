erDiagram
    %% Módulo de Seguridad y Usuarios
    USERS ||--o{ USER_ROLES : has
    ROLES ||--o{ USER_ROLES : assigned_to
    ROLES ||--o{ ROLE_PERMISSIONS : contains
    PERMISSIONS ||--o{ ROLE_PERMISSIONS : granted_to

    %% Módulo de Personas y Atributos
    PERSONA ||--o{ TELEFONO : tiene
    PERSONA ||--o{ EMAIL : tiene
    PERSONA ||--o{ DIRECCION : reside_en
    PERSONA }|--|| GENERO : pertenece
    
    %% Gestión de Liderazgo (Nueva tabla para evitar indicadores)
    PERSONA ||--o{ LIDERAZGO_ZONA : ejerce
    ZONA_ELECTORAL ||--o{ LIDERAZGO_ZONA : coordinada_por

    %% Gestión de Candidatos
    PERSONA ||--|| CANDIDATO : "puede ser"
    CANDIDATO }|--|| PARTIDO_POLITICO : pertenece_a
    
    %% Relación de Apoyo (Quién apoya a quién)
    PERSONA }|--|| CANDIDATO : apoya
    PERSONA }|--|| PARTIDO_POLITICO : milita

    %% Módulo Electoral
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
    }

    PERSONA {
        int id PK
        string nombre_completo
        string identificacion_oficial UK
        int edad
        int genero_id FK
        int mesa_id FK
    }

    CANDIDATO {
        int id PK
        int persona_id FK "Relación 1:1 con Persona"
        int partido_id FK
        string cargo_aspirado "Ej: Alcaldía, Concejo"
        string eslogan
        boolean activo
    }

    LIDERAZGO_ZONA {
        int id PK
        int persona_id FK
        int zona_id FK
        date fecha_asignacion
        boolean activo
    }

    ZONA_ELECTORAL {
        int id PK
        int ciudad_id FK
        string codigo_zona "VARCHAR"
        string nombre_zona
    }

    DIRECCION {
        int id PK
        int persona_id FK
        string nomenclatura
        string barrio
        string comuna
    }

    %% Resto de tablas maestras
    GENERO { int id PK, string nombre }
    MESA { int id PK, int puesto_id FK, string numero_mesa }
    PUESTO_VOTACION { int id PK, int zona_id FK, string nombre_puesto }
    PARTIDO_POLITICO { int id PK, string nombre, string siglas }
    CIUDAD { int id PK, string nombre }
    DEPARTAMENTO { int id PK, string nombre }
    PAIS { int id PK, string nombre }
    TELEFONO { int id PK, int persona_id FK, string numero }
    EMAIL { int id PK, int persona_id FK, string correo }
    USER_ROLES { int user_id FK, int role_id FK }
    ROLE_PERMISSIONS { int role_id FK, int permission_id FK }
    ROLES { int id PK, string name }
    PERMISSIONS { int id PK, string name }
