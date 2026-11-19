# FluentUI Blazor V5 forces enum grid columns to lowercase 

This repository was created to demonstrate an issue with FluentUI Blazor V5 where enum grid
columns are forced to lowercase.

This issue is new in V5.x of FluentUI Blazor. The same code works as expected in V4.x of FluentUI Blazor.

The code is based on the FluentUI Blazor WebAssembly template. I changed the "Summary" column
of the Weather page to be an enum rather than a string. The weather page shows the enums in lowercase
instead of the defined case. Although, if the enum has a [Display(Name = "Some Name")] attribute, 
the grid shows the display name instead of the lowercase enum value.

This issue is caused by the addition of ".ToLower..." in the GetDisplay enum extension (Core/Extensions/EnumExtensions.cs).

```csharp
    public static string GetDisplay(this Enum value)
    {
        var fieldInfo = value.GetType().GetField(value.ToString());
        var attribute = fieldInfo?.GetCustomAttribute<DisplayAttribute>();

        var result = attribute?.GetName() ?? value.ToString().ToLower(System.Globalization.CultureInfo.InvariantCulture);

        return result;
    }

```

I haven't been able to determine why the ToLower was added to this method.
It seems to be a breaking change that was not documented in the release notes.