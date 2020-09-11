1. Create helper dir can class in blazor client root.
```razor
namespace BlazorUde.Client.Helpers
{
    public class StringUtilities
    {
        public static string CustomToUpper(string str) => str.ToUpper();
    }
}
```
2. Now lets register it to be used by our components. Use _Imports.razor
```razor
@using BlazorUde.Client.Helpers
```
3. now call that helper funciton in a component.
```razor
@page "/"

@* Call the helper function *@
<p>@StringUtilities.CustomToUpper(name)</p>

@code{
    string name = "Jane";
}
```
