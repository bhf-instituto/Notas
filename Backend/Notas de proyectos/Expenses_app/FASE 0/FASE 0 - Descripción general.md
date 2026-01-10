## 1️⃣ Propósito general de la aplicación

La aplicación permite **registrar gastos** con el objetivo de **analizarlos posteriormente**.  
El foco principal está en la **calidad, consistencia y trazabilidad de los datos**, más que en la simple carga.

Los gastos se organizan dentro de **grupos**, que funcionan como contextos independientes (por ejemplo, “Personal”, “Emprendimiento”, etc.).

---

## 2️⃣ Usuarios

- Existen usuarios individuales.
    
- Un usuario puede:
    
    - crear uno o varios grupos
        
    - participar en grupos creados por otros usuarios
        
- Un usuario puede pertenecer a muchos grupos.
    
- Dentro de cada grupo:
    
    - hay un **administrador** (el creador)
        
    - puede haber **participantes**
        
- Los roles existen **solo en el contexto del grupo**.
    

---

## 3️⃣ Grupos

- El grupo es una **unidad de contexto** para los gastos.
    
- Todo gasto pertenece a **un único grupo**.
    
- Los grupos:
    
    - tienen un creador/administrador
        
    - pueden ser compartidos entre varios usuarios
        
- Los usuarios que pertenecen a un grupo pueden:
    
    - cargar gastos en ese grupo
        
    - usar las categorías y proveedores definidos para ese grupo
        

---

## 4️⃣ Gastos (núcleo del sistema)

El gasto es la entidad central de la aplicación.

Cada gasto:

- pertenece a **un solo usuario**
    
- pertenece a **un solo grupo**
    
- tiene **exactamente un tipo de gasto**
    
- tiene **exactamente una categoría**
    
- representa un evento económico único
    

No existen:

- gastos compartidos entre usuarios
    
- gastos que pertenezcan a más de un grupo
    

---

## 5️⃣ Tipo de gasto

- Todo gasto tiene obligatoriamente un **tipo de gasto**.
    
- Los tipos definidos actualmente son:
    
    - **FIJO**
        
    - **VARIABLE**
        
- El tipo de gasto:
    
    - determina ciertas reglas de negocio
        
    - no es opcional
        
    - forma parte de la identidad lógica del gasto
        

---

## 6️⃣ Categorías

- Las categorías sirven para **clasificar los gastos**.
    
- Las categorías:
    
    - pertenecen a un **grupo**
        
    - están asociadas a **un único tipo de gasto** (fijo o variable)
        
- Las categorías:
    
    - no pertenecen a un usuario individual
        
    - son compartidas por todos los participantes del grupo
        
- Un usuario puede crear categorías dentro de un grupo, y luego:
    
    - cualquier participante del grupo puede utilizarlas al cargar gastos
        
- Las categorías **no tienen relación directa con proveedores**.
    
- Una categoría **nunca sirve para más de un tipo de gasto**.
    

---

## 7️⃣ Proveedores

- El proveedor representa una **entidad real** con la que se realiza un gasto.
    
- Un proveedor:
    
    - pertenece a un **grupo**
        
    - puede tener información propia (nombre, contacto, teléfono, etc.)
        
- El proveedor **no es una categoría**.
    
- El proveedor **no es obligatorio en todos los gastos**.
    

### Relación proveedor ↔ gasto

- Solo los gastos de tipo **VARIABLE**:
    
    - pueden tener **0 o 1 proveedor**
        
- Los gastos de tipo **FIJO**:
    
    - **no pueden** tener proveedor bajo ninguna circunstancia
        
- El proveedor:
    
    - es opcional
        
    - complementa al gasto
        
    - no reemplaza a la categoría
        

---

## 8️⃣ Reglas de negocio clave (resumen)

- Todo gasto:
    
    - tiene grupo
        
    - tiene usuario
        
    - tiene tipo (fijo/variable)
        
    - tiene categoría
        
- Una categoría:
    
    - pertenece a un grupo
        
    - pertenece a un tipo de gasto
        
- Un proveedor:
    
    - pertenece a un grupo
        
- Un gasto variable:
    
    - puede tener proveedor o no
        
- Un gasto fijo:
    
    - nunca tiene proveedor
        

---

## 9️⃣ Flujo lógico de uso

### Configuración inicial del grupo

1. Usuario crea un grupo
    
2. Usuario crea categorías:
    
    - para gastos fijos
        
    - para gastos variables
        
3. Usuario crea proveedores (opcional)
    

### Carga de gasto

1. Usuario selecciona grupo
    
2. Usuario crea gasto
    
3. Selecciona tipo de gasto (fijo o variable)
    
4. Selecciona categoría válida para ese tipo y grupo
    
5. (Si es variable) opcionalmente selecciona proveedor
    
6. Ingresa datos del gasto (monto, descripción, etc.)
    

---

## 10️⃣ Características implícitas del diseño

Este modelo:

- evita duplicación de datos
    
- separa correctamente conceptos
    
- es fácilmente validable
    
- permite análisis futuro sin refactor
    
- mantiene el frontend simple sin sacrificar calidad de datos
    

---