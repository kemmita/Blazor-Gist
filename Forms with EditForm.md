1. Create a Model that we will use to create and edit data in the database.
```cs
public class Genre
    {
        public int Id { get; set; }
        // by setting required and the error message here, we can use Blazor's DataAnnotationsValidator and ValidationMessage component within our form.
        [Required(ErrorMessage = "This Name Field Is Required Jack!")]
        public string Name { get; set; }
    }
```
2. We will create a form component that we will use for both edit and create functionality.
```blazor
@*The EditForm component requires a model to work we use their OnValidSubmit to trigger a call back that will communicate with the edit and create components.*@
<EditForm Model="Genre" OnValidSubmit="OnValidSubmit">
    @*Include the validator below to validate against our rules defined in the model above.*@
    <DataAnnotationsValidator />
    <div>
        <div class="form-group">
            <label>Genre Name</label>
            @*Using Blazors input text, we simply need to bind the value we will be working with.*@
            <InputText class="form-control" @bind-Value="Genre.Name" />
            @*Below is how we include our custom error message we defined in the Model*@
            <ValidationMessage For="@(() => Genre.Name)" />
        </div>
        <button class="btn btn-success" type="submit">Save</button>
    </div>
</EditForm>

@code {
    @*A instantiated Genre must be sent to this component to work.*@
    [Parameter]
    public Genre Genre { get; set; }
    @*EventCallback for the successfull submission of either edit or create*@
    [Parameter]
    public EventCallback OnValidSubmit { get; set; }
}
```
3. Create component.
```razor
@page "/genre/create"
<h3>Create</h3>

<GenreForm Genre="_genre" OnValidSubmit="CreateGenre" />

@code {
    @*Create new instance of Genre to be passed to our form component*@
    private Genre _genre = new Genre();
    
    @*Store new Genre to db on successful submission*@
    private void CreateGenre()
    {
        Console.WriteLine($"Stored {_genre.Name} in db");
    }
}
```
4. Edit component.
```razor
age "/genre/edit/{GenreId:int}"

<h3>Edit</h3>

@if (_genre != null)
{
    <GenreForm Genre="_genre" OnValidSubmit="EditGenre" />
}

@code {
    [Parameter]
    public int GenreId { get; set; }
    private Genre _genre;

    private void EditGenre()
    {
        Console.WriteLine($"Updated to {_genre.Name}");
    }

    protected override void OnInitialized()
    {
        _genre = new Genre { Id = 1, Name = "Action" };
    }
}
```
