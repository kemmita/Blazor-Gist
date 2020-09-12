```razor
@* Call a function in Razor *@
<p>@(name != null ? CustomToUpper(name) : "Default")</p>

@* Call a var in Razor *@
<p>@name</p>

@* Explicit expressions in Razor *@
<p>@(4 > 3 ? "Yes" : "No")</p>

@* Foreach syntax *@
@foreach (var movie in _movies)
{
    <h1>Movie Title: @movie.Title</h1>
}

@* For syntax*@
@for (int i = 0; i < _movies.Count; i++)
{
    <h1>Movie Title: @_movies[i].Title</h1>
}

@* If Else syntax *@
@if (_movies != null)
{
    @foreach (var movie in _movies)
    {
        <h1>Movie Title: @movie.Title</h1>
    }
}
else
{
    <p>Loading...</p>
}

@code{
    string name = "Jane";
    int nameLen;

    private string CustomToUpper(string str)
    {
        return str.ToUpper();
    }
    
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
