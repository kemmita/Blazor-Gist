0. Read forms with EditForm before looking at this code.
1. Create a Model Person
```cs
    public class Person
    {
        public int Id { get; set; }
        [Required]
        public string Name { get; set; }
        [Required]
        public string Biography { get; set; }
        [Required]
        public string Picture { get; set; }
        [Required]
        public DateTime? DateOfBirth { get; set; }
    }
```
2. Next go to nugget and include the Tewr.Blazor.FileReader package. Then register it in program.cs
```cs
public static async Task Main(string[] args)
        {
            var builder = WebAssemblyHostBuilder.CreateDefault(args);
            builder.RootComponents.Add<App>("app");
            builder.Services.AddFileReaderService(opt => opt.InitializeOnFirstCall = true);
            ConfigureServices(builder.Services);
            await builder.Build().RunAsync();
        }
```
3. Now we can use that package in the image component. I use a number system below to help guide the way of the code execution, begins with 1.
```razor
@using System.IO
@inject Tewr.Blazor.FileReader.IFileReaderService FileReaderService

<div>
    <label>@Label</label>
    <div>
        @* 1. Below we create a reference to an input element this will help us grab onto the image that was selected by the user. We can define accept and
          limit the types of files submitted. Finally, we use the onchange directive to watch for the selected action of the user.*@
        <input type="file" @ref="_inputElement" @onchange="ImageFileSelected" accept=".jpg,.jpeg,.png" />
    </div>
</div>
@*3. Now the image will be displayed.*@
<div>
    @if (_imageBase64 != null)
    {
        <div>
            <div class="m-1">
                <img src="data:image/jpeg;base64, @_imageBase64" alt="Alternate Text" style="width: 400px" />
            </div>
        </div>
    }
    @if (ImageUrl != null)
    {
        <div>
            <div class="m-1">
                <img src="data:image/jpeg;base64, @ImageUrl" alt="Alternate Text" style="width: 400px" />
            </div>
        </div>
    }
</div>

@code {

    [Parameter]
    public string Label { get; set; } = "Image";
    [Parameter]
    public string ImageUrl { get; set; }
    [Parameter]
    public EventCallback<string> OnSelectedImage { get; set; }
    private string _imageBase64;
    private ElementReference _inputElement;

    @* 2. *@
    private async Task ImageFileSelected()
    {
        foreach (var file in await FileReaderService.CreateReference(_inputElement).EnumerateFilesAsync())
        {
            await using var memoryStream = await file.CreateMemoryStreamAsync(4*1024);
            var imageBytes = new byte[memoryStream.Length];
            memoryStream.Read(imageBytes, 0, (int) memoryStream.Length);
            _imageBase64 = Convert.ToBase64String(imageBytes);
            @*After being converted to a base64 string it is sent the function in the form component below so that it can be provided as the string value
              in Person.Picture to store in the db.*@
            await OnSelectedImage.InvokeAsync(_imageBase64);
            ImageUrl = null;
            StateHasChanged();
        }
    }
}

```
4. Form component
```razor
<EditForm Model="Person" OnValidSubmit="OnValidSubmit">
    <DataAnnotationsValidator />
    <div>
        <div class="form-group">
            <label for="name">Name</label>
            <div>
                <InputText @bind-Value="Person.Name" class="form-control"/>
                <ValidationMessage For="@(() => Person.Name)" />
            </div>
        </div>
        <div class="form-group">
            <label for="dob">Dob</label>
            <div>
                <InputDate @bind-Value="Person.DateOfBirth" class="form-control"/>
                <ValidationMessage For="@(() => Person.DateOfBirth)" />
            </div>
        </div>
        <div class="form-group">
            <label for="bio">Bio</label>
            <div>
                <InputTextArea @bind-Value="Person.Biography" class="form-control"/>
                <ValidationMessage For="@(() => Person.Biography)" />
            </div>
        </div>
        <div class="form-group">
            <div>
                @*Below we had to create a custom component to select a image.*@
                <InputImage Label="Picture" ImageUrl="@_imageUrl" OnSelectedImage="PictureSelected"/>
                <ValidationMessage For="@(() => Person.Picture)" />
            </div>
        </div>
        <button class="btn btn-success" type="submit">Save</button>
    </div>
</EditForm>

@code {
    [Parameter]
    public EventCallback OnValidSubmit { get; set; }
    [Parameter]
    public Person Person { get; set; }

    private string _imageUrl;

    private void PictureSelected(string imageBase64)
    {
        Person.Picture = imageBase64;
        _imageUrl = null;
        Console.WriteLine(_imageUrl);
    }
}
```
5. Now lets finally use the form.
```razor
@page "/people/create"
<h3>Create</h3>

<PeopleForm OnValidSubmit="CreatePerson" Person="_person"/>

@code {
    private Person _person = new Person();

    private void CreatePerson()
    {
        Console.WriteLine($"{_person.DateOfBirth}, {_person.Name}, {_person.Biography}");
        Console.WriteLine("Redirect User.");
    }
}
```
