1. Client component Create Genre
```razor
@page "/genre/create"
@*Inject our Genre Service that we will create later in the walkthrough*@
@inject IGenreService GenreService
@inject NavigationManager NavigationManager
<h3>Create</h3>

<GenreForm Genre="_genre" OnValidSubmit="CreateGenre" />

@code {
    private Genre _genre = new Genre();

    private async Task CreateGenre()
    {
        try
        {
            Console.WriteLine($"Stored {_genre.Name} in db");
            @*When the form is submitted, we will call our CreateGenreAsync method from ther service injected above.*@
            await GenreService.CreateGenreAsync(_genre);
            NavigationManager.NavigateTo("genres");
        }
        catch (Exception e)
        {
            Console.WriteLine(e);
            throw;
        }
    }
}
```
2. Create the interface on the clinet side
```cs
namespace BlazorUde.Client.Interfaces
{
    public interface IGenreService
    {
        Task CreateGenreAsync(Genre genre);
    }
}
```
3. Now create the service on the client side
```cs
namespace BlazorUde.Client.Services
{
    public class GenreService : IGenreService
    {
        // Inject the HttpService that we will create next.
        private readonly IHttpService _httpService;
        private readonly string _url;

        public GenreService(IHttpService httpService)
        {
            _httpService = httpService;
            _url = "api/genres";
        }
        public async Task CreateGenreAsync(Genre genre)
        {
            // call the post method from our httpservice. Pass the Url and data
            var response = await _httpService.Post(_url, genre);
            if (!response.Success)
                throw new ApplicationException(await response.GetBody());
        }
    }
}
```
4. Now in the clinet project create an IHttpService interface.
```cs
namespace BlazorUde.Client.Interfaces
{
    public interface IHttpService
    {
        // We will create the HttpResponseWrapper later on in the walkthrough
        Task<HttpResponseWrapper<object>> Post<T>(string url, T data);
    }
}
```
5. Now in the clinet project create the HttpService.
```cs
namespace BlazorUde.Client.Services
{
    public class HttpService : IHttpService
    {
        private readonly HttpClient _http;
        public HttpService(HttpClient http)
        {
            _http = http;
        }

        public async Task<HttpResponseWrapper<object>> Post<T>(string url, T data)
        {
            var dataJson = JsonSerializer.Serialize(data);
            var stringContent = new StringContent(dataJson, Encoding.UTF8, "application/json");
            var response = await _http.PostAsync(url, stringContent);
            return new HttpResponseWrapper<object>(null, response.IsSuccessStatusCode, response);
        }
    }
}
```
6. Now lets create the HttpResponseWrapper
```cs
namespace BlazorUde.Client.Helpers
{
    public class HttpResponseWrapper<T>
    {
        public HttpResponseWrapper(T response, bool success, HttpResponseMessage httpResponseMessage)
        {
            Success = success;
            Response = response;
            HttpResponseMessage = httpResponseMessage;
        }
        public bool Success { get; set; }
        public T Response { get; set; }
        public HttpResponseMessage HttpResponseMessage { get; set; }

        public async Task<string> GetBody()
        {
            return await HttpResponseMessage.Content.ReadAsStringAsync();
        }
    }
}
```
7. Now we need to register the services in the client's Program.cs file. Make sure to read my gist on injection within the client before doing this.
```cs
        private static void ConfigureServices(IServiceCollection services)
        {
            services.AddOptions();
            services.AddScoped<IHttpService, HttpService>();
            services.AddScoped<IGenreService, GenreService>();
        }
```
8. Lastly it will hit the controller based off of line 54 of this gist.
```cs
namespace BlazorUde.Server.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class GenresController : ControllerBase
    {
        private readonly ApplicationDbContext _db;

        public GenresController(ApplicationDbContext db)
        {
            _db = db;
        }

        [HttpPost]
        public async Task<ActionResult> Post(Genre genre)
        {
            await _db.Genres.AddAsync(genre); 
            await _db.SaveChangesAsync();
            return Ok();
        }
    }
}
```
9. Post with return value, HttpService
```cs
        public async Task<HttpResponseWrapper<TResponse>> Post<T, TResponse>(string url, T data)
        {
            var dataJson = JsonSerializer.Serialize(data);
            var stringContent = new StringContent(dataJson, Encoding.UTF8, "application/json");
            var response = await _http.PostAsync(url, stringContent);
            if (response.IsSuccessStatusCode)
            {
                var deserializedResponse = await Deserialize<TResponse>(response, DefaultJsonSerializerOptions);
                return new HttpResponseWrapper<TResponse>(deserializedResponse, response.IsSuccessStatusCode, response);
            }
            return new HttpResponseWrapper<TResponse>(default,response.IsSuccessStatusCode, response);
        }
        
        private async Task<T> Deserialize<T>(HttpResponseMessage httpResponse, JsonSerializerOptions options)
        {
            var responseString = await httpResponse.Content.ReadAsStringAsync();

            return JsonSerializer.Deserialize<T>(responseString, options);
        }
```
10. Get 1. Http Service
```cs
         public async Task<HttpResponseWrapper<T>> Get<T>(string url)
        {
            var response = await _http.GetAsync(url);
            if (response.IsSuccessStatusCode)
            {
                var deserializedResponse = await Deserialize<T>(response, DefaultJsonSerializerOptions);
                return new HttpResponseWrapper<T>(deserializedResponse, response.IsSuccessStatusCode, response);
            }
            return new HttpResponseWrapper<T>(default, response.IsSuccessStatusCode, response);
        }

        private async Task<T> Deserialize<T>(HttpResponseMessage httpResponse, JsonSerializerOptions options)
        {
            var responseString = await httpResponse.Content.ReadAsStringAsync();

            return JsonSerializer.Deserialize<T>(responseString, options);
        }
```
11. Get 2. Genre Service
```cs
 public class GenreService : IGenreService
    {
        private readonly IHttpService _httpService;
        private readonly string _url;

        public GenreService(IHttpService httpService)
        {
            _httpService = httpService;
            _url = "api/genres";
        }
        public async Task<List<Genre>> ListGenres()
        {
            var response = await _httpService.Get<List<Genre>>($"{_url}/list");
            if(!response.Success)
                throw new ApplicationException(await response.GetBody());
            return response.Content;
        }
    }
```
12. Get 3. Component using Genre Service.
```rtazor
@page "/genres"
@inject IGenreService GenreService
<h3>GenresList</h3>

<div class="list-group">
    @if (_genres != null)
    {
        @foreach (var genre in _genres)
        {
            <a href="#" class="list-group-item list-group-item-action disabled">@genre.Name</a>
        }
    }
</div>

@code {
    private IList<Genre> _genres;

    protected override async Task OnInitializedAsync()
    {
        _genres = await GenreService.ListGenres();
    }

}
```
13. Get 4. IMPORTANT!!! Controller Server
```cs
 // There were no other endpoints of type get so I thought I could user get without adding anything to the route. This weirdly did not work on IIS, so I had to add
 // www.test.com/genres/list to get it to work with the blazor client. 
 [HttpGet("list")]
 public async Task<ActionResult<List<Genre>>> ListGenres()
 {
      return await _db.Genres.ToListAsync();
 }
```
