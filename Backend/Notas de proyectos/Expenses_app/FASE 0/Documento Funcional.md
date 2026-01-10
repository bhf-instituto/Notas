# Documento Funcional — Aplicación de Registro de Gastos

## 1. Propósito de la aplicación

La aplicación permite a los usuarios **registrar gastos estructurados** con el objetivo de **analizarlos posteriormente**.  
No es una aplicación contable ni fiscal; los gastos representan **registros declarativos** creados por los usuarios.

La calidad, consistencia y trazabilidad de los datos es un objetivo prioritario del sistema.

---

## 2. Usuarios

- Existen usuarios individuales.
- Un usuario puede existir **sin pertenecer a ningún grupo** (por ejemplo, al momento de registrarse).
- Un usuario puede:
    - crear uno o varios grupos
    - participar en grupos creados por otros usuarios
- Los roles de los usuarios existen **exclusivamente dentro del contexto de un grupo**.

---

## 3. Grupos

- El grupo es una **unidad de contexto** que agrupa gastos relacionados.
- Un grupo:
    - tiene un **usuario creador**, que actúa como **administrador**
    - puede tener cero o más participantes
    - puede existir **sin gastos**
    
- Los grupos son independientes entre sí:
    - no comparten gastos
    - no comparten categorías
    - no comparten proveedores

---

## 4. Roles dentro del grupo

- **Administrador del grupo**
    - Es el creador del grupo
    - Tiene permisos exclusivos para:
        - crear, editar y eliminar categorías
        - crear, editar y eliminar proveedores
- **Participante**
    - Puede cargar gastos
    - Puede usar categorías y proveedores existentes
    - No puede gestionar categorías ni proveedores
        

---

## 5. Gastos (Entidad central)

El gasto es el núcleo de la aplicación.

Cada gasto:

- pertenece a **un único grupo**
- pertenece a **un único usuario**
- tiene **exactamente un tipo de gasto**
- tiene **exactamente una categoría**
- tiene una **fecha obligatoria**
- representa un evento económico único
- no puede ser compartido entre usuarios
- no puede pertenecer a más de un grupo

### Operaciones permitidas sobre gastos

- Un gasto **puede editarse**
- Un gasto **puede eliminarse**
- La eliminación es **real (hard delete)**, no soft delete.

---

## 6. Tipo de gasto

- Todo gasto tiene obligatoriamente un **tipo de gasto**
- Los tipos disponibles son:
    - **FIJO**
    - **VARIABLE**
- El conjunto de tipos es **cerrado y definido por el sistema**
- El usuario no puede crear nuevos tipos de gasto

El tipo de gasto determina reglas de negocio específicas.

---

## 7. Categorías

- Las categorías se utilizan para **clasificar los gastos**
- Una categoría:
    - pertenece a **un único grupo**
    - está asociada a **un único tipo de gasto** (FIJO o VARIABLE)
- Dentro de un mismo **grupo + tipo**, el nombre de la categoría debe ser **único**
- Las categorías:
    - no pertenecen a usuarios individuales
    - son compartidas por todos los participantes del grupo
- Gestión de categorías:
    - solo el **administrador del grupo** puede crear, editar o eliminar categorías

Las categorías no tienen ninguna relación directa con proveedores.

---

## 8. Proveedores

- El proveedor representa una **entidad real** con la que se realiza un gasto
- Un proveedor:
    - pertenece a **un único grupo**
    - puede tener información propia (nombre, contacto, teléfono, etc.)
- Proveedores con el mismo nombre pueden existir en distintos grupos como entidades independientes

### Gestión de proveedores

- Solo el **administrador del grupo** puede crear, editar o eliminar proveedores
- Un proveedor **puede eliminarse incluso si fue usado en gastos**
    - Los gastos históricos permanecen asociados a ese proveedor
    - El sistema debe conservar información histórica (logs) del proveedor eliminado

---

## 9. Relación entre gastos y proveedores

- Solo los gastos de tipo **VARIABLE**:
    - pueden tener **0 o 1 proveedor**
- Los gastos de tipo **FIJO**:
    - **no pueden** tener proveedor bajo ninguna circunstancia
- El proveedor:
    - es opcional
    - complementa al gasto
    - no reemplaza a la categoría

---

## 10. Reglas de negocio clave (resumen)

- Todo gasto:
    - tiene grupo
    - tiene usuario
    - tiene tipo (fijo o variable)
    - tiene categoría
    - tiene fecha
- Una categoría:
    - pertenece a un grupo
    - pertenece a un tipo de gasto
- Un proveedor:
    - pertenece a un grupo
- Un gasto variable:
    - puede o no tener proveedor
- Un gasto fijo:
    - nunca puede tener proveedor
- Todas las validaciones deben cumplirse tanto en frontend como en backend


---

## 11. Flujo funcional principal

### Configuración inicial

1. Usuario crea un grupo
2. Administrador crea categorías para:
    - gastos fijos
    - gastos variables
3. Administrador crea proveedores (opcional)

### Carga de gasto

1. Usuario selecciona grupo
2. Usuario crea gasto
3. Selecciona tipo de gasto
4. Selecciona categoría válida para ese grupo y tipo
5. Si el gasto es variable:
    - opcionalmente selecciona proveedor
6. Ingresa datos del gasto (monto, fecha, descripción, etc.)