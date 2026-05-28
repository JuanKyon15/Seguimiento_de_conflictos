# ConflictosArmados — Base de Datos y Aplicación de Escritorio

Proyecto académico de bases de datos que modela, almacena y gestiona información sobre conflictos armados a nivel mundial. Incluye el diseño de la base de datos relacional, scripts SQL completos y una aplicación de escritorio Java con interfaz gráfica para consultar y administrar los datos.

---

## Tecnologías utilizadas

| Capa | Tecnología |
|---|---|
| Base de datos | Microsoft SQL Server |
| Lenguaje de aplicación | Java (Swing) |
| IDE | Apache NetBeans |
| Driver de conexión | JDBC (`mssql-jdbc`) |

---

## Estructura del proyecto

```
Proyecto BD Conflictos/
└── Entrega/
    ├── Creacion BD_CONFLICTOS.sql               # DDL: creación de tablas y relaciones
    ├── Llenado de BD_CONFLICTOS.sql             # DML: datos de prueba
    ├── Diccionario de datos.xlsx                # Documentación de cada tabla y campo
    ├── ImagenDiseñoConflicto.jpg                # Diagrama entidad-relación (ER)
    ├── trigger-vistas-procedimiento/
    │   ├── TRG_CONFLICTOS_INSERT.sql            # Trigger de auditoría en INSERT
    │   ├── TRG_CONFLICTOS_UPDATE.sql            # Trigger de auditoría en UPDATE
    │   ├── VISTA 1 4-TABLAS.sql                 # Vista: conflictos, grupos y líderes políticos
    │   ├── VISTA 2 3-TABLAS.sql                 # Vista: traficantes y armas
    │   ├── VISTA 3 4-TABLAS.sql                 # Vista: conflictos y organizaciones mediadoras
    │   └── PA_TOTAL_BAJAS.sql                   # Procedimiento almacenado: total de bajas por grupo
    └── ConflictosArmadosApp/
        └── src/
            ├── conexion/
            │   └── Conexion.java                # Clase de conexión JDBC a SQL Server
            └── formularios/
                ├── FrmMenu.java / .form         # Menú principal
                ├── FrmConflictos.java / .form   # CRUD de conflictos
                ├── FrmEditarConflicto.java / .form  # Edición de un conflicto
                └── FrmProcedimiento.java / .form    # Ejecución del procedimiento almacenado
```

---

## Modelo de datos

La base de datos `ConflictosArmados` contiene **30 tablas** que cubren:

- **Conflictos** — entidad principal con nombre, número de muertos y heridos. Se especializa en cuatro subtipos: *territorial*, *religioso*, *económico* y *racial*.
- **Países y Regiones** — territorios involucrados en cada conflicto.
- **Grupos armados y Divisiones** — unidades militares con inventario de armas, tanques, barcos, aviones y registro de bajas.
- **Líderes políticos y militares** — con jerarquía entre ellos.
- **Organizaciones mediadoras** — ONGs, organismos internacionales, etc., con tipo y posible dependencia entre sí.
- **Armas, Niveles destructivos y Traficantes** — gestión del tráfico de armamento hacia los grupos.
- **Suministros** — relación entre grupo armado, traficante y arma suministrada.
- **Ayudas** — tipo de apoyo que una organización mediadora aporta a un conflicto.
- **Bitácora de conflictos** — tabla de auditoría poblada automáticamente por los triggers.

---

## Objetos de base de datos

### Triggers

| Nombre | Evento | Descripción |
|---|---|---|
| `TRG_CONFLICTOS_INSERT` | AFTER INSERT en `CONFLICTOS` | Registra en `BITACORA_CONFLICTOS` cada nuevo conflicto con la observación "Nuevo conflicto registrado". |
| `TRG_CONFLICTOS_UPDATE` | AFTER UPDATE en `CONFLICTOS` | Registra en `BITACORA_CONFLICTOS` los cambios realizados sobre un conflicto. |

### Vistas

| Nombre | Tablas involucradas | Descripción |
|---|---|---|
| Vista 1 | `CONFLICTOS`, `CONFLICTOS_GRUPOS_ARMADOS`, `GRUPOS_ARMADOS`, `LIDERES_POLITICOS` | Lista cada conflicto con su grupo armado y líder político asociado. |
| Vista 2 | `TRAFICANTES`, `TRAFICANTES_ARMAS`, `ARMAS` | Muestra qué armas y en qué cantidad maneja cada traficante. |
| Vista 3 | `CONFLICTOS`, `CONFLICTOS_ORGANIZACIONES_MEDIADORAS`, `ORGANIZACIONES_MEDIADORAS`, `AYUDAS` | Detalla qué organizaciones participan en cada conflicto, el tipo de ayuda y el personal desplegado. |

### Procedimiento almacenado

**`SP_TOTAL_BAJAS`** — Recorre con un cursor todas las divisiones de cada grupo armado y acumula el total de bajas por grupo, devolviendo un resumen con `codigo_grupo_armado`, `nombre_grupo` y `total_bajas`.

---

## Aplicación de escritorio

La aplicación Java Swing permite interactuar con la base de datos sin escribir SQL manualmente.

### Formularios

- **FrmMenu** — pantalla de inicio con acceso a los dos módulos principales.
- **FrmConflictos** — lista todos los conflictos en una tabla estilo Excel; permite agregar y seleccionar un registro para editar.
- **FrmEditarConflicto** — edición de los campos de un conflicto existente (nombre, muertos, heridos) con validación y confirmación.
- **FrmProcedimiento** — ejecuta `SP_TOTAL_BAJAS` y muestra el resultado del total de bajas por grupo armado.

---

## Configuración y ejecución

### 1. Base de datos

1. Abrir **SQL Server Management Studio** (o similar).
2. Ejecutar `Creacion BD_CONFLICTOS.sql` para crear la base de datos y todas las tablas.
3. Ejecutar `Llenado de BD_CONFLICTOS.sql` para insertar los datos de prueba.
4. Ejecutar los scripts de la carpeta `trigger-vistas-procedimiento/` para crear los triggers, vistas y procedimiento almacenado.

### 2. Aplicación Java

1. Abrir el proyecto `ConflictosArmadosApp` en **Apache NetBeans**.
2. Agregar el driver JDBC de SQL Server (`mssql-jdbc-*.jar`) a las librerías del proyecto.
3. Verificar los parámetros de conexión en `src/conexion/Conexion.java`:

```java
String usuario = "sa";
String password = "123456";
String bd      = "ConflictosArmados";
// Host: localhost, Puerto: 1433
```

4. Compilar y ejecutar el proyecto (`Run Project`).

> **Nota:** Si se cambian las credenciales de SQL Server, actualizar los valores correspondientes en `Conexion.java` antes de compilar.

---

## Autores

Proyecto desarrollado como entrega académica para la materia de Bases de Datos.

- **Royman Orozco**
- **Samuel Guzman**
- **Juan Cayon**
 

Fecha de entrega: **28 de mayo de 2026**
