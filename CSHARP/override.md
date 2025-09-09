## 1.¿Qué significa `override`?
#### (Foma parte de [[Poliformismo]])
### Teoría
En C#, `override` es una palabra clave que permite a una clase derivada proporcionar una nueva implementación para un método, propiedad, indexador o evento que ya está declarado en su clase base como `virtual`, `abstract` o `override`. Para usar `override`, el miembro de la clase base debe haber sido marcado como `virtual` o `abstract`, y el método en la clase derivada debe tener la misma firma (nombre, tipo de retorno y parámetros). 

#### Propósito y uso:
- **[Polimorfismo](https://www.google.com/search?sca_esv=453f87096d44fbd1&cs=1&sxsrf=AE3TifN01LvwR6x-FGTHl_q78EzPO33dfA%3A1757382584165&q=Polimorfismo&sa=X&ved=2ahUKEwj81_akyMqPAxWkg5UCHQUsGuQQxccNegQIDhAB&mstk=AUtExfBG7fWDb9doRWlJbcSXPdC3_RxRpmMDurpqNfeCq9QU4ogACICEpJPNKsOkfuixfB79hX30qwd7go5wY81BOpSxxBJ7i1w1Qjen82R-oEW5teN5ruj-txsBR9dAU7iSnZSpRhYqjAtLUcpHS08f_RRIqZLATjTRcpaFpZvkqZN2HgPcGBuPwYf4Ml_iJOHCc-urqiEBIdsKjz0QJ_DPQByTZb9HmQZxD2xdr1geTAnh-nLWaDJKAZVXkBY5FDAbltKb7S_yCoavzqAmlBKycOiY&csui=3):**
    `override` es una característica fundamental del polimorfismo, que permite a las clases derivadas ofrecer su propia versión de un comportamiento de la clase base. 
- **Modificar comportamiento:**
    Se usa para extender o modificar la implementación de un miembro heredado, en lugar de crear uno nuevo que lo oculte. 
- **Herencia:**
    Garantiza que cuando se llama a un miembro desde una referencia de la clase base, se ejecute  la implementación específica de la clase derivada. 
#### Requisitos:
- **Clase base virtual/abstract:**
    El miembro de la clase base que se va a sobrescribir debe estar marcado como `virtual` o `abstract`. 
- **Firma idéntica:**
    La declaración del método en la clase derivada debe tener exactamente la misma firma que el método de la clase base. 
- **Acceso de nivel:**
    Tanto el método `override` como el método `virtual` de la clase base deben tener el mismo modificador de nivel de acceso (por ejemplo, `public` o `protected`).
#### Diferencia con `new`:

- La palabra clave `new` se utiliza para ocultar un miembro existente en la clase base, en lugar de extender su comportamiento. 

- `override` se usa cuando se desea modificar explícitamente el comportamiento del miembro virtual o abstracto de la clase base, mientras que `new` simplemente crea una nueva definición que "esconde" la anterior.
### Ejemplo
En C#, todas las **clases** (y los `struct` también, aunque no soportan herencia) heredan de la clase base `object`. Esa clase base trae métodos “de fábrica”, como:
- `ToString()`
- `Equals()`
- `GetHashCode()`

Cuando vos ponés **`override`**, lo que hacés es **redefinir** un método que ya existe en la clase base para darle tu propia implementación.

**Ejemplo Simple**
```csharp 
public class Animal
{
    public virtual void HacerSonido()
    {
        Console.WriteLine("Sonido genérico de animal");
    }
}
public class Perro : Animal
{
    public override void HacerSonido()
    {
        Console.WriteLine("Guau guau!");
    }
}

// Uso 
Animal a = new Animal();
a.HacerSonido(); // → "Sonido genérico de animal"

Perro p = new Perro();
p.HacerSonido(); // → "Guau guau!"

```
➡️ El `override` indica: “no uses la versión de la clase base, usá esta que yo defino”.
