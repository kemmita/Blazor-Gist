1. Create child component, ommit @page attribute and define the params you want passed from the parent.
```razor
<div class="panel panel-default">
    <p>@(Title ?? "temp")</p>
    <p>@(Content ?? "temp")</p>
    <button class="btn btn-danger">Click</button>
</div>

@code {
    [Parameter]
    public string Title { get; set; }
    [Parameter]
    public string Content { get; set; }
}
```
