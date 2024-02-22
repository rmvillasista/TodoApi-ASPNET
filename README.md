### Create an initial web project(REST):

1. From the File menu, select `New > Project`.
2. Enter `Web API` in the search box.
3. Select the `ASP.NET Core Web API template` and select Next.
4. In the Configure your new project dialog, name the project `TodoApi` and select Next.
5. In the Additional information dialog:
5.a. Confirm the Framework is `.NET 8.0 (Long Term Support)`.
5.b. Confirm the checkbox for `Use controllers`(uncheck to use minimal APIs) is checked.
5.c. Confirm the checkbox for `Enable OpenAPI support` is checked.
5. Select Create.

### Add a NuGet package

1. A NuGet package must be added to support the database used in this tutorial.
2. From the Tools menu, select `NuGet Package Manager > Manage NuGet` Packages for Solution.
3. Select the `Browse` tab.
4. Enter `Microsoft.EntityFrameworkCore.InMemory` in the search box, and then select `Microsoft.EntityFrameworkCore.InMemory`.
5. Select the Project checkbox in the right pane and then select Install.

### Create Models

1. In Solution Explorer, right-click the project. Select Add > New Folder. Name the folder Models.
2. Right-click the Models folder and select Add > Class. Name the class TodoItem.cs and select Add.
3. Replace the template code with the following:

```
namespace TodoApi.Models;


public class TodoItem
{
    public long Id { get; set; }
    public string? Name { get; set; }
    public bool IsComplete { get; set; }
}
```

### Create Context

1. Right-click the Models folder and select Add > Class. Name the class TodoContext.cs and click Add.

```
using Microsoft.EntityFrameworkCore;

namespace TodoApi.Models;

public class TodoContext : DbContext
{
    public TodoContext(DbContextOptions<TodoContext> options)
        : base(options)
    {
    }

    public DbSet<TodoItem> TodoItems { get; set; } = null!;
}
```

### Register Context

Update Program.cs with the following highlighted code:

```
using Microsoft.EntityFrameworkCore;
using TodoApi.Models;

builder.Services.AddDbContext<TodoContext>(opt =>
    opt.UseInMemoryDatabase("TodoList"));
```
### Scafold Controller

Right-click the Controllers folder.

Select Add > New Scaffolded Item.

Select API Controller with actions, using Entity Framework, and then select Add.

In the Add API Controller with actions, using Entity Framework dialog:

Select TodoItem (TodoApi.Models) in the Model class.
Select TodoContext (TodoApi.Models) in the Data context class.
Select Add.
If the scaffolding operation fails, select Add to try scaffolding a second time.

### Update the PostTodo create method 

Update the return statement in the PostTodoItem to use the nameof operator:

The preceding code is an HTTP POST method, as indicated by the [HttpPost] attribute. The method gets the value of the TodoItem from the body of the HTTP request.

For more information, see Attribute routing with Http[Verb] attributes.

The CreatedAtAction method:

> Returns an HTTP 201 status code if successful. HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.
> Adds a Location header to the response. The Location header specifies the URI of the newly created to-do item. For more information, see 10.2.2 201 Created.
> References the GetTodoItem action to create the Location header's URI. The C# nameof keyword is used to avoid hard-coding the action name in the CreatedAtAction call.

```
[HttpPost]
public async Task<ActionResult<TodoItem>> PostTodoItem(TodoItem todoItem)
{
    _context.TodoItems.Add(todoItem);
    await _context.SaveChangesAsync();

    //    return CreatedAtAction("GetTodoItem", new { id = todoItem.Id }, todoItem);
    return CreatedAtAction(nameof(GetTodoItem), new { id = todoItem.Id }, todoItem);
}
```

Reference Link:
https://learn.microsoft.com/en-us/aspnet/core/tutorials/first-web-api?view=aspnetcore-8.0&tabs=visual-studio
