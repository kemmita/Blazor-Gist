1. Here in our MovieList component, we pass our individualMovie component as a render fragment to the GenericList component. By doing this we can house all the loading and error display for list in one compoenent. So we do not need to repeat it for each list.
```razor
<GenericList List="Movies">
    <ElementTemplate Context="movie">
        <IndividualMovie Movie="movie" />
    </ElementTemplate>
</GenericList>

@code {
    [Parameter]
    public List<Movie> Movies { get; set; }
    bool _showButtons = false;

    void DeleteMovie(Movie movie)
    {
        Movies.Remove(movie);
    }
}
```
2. 
```razor
@*Import typeparam*@
@typeparam TItem

@if (List == null)
{
    <img src="/loading.gif" width="80px" />
}
else if (List.Count == 0)
{
    <p>There are no records found.</p>
}
else
{
    @*For each item in the generic list*@
    @foreach (var element in List)
    {
        @*Render the indivualMovie || indiviudalGeneric component*@
       @ElementTemplate(element)
    }
}

@code {
    @*Create a param to pass in the list*@
    [Parameter]
    public List<TItem> List { get; set; }
    @*Create the generic render frag to pass in the IndividualMovie component*@
    [Parameter]
    public RenderFragment<TItem> ElementTemplate { get; set; }
}
```
