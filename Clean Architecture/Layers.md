# What are layers

As applications grow in complexity, one way to manage that complexity is to break up the application according to its responsabilities or concerns. This approach follows the separation of concers principle and can help keep a growing codebase organizes so that developers can easily find where certain functionality is implemented. Layered architecture offers a number of advantages beyond just code organization, though.

By organizing code into layers, common low-level functionality can be reuses throughout the application. This reuse is beneficial because it menas less code needs to be written and because it can allow the aplication to standardize on a single implementantion, following the DRY principle.

With a layered architecture, applications can enforce restrictions on which layers can communicate with other layers. This architecture helps to achieve encapsulation. When a layer is changed or replaced, only those layers that work with it should be impacted. By limiting which layers depend on which other layers, the impact of changes can be mitigated so that a single change doesn't impact the entire application.

## Traditional "N-Layer" architecture applications

![Typical application layers](https://docs.microsoft.com/pt-br/dotnet/architecture/modern-web-apps-azure/media/image5-2.png)