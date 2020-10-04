1. Add connection string to Appsettings.json
```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "ConnectionStrings": {
    "DefaultConnection": "Server=DESKTOP-86JBBTP;Database=BlazorFilms;Integrated Security=True; Trusted_Connection=Yes;"
  },
  "AllowedHosts": "*"
}
```
2. In shared project create Models.
```cs
namespace BlazorUde.Shared.Entities
{
    public class Movie
    {
        public int Id { get; set; }
        [Required]
        public string Trailer { get; set; }
        [Required]
        public string Title { get; set; }
        [Required]
        public DateTime? ReleaseDate { get; set; }
        [Required]
        public string Poster { get; set; }
        [Required]
        public string Summary { get; set; }
        public bool InTheaters { get; set; }
    }
}
```
3. Add EntityFrameworkCore to Server project.
```
Microsoft.EntityFrameworkCore.SqlServer
Microsoft.EntityFrameworkCore.Tools
```
4. Now in the server project create a new dir data and place your db context class inside.
```cs
public class ApplicationDbContext : DbContext
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {

        }
        public DbSet<Movie> Movies { get; set; }
    }
```
5. Now in ther server project register it in the Startup class
```cs
       public void ConfigureServices(IServiceCollection services)
        {
            services.AddDbContext<ApplicationDbContext>(opt =>
            {
                opt.UseSqlServer(Configuration.GetConnectionString("DefaultConnection"));
            });
            services.AddControllersWithViews();
            services.AddRazorPages();
        }
```
6. now run thy commands thee wise child of christ.
```
Add-Migration initial
Update-Database
```
7. Done!
