Create an initial web project(REST):

From the File menu, select New > Project.
Enter Web API in the search box.
Select the ASP.NET Core Web API template and select Next.
In the Configure your new project dialog, name the project TodoApi and select Next.
In the Additional information dialog:
Confirm the Framework is .NET 8.0 (Long Term Support).
Confirm the checkbox for Use controllers(uncheck to use minimal APIs) is checked.
Confirm the checkbox for Enable OpenAPI support is checked.
Select Create.

Add a NuGet package

A NuGet package must be added to support the database used in this tutorial.
From the Tools menu, select NuGet Package Manager > Manage NuGet Packages for Solution.
Select the Browse tab.
Enter Microsoft.EntityFrameworkCore.InMemory in the search box, and then select Microsoft.EntityFrameworkCore.InMemory.
Select the Project checkbox in the right pane and then select Install.