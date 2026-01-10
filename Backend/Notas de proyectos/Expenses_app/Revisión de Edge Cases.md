## 1️⃣ Usuarios y grupos

### 🔹 Usuario eliminado del sistema

**Escenario**

- Un usuario tiene gastos cargados en uno o varios grupos
- El usuario se elimina

**Riesgo**

- ¿Qué pasa con los gastos históricos?
- ¿Qué pasa con los grupos que creó?

**Recomendación funcional**

- No permitir eliminar usuarios con gastos  
    **o**
- Convertir usuario en “inactivo” (recomendado)
- Los gastos **no deben borrarse automáticamente**

👉 Esto es crítico para trazabilidad.

---

### 🔹 Administrador abandona el grupo

**Escenario**

- El creador del grupo quiere salir del grupo
    

**Decisión necesaria**

- ¿Se permite?
    
    - Si sí: ¿a quién se transfiere el rol de admin?
        
    - Si no: bloquear acción
        

**Recomendación**

- Requerir transferencia explícita de administración
    

---

## 2️⃣ Categorías

### 🔹 Eliminación de categoría en uso

**Escenario**

- Categoría “Nafta” usada en 300 gastos
    
- El admin intenta eliminarla
    

**Riesgo**

- Gastos quedarían sin categoría (violación de regla)
    

**Opciones válidas**

1. Bloquear eliminación si hay gastos asociados
    
2. Permitir eliminación pero:
    
    - mantener referencia histórica
        
    - ocultarla solo para nuevos gastos
        

👉 Recomiendo **opción 2** (estado inactivo).

---

### 🔹 Cambio de tipo de una categoría

**Escenario**

- Categoría creada como VARIABLE
    
- Admin intenta cambiarla a FIJO
    

**Problema**

- Gastos existentes pueden tener proveedor
    
- Inconsistencia directa
    

**Regla clara**

- ❌ No permitir cambiar el tipo de una categoría una vez creada
    

---

## 3️⃣ Proveedores

### 🔹 Eliminación de proveedor en uso

**Escenario**

- Proveedor “YPF” eliminado
    
- Gastos históricos lo referencian
    

**Tu decisión**

- Correcta: los gastos siguen asociados
    

**Edge case adicional**

- ¿El proveedor eliminado puede volver a crearse con el mismo nombre?
    

**Recomendación**

- Sí, pero como **entidad nueva**
    
- Los gastos antiguos siguen apuntando al proveedor histórico
    

---

### 🔹 Proveedor asignado a gasto fijo (error)

**Escenario**

- Bug en frontend
    
- Se envía proveedor para gasto fijo
    

**Regla crítica**

- Backend debe rechazarlo siempre
    

👉 Esto debe ser validado **sí o sí** en backend.

---

## 4️⃣ Gastos

### 🔹 Cambio de tipo de gasto

**Escenario**

- Gasto creado como VARIABLE con proveedor
    
- Usuario lo edita y lo cambia a FIJO
    

**Problema**

- El proveedor deja de ser válido
    

**Regla recomendada**

- Al cambiar de VARIABLE → FIJO:
    
    - eliminar proveedor automáticamente
        
    - o bloquear el cambio hasta que se quite proveedor
        

---

### 🔹 Edición retroactiva

**Escenario**

- Usuario cambia categoría o proveedor de un gasto antiguo
    

**Impacto**

- Reportes históricos cambian
    

**Decisión funcional**

- Está permitido (según lo definido)
    
- Aceptar que los análisis reflejen el estado actual, no histórico
    

👉 Esto está bien, solo hay que asumirlo conscientemente.

---

### 🔹 Eliminación masiva accidental

**Escenario**

- Usuario borra muchos gastos por error
    

**Recomendación**

- Confirmaciones fuertes en frontend
    
- Posible “undo” temporal (aunque no soft delete)
    

No es DB, pero es UX crítica.

---

## 5️⃣ Grupos

### 🔹 Eliminación de grupo

**Escenario**

- Admin elimina un grupo con:
    
    - gastos
        
    - categorías
        
    - proveedores
        

**Decisión necesaria**

- ¿Se permite?
    

**Recomendación fuerte**

- Sí, pero:
    
    - eliminación en cascada
        
    - confirmación explícita
        
    - acción irreversible
        

---

## 6️⃣ Consistencia entre entidades

### 🔹 Uso cruzado de entidades

**Escenario**

- Gasto intenta usar:
    
    - categoría de otro grupo
        
    - proveedor de otro grupo
        

**Regla absoluta**

- Debe ser imposible
    
- Validación backend obligatoria
    

---

### 🔹 Usuario crea gasto en grupo ajeno

**Escenario**

- Usuario intenta crear gasto en grupo donde no es miembro
**Regla**

- Rechazo inmediato


---

## 7️⃣ Concurrencia (multiusuario)

### 🔹 Dos admins editan lo mismo

**Escenario**

- Dos sesiones abiertas
- Ambos editan categoría o proveedor

**Riesgo**

- Última escritura pisa la anterior

**Recomendación**

- Manejo simple:
    
    - aceptar last-write-wins
        
- No es crítico en esta etapa






---

## 8️⃣ Análisis futuro (edge cases de reporting)

### 🔹 Categorías eliminadas

- Siguen apareciendo en reportes históricos
    
- Debe ser esperado, no un bug
    

### 🔹 Proveedores eliminados

- Siguen apareciendo en reportes
    
- Con estado “eliminado” o similar
    

---

## 9️⃣ Edge cases RESUELTOS por tu modelo

Esto es importante destacarlo:

✔ No hay gastos sin categoría  
✔ No hay gastos sin tipo  
✔ No hay proveedores en gastos fijos  
✔ No hay gastos compartidos  
✔ No hay ambigüedad grupo–usuario  
✔ No hay mezcla de conceptos (categoría ≠ proveedor)

Esto habla muy bien del diseño.

---