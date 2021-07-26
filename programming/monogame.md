# Monogame + Visual Studio

### New project setup:

1. Install:
    - `Monogame for Visual Studio` (MonoGame Pipeline Tool)
    - `.NET Core SDK`
2. In any folder you want, run:
    - `dotnet new sln`
    - `dotnet new -i MonoGame.Template.CSharp`
    - `dotnet new mgdesktopgl -o ProjectName`
    - `dotnet sln add ProjetName/ProjectName.csproj`
3. Edit `ProjectName.csproj` to have the following:

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.1</TargetFramework>
        <TargetLatestRuntimePatch>true</TargetLatestRuntimePatch>
    </PropertyGroup>
    <ItemGroup>
        <MonoGameContentReference Include="**\*.mgcb" />
    </ItemGroup>
    <ItemGroup>
        <PackageReference Include="MonoGame.Framework.DesktopGL" Version="3.8.0.1641" />
        <PackageReference Include="MonoGame.Content.Builder.Task" Version="3.8.0.1641" />
    </ItemGroup>

    <Target Name="SpicNSpan" AfterTargets="Clean">
        <RemoveDir Directories="$(BaseIntermediateOutputPath)" />
        <RemoveDir Directories="$(BaseOutputPath)" />
    </Target>
    </Project>
    ```

### Commands

To know if you have any compile errors: `dotnet test`.  
To build the game: `dotnet clean` and `dotnet build`. Then, press F5 to run the debug version. F5 will only work correctly when working in a workspace of the project.  
To publish the game:

-   Windows: `dotnet clean` -> `dotnet build` -> `dotnet publish -r win-x64 -c release`
-   Mac: `dotnet clean` -> `dotnet build` -> `dotnet publish -r osx-x64 -c release`
-   Linux: `dotnet clean` -> `dotnet build` -> `dotnet publish -r linux-x64 -c release`

### Preprocessor symbols

To define a custom preprocessor symbol:

1. Open `tasks.json` file in `.vscode` folder
2. Add the following line to `args` property in the build task:

```
-p:DefineConstants=CUSTOM_SYMBOL
```

# Coordinate system

Coordinate system in monogame is:

-   top left = `(0, 0)`
-   bottom right = `(width, height)`

Due to this, it might be confusing to move objects in y coordinate, because usually Y coordinate is up, not down. To solve this, one could wrap a sprite batch drawing function so that it recalculates the position to draw in so that Y becomes up, not down.

# Graphics

### Scissor Test

To enable a scissor test, you have to do the following things:

1. Set the scissor rect at `spriteBatch.GraphicsDevice.ScissorRectangle`.
2. Set the appropriate rasterizer state for the spriteBatch:

```csharp
var state = new RasterizerState() { ScissorTestEnable = true };
spriteBatch.Begin(rasterizerState: state);
```

**IMPORTANT**: Scissoring only happens when on spriteBatch.End()

# Cursor

Set a cursor to custom image:

```csharp
Texture2D tex = Content.Load<Texture2D>("texture_name");
var cursor = MouseCursor.FromTexture2D(tex, tex.Width / 2, tex.Height / 2);
Mouse.SetCursor(cursor);
```

# Debug

To write to console, use:

```csharp
System.Console.WriteLine('Text');
```

# Fonts

Monogame can only use bitmap fonts. To import a .ttf font into the game do the following:

1. In Content editor, add new Sprite Font Description file.
2. Open the file (it's an XML) and set the name of the font you want to import.
3. (Optional) Change other properties of the font, like size, spacing, kerning, etc.

# Callbacks

To implement callbacks, use `System.Action` or `System.Func`;
