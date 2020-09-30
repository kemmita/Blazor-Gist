```razor
@page "/movie/search"
@inject IMovieService MovieService

<h3>MovieFilter</h3>

<div class="form-inline">
    <div class="form-group mb-2">
        <label for="title" class="sr-only">Title</label>
        <input type="text" id="title" placeholder="Movie Title" class="form-control"
               @bind-value="_title"
               @bind-value:event="oninput"
               @onkeypress="@((KeyboardEventArgs e) => TitleKeyPress(e))" />
    </div>
    <div class="form-group mx-sm-3 mb-2">
        <select class="form-control" @bind="_selectedGenre">
            <option value="0">Genres</option>
            @foreach (var genre in _genres)
            {
                <option value="@genre.Id">@genre.Name</option>
            }
        </select>
    </div>
    <div class="form-group mx-sm-3 mb-2">
        <input type="checkbox" class="form-check-input" id="upcomingReleases" @bind="_upcomingReleases" />
        <label class="form-check-label" for="upcomingReleases">Upcoming Releases</label>
    </div>
    <div class="form-group mx-sm-3 mb-2">
        <input type="checkbox" class="form-check-input" id="inTheaters" @bind="_inTheaters" />
        <label class="form-check-label" for="inTheaters">In Theaters</label>
    </div>

    <button type="button" class="btn btn-primary mb-2 mx-sm-3" @onclick="Search">Filter</button>
    <button type="button" class="btn btn-danger mb-2" @onclick="Clear">Clear</button>
</div>

<MovieList Movies="_movies" />

@code {
    private string _title = "";
    private string _selectedGenre = "0";
    private List<Genre> _genres = new List<Genre> { new Genre { Id = 1, Name = "Action" }, new Genre { Id = 2, Name = "Horror" } };
    private bool _upcomingReleases = false;
    private bool _inTheaters = false;
    private List<Movie> _movies;

    protected override async Task OnInitializedAsync()
    {
        _movies = await MovieService.GetMoviesAsync();
    }

    private void TitleKeyPress(KeyboardEventArgs e)
    {
        if (e.Key == "Enter")
        {
            Search();
        }
    }

    private void Search()
    {
        _movies = _movies.Where(x => x.Title == _title).ToList();
        Console.WriteLine($"Search for this {_title} {_inTheaters} {_upcomingReleases} {_selectedGenre}");
    }

    private async Task Clear()
    {
        _movies = await MovieService.GetMoviesAsync();
        _title = "";
        _selectedGenre = "0";
        _upcomingReleases = false;
        _inTheaters = false;
    }
}

```
