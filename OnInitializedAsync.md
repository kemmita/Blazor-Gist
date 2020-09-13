@page "/"


<div>
    <h1 class="text-center">Movies Yo</h1>
    <MovieList Movies="_movies"/>
</div>



@code{
    List<Movie> _movies;


    protected override async Task OnInitializedAsync()
    {
        await Task.Delay(3000);
        _movies = new List<Movie>
        {
            new Movie{ReleaseDate = DateTime.Now, Title = "Spider Man"},
            new Movie{ReleaseDate = DateTime.Now, Title = "Bat Man"},
            new Movie{ReleaseDate = DateTime.Now, Title = "Gold fang"}
        };
    }

}
