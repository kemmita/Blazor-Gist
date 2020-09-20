1. If we want to have a window popup on the browser confirming if they want to delete an element from a list, we can use js funcitons like confirm. Create a extension class for the ijs runtime
```
namespace BlazorUde.Client.Helpers
{
    public static class IjsRuntimeExtensions
    {
        public static async ValueTask<bool> Confirm(this IJSRuntime jSRuntime, string message)
        {
            return await jSRuntime.InvokeAsync<bool>("confirm", message);
        }
    }
}
```
2. Now lets use that extension method in a component.
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
        var confirmed = await JsRuntime.Confirm($"Are you sure you want to delete {movie.Title}");
        if(confirmed)
            Movies.Remove(movie);
    }

}
```
