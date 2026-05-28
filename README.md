# 🗃️ Base de Datos — Conflictos Armados

Base de datos relacional diseñada para modelar y gestionar información sobre conflictos armados a nivel mundial, incluyendo grupos armados, líderes, armamento, traficantes, organizaciones mediadoras y países involucrados.

---

## 📋 Contenido del repositorio

```
Entrega/
├── conflicto.sql                          # Script DDL — creación del esquema completo
├── Diccionario de datos.xlsx              # Diccionario de datos
├── ImagenDiseñoConflicto.jpg              # Diagrama entidad-relación
├── proyectoConflictos.zip                 # Proyecto empaquetado
├── sql llenado bd/                        # Scripts de inserción de datos
│   ├── InsertConflictos.sql
│   ├── insertPaises.sql
│   ├── insertGruposArmados.sql
│   ├── InsertArmas.sql
│   ├── insertOrganizaciones.sql
│   ├── insertLideresMilitares.sql
│   ├── insertLideresPoliticos.sql
│   ├── insertDivisiones.sql
│   ├── insertEtnias.sql
│   ├── insertEspecializaciones.sql
│   ├── insertDialogos.sql
│   ├── insertTraficantes.sql
│   ├── InsertTraficantesArmas.sql
│   ├── insertSuministros.sql
│   ├── InsertConflictos_Grupos.sql
│   ├── InsertConflictos_OrganizacionesMediadoras.sql
│   ├── InsertConflictos_Paises.sql
│   └── insertRegionesUotroNoDependientes.sql
└── trigger-vistas-procedimiento/          # Objetos adicionales de BD
    ├── PA_TOTAL_BAJAS.sql                 # Procedimiento almacenado
    ├── TRG_CONFLICTOS_INSERT.sql          # Trigger de inserción
    ├── TRG_CONFLICTOS_UPDATE.sql          # Trigger de actualización
    ├── VISTA 1 4-TABLAS.sql
    ├── VISTA 2 3-TABLAS.sql
    └── VISTA 3 4-TABLAS.sql
```

---

## 🧱 Estructura de la base de datos

La base de datos `ConflictosArmados` cuenta con **30 tablas** organizadas en los siguientes módulos:

### Entidades principales

| Tabla | Descripción |
|---|---|
| `CONFLICTOS` | Registro de conflictos con nombre, número de muertos y heridos |
| `PAISES` | Países del mundo involucrados en conflictos |
| `GRUPOS_ARMADOS` | Grupos armados participantes |
| `ORGANIZACIONES_MEDIADORAS` | ONGs, organismos internacionales y entidades gubernamentales |
| `TRAFICANTES` | Traficantes de armas |
| `ARMAS` | Armamento con nivel destructivo asociado |
| `NIVELES_DESTRUCTIVOS` | Clasificación del nivel de destrucción de un arma (1–5) |

### Tipos de conflicto (herencia)

| Tabla | Descripción |
|---|---|
| `CONFLICTOS_TERRITORIALES` | Conflictos por disputas de territorio |
| `CONFLICTOS_RELIGIOSOS` | Conflictos motivados por diferencias religiosas |
| `CONFLICTOS_ECONOMICOS` | Conflictos por recursos y materias primas |
| `CONFLICTOS_RACIALES` | Conflictos de origen racial o étnico |

### Estructura militar

| Tabla | Descripción |
|---|---|
| `DIVISIONES` | Divisiones por grupo armado (barcos, tanques, aviones, hombres, bajas) |
| `LIDERES_MILITARES` | Líderes militares con rango y división asignada |
| `LIDERES_POLITICOS` | Líderes políticos por grupo armado |

### Tablas de relación y clasificación

`CONFLICTOS_PAISES`, `CONFLICTOS_GRUPOS_ARMADOS`, `CONFLICTOS_ORGANIZACIONES_MEDIADORAS`, `CONFLICTOS_TERRITORIALES_REGIONES`, `CONFLICTOS_RELIGIOSOS_RELIGIONES`, `CONFLICTOS_ECONOMICOS_MATERIAS_PRIMAS`, `CONFLICTOS_RACIALES_ETNIAS`, `TRAFICANTES_ARMAS`, `SUMINISTROS`, `ORGANIZACIONES_MEDIADORAS_LIDERES_POLITICOS`

---

## 📊 Datos de ejemplo incluidos

Los scripts de inserción cargan datos reales y de prueba, entre ellos:

- **50 conflictos** — Guerra Rusia-Ucrania, Guerra Civil Siria, Conflicto Israel-Palestina, entre otros
- **50 países** — desde Colombia y Venezuela hasta Rusia, China y Japón
- **50 grupos armados** — Fuerzas Armadas de Ucrania, Talibanes, Hamas, FARC-EP, Boko Haram, etc.
- **50 armas** — desde pistolas hasta tanques M1 Abrams, sistemas HIMARS y drones Bayraktar TB2
- **50 organizaciones mediadoras** — ONU, Cruz Roja, Médicos Sin Fronteras, OTAN, entre otras

---

## ⚙️ Objetos de base de datos

### Procedimiento almacenado — `SP_TOTAL_BAJAS`
Calcula el **total de bajas por grupo armado** sumando las bajas de todas sus divisiones mediante un cursor. Retorna una tabla con `codigo_grupo_armado`, `nombre_grupo` y `total_bajas`.

```sql
EXEC SP_TOTAL_BAJAS;
```

### Triggers

| Trigger | Evento | Acción |
|---|---|---|
| `TRG_CONFLICTOS_INSERT` | `AFTER INSERT` en `CONFLICTOS` | Registra el nuevo conflicto en la tabla `BITACORA_CONFLICTOS` |
| `TRG_CONFLICTOS_UPDATE` | `AFTER UPDATE` en `CONFLICTOS` | Registra la actualización con fecha en `BITACORA_CONFLICTOS` |

### Vistas

| Vista | Tablas involucradas | Descripción |
|---|---|---|
| Vista 1 | 4 tablas | Conflictos con sus grupos armados y líderes políticos |
| Vista 2 | 3 tablas | Traficantes con las armas que suministran y cantidades |
| Vista 3 | 4 tablas | Conflictos con organizaciones mediadoras y tipo de ayuda |

---

## 🚀 Instalación y uso

### Requisitos

- **SQL Server** (cualquier edición, 2016 o superior recomendado)
- SQL Server Management Studio (SSMS) u otro cliente compatible

### Pasos

1. Crear la base de datos:
   ```sql
   CREATE DATABASE ConflictosArmados;
   USE ConflictosArmados;
   ```

2. Ejecutar el script de estructura:
   ```
   conflicto.sql
   ```

3. Poblar la base de datos ejecutando los scripts en la carpeta `sql llenado bd/` en el siguiente orden sugerido:
   ```
   insertPaises.sql
   insertGruposArmados.sql
   InsertArmas.sql
   insertOrganizaciones.sql
   InsertConflictos.sql
   insertDivisiones.sql
   insertLideresPoliticos.sql
   insertLideresMilitares.sql
   insertTraficantes.sql
   InsertTraficantesArmas.sql
   ... (resto de relaciones)
   ```

4. Ejecutar los objetos de la carpeta `trigger-vistas-procedimiento/`.

---

## 📐 Diagrama ER

El diagrama entidad-relación del diseño se encuentra en:

```
Entrega/ImagenDiseñoConflicto.jpg
```

---

## 📖 Diccionario de datos

La descripción detallada de cada tabla, sus atributos, tipos de datos y restricciones está documentada en:

```
Entrega/Diccionario de datos.xlsx
```
