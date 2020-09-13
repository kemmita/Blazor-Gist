1. Create an interface.
```cs
    public interface IMovieService
    {
        Task<List<Movie>> GetMoviesAsync();
    }
```
2. Create a hard class
```cs
 public class MovieService : IMovieService
    {
        public async Task<List<Movie>> GetMoviesAsync()
        {
            await Task.Delay(3000);
            return new List<Movie>
            {
                new Movie{ReleaseDate = DateTime.Now, Title = "Spider Man"},
                new Movie{ReleaseDate = DateTime.Now, Title = "Bat Man"},
                new Movie{ReleaseDate = DateTime.Now, Title = "Gold fang"}
            };
        }
    }
```
3. Add Service and Interface dirs to _Imports.razor
```razor
@using BlazorUde.Client.Services
@using BlazorUde.Client.Interfaces
```
4. Configure DI in Program.cs as Setup.cs is no longer availble in client. One done register your service.
```cs
    public class Program
    {
        public static async Task Main(string[] args)
        {
            var builder = WebAssemblyHostBuilder.CreateDefault(args);
            builder.RootComponents.Add<App>("app");

            builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });
            ConfigureServices(builder.Services);
            await builder.Build().RunAsync();
        }

        private static void ConfigureServices(IServiceCollection services)
        {
            services.AddOptions();
            services.AddScoped<IMovieService, MovieService>();
        }
    }
```
5. Now lets use that service inside a component.
```razor
@page "/"
@*Inject services here
@inject IMovieService MovieService

<div>
    <h1 class="text-center">Movies Yo</h1>
    <MovieList Movies="_movies"/>
</div>

@code{
    List<Movie> _movies;
    protected override async Task OnInitializedAsync()
    {
        @*Calling Service*@
        _movies = await MovieService.GetMoviesAsync();
    }
}

```
