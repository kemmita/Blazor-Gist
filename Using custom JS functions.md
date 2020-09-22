1. In the client app, create a new js dir in wwwroot, then create a new js file, "CusotmJavaScript".
```js
function customFunc(msg){
    console.log(msg);
}
```
2. In your js extensions class create a new extension method that invokes your custom js funciton.
```cs
namespace BlazorUde.Client.Helpers
{
    public static class IjsRuntimeExtensions
    {
        public static async ValueTask MyCustomFunc(this IJSRuntime jsRuntime, string msg)
        {
            await jsRuntime.InvokeVoidAsync("customFunc", msg);
        }
    }
}
```
3. Now in our component we will call the js function.
```blazor
@inject IJSRuntime JsRuntime

<input type="checkbox" @bind="_showButtons" />

<GenericList List="Movies">
    <ElementTemplate Context="movie">
        <IndividualMovie Movie="movie" DisplayButtons="_showButtons" DeleteMovie="@(() => DeleteMovie(movie))" />
    </ElementTemplate>
</GenericList>

@code {
    [Parameter]
    public List<Movie> Movies { get; set; }
    bool _showButtons = false;

    private async Task DeleteMovie(Movie movie)
    {
        await JsRuntime.MyCustomFunc("testing");
    }
}
```
4. Lastly, include your js file in the index.html file
```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
    <title>BlazorUde</title>
    <base href="/" />
    <link href="css/bootstrap/bootstrap.min.css" rel="stylesheet" />
    <link href="css/app.css" rel="stylesheet" />
</head>

<body>
    <app>Loading...</app>

    <div id="blazor-error-ui">
        An unhandled error has occurred.
        <a href="" class="reload">Reload</a>
        <a class="dismiss">🗙</a>
    </div>
    <script src="_framework/blazor.webassembly.js"></script>
    <script src="js/CustomJavaScript.js"></script>
</body>
</html>
```
