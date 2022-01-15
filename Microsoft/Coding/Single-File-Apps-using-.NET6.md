# Single File Apps In .NET 6
> **Source:** https://dotnetcoretutorials.com/2021/11/10/single-file-apps-in-net-6/

## Basics
- Create a .NET 6 Console app and keep it as basic as possible (see below.)
- Build the project

```cs
using System;

Console.WriteLine("Hello, World!");
Console.ReadLine();
```
- Open a terminal and navigate to the root folder for your project.
- To publish **self-contained**, run the following command. 
  - **Warning:** "Self contained" means the entire `.NET 6 Runtime` will be packaged into an exe with your app.
  - Even this tiny Console app will be around `60MB` when built.
- Once the operation completes, you can find your `Single File App` in its published directory similar to: `C:\Users\%username%\source\repos\%your_project%\bin\Release\net6.0\win-x64\publish`

> **Self-Contained Package Publishing:**
> ```bat
> dotnet publish -p:PublishSingleFile=true -r win-x64 -c Release --self-contained true
> ```

- You also have the option to publish using **Self-contained** `false` which will package everything except the `.NET 6 Runtime` and leave you with a much smaller package (.exe.)
  - **Warning:** This option requires the device(s) it runs on to have the `.NET 6 Runtime` installed prior to running the application.
  - Expect this console app to be around `150KB` when published.

> **Publishing "Self-Contained" but WITHOUT the .NET 6 Runtime:**
> ```bat
> dotnet publish -p:PublishSingleFile=true -r win-x64 -c Release --self-contained false
> ```
