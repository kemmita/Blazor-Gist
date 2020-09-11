```razor
@page "/"

@for (int i = 0; i < _movies.Count; i++)
{
    <h1>Movie Title: @_movies[i].Title</h1>
    
    <button class="@(i % 2 == 0 ? "btn-danger":"btn-info")"></button>
}

@code{
    List<Movie> _movies;

    protected override void OnInitialized()
    {
        _movies = new List<Movie>
        {
            new Movie{ReleaseDate = DateTime.Now, Title = "Spider Man"},
            new Movie{ReleaseDate = DateTime.Now, Title = "Bat Man"},
            new Movie{ReleaseDate = DateTime.Now, Title = "Gold fang"}
        };
    }


}
```
