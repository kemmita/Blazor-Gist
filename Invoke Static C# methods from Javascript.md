1. In your index.html file ref your custom js file in wwwroot/js/CustomJavaScript.js
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
2. Now head to your components partial file
```cs
namespace BlazorUde.Client.Pages
{
    public partial class Counter
    {
        // inject js runtime to call the custom function
        [Inject] private IJSRuntime Js { get; set; }

        private static int currentCountStatic = 0;
        private void IncrementCount()
        {
            currentCountStatic++;
            // Here we invoke the js method.
            Js.InvokeVoidAsync("dotnetStaticInvoke");
        }
        
        // Below is the static method we will invoke from js
        [JSInvokable]
        public static Task<int> GetCurrentCount()
        {
            return  Task.FromResult(currentCountStatic);
        }
    }
}
```
3. Below is the custom js file
```js
// We invoke the c# method using the name of the project and the name of the static method we created in the partial function above.
function dotnetStaticInvoke() {
    DotNet.invokeMethodAsync("BlazorUde.Client", "GetCurrentCount")
        .then(res => {
            console.log(res);
        });
}
```
