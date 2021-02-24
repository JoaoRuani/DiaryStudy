# .NET Common Web Applicatoins Architectures

[Microsoft Reference](https://docs.microsoft.com/pt-br/dotnet/architecture/modern-web-apps-azure/common-web-application-architectures)

![VS Solution Structure](https://docs.microsoft.com/pt-br/dotnet/architecture/modern-web-apps-azure/media/image5-1.png)

An architecture organized into folders can solve small problems, but as the complexity and project's size grows, the number of files and folders will continue to grow as well.

Organizing in folders, there's no clear indication of which classes in which folders should depend on which others. This lack of organization often leads to spaghetti code.

To address these issues, applications often evolve into multi-project solutions, where each project is considere to reside in a particular layer of the application.