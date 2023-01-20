# CS_CreacionDeAPIweb.NET-E2
Controladores de API web de ASP.NET Core

Ejercicio: Adición de un almacén de datos

Ejecute el siguiente comando para crear una carpeta *Models*
:

```bash
mkdir Models
```

Agregue el código siguiente a *Models/Pizza.cs*
 para definir una pizza. Guarde los cambios.

namespace ContosoPizza.Models;

```csharp
namespace ContosoPizza.Models;

public class Pizza
{
    public int Id { get; set; }
    public string? Name { get; set; }
    public bool IsGlutenFree { get; set; }
}
```

## **Incorporación de un servicio de datos**

1. Ejecute el comando siguiente para crear una carpeta *Services*.

```bash
mkdir Services
```

Agregue el código siguiente a *Services/PizzaService.cs*
 para crear un servicio de datos de pizzas en memoria. Guarde los cambios.

```csharp
using ContosoPizza.Models;

namespace ContosoPizza.Services;

public static class PizzaService
{
    static List<Pizza> Pizzas { get; }
    static int nextId = 3;
    static PizzaService()
    {
        Pizzas = new List<Pizza>
        {
            new Pizza { Id = 1, Name = "Classic Italian", IsGlutenFree = false },
            new Pizza { Id = 2, Name = "Veggie", IsGlutenFree = true }
        };
    }

    public static List<Pizza> GetAll() => Pizzas;

    public static Pizza? Get(int id) => Pizzas.FirstOrDefault(p => p.Id == id);

    public static void Add(Pizza pizza)
    {
        pizza.Id = nextId++;
        Pizzas.Add(pizza);
    }

    public static void Delete(int id)
    {
        var pizza = Get(id);
        if(pizza is null)
            return;

        Pizzas.Remove(pizza);
    }

    public static void Update(Pizza pizza)
    {
        var index = Pizzas.FindIndex(p => p.Id == pizza.Id);
        if(index == -1)
            return;

        Pizzas[index] = pizza;
    }
}
```

Este servicio proporciona un sencillo servicio de almacenamiento en caché de datos en memoria con dos pizzas de forma predeterminada. Nuestra API web usará ese servicio con fines de demostración. Cuando se detiene y se inicia la API web, la caché de datos en memoria se restablecerá a las dos pizzas predeterminadas del constructor de `PizzaService`
.

## **Compilación del proyecto de API web**

Ejecute el siguiente comando para compilar la aplicación:

```csharp
dotnet build
```

La compilación se ejecuta correctamente sin advertencias. Si se produce un error en la compilación, compruebe la salida con el fin de obtener información para solucionar problemas.
