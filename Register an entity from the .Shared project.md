1. The Shared project gives us a place to share code between client and server. Let's create a new model there.
```cs
namespace BlazorUde.Shared.Entities
{
    public class Movie
    {
        public string Title { get; set; }
        public DateTime ReleaseDate { get; set; }
    }
}
```
2. Now head to the client project and register the method in the _Import.razor file so that every component will have access to it.
```razor
@using BlazorUde.Shared.Entities
```
3. Now lets use that model in a component.
```razor
@page "/"

<h1>Movie Title: @_movie.Title</h1>

@code{

    readonly Movie _movie = new Movie
    {
        ReleaseDate = DateTime.Now,
        Title = "Spider Man"
    };
}

```
