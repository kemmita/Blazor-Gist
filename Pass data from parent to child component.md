1. Create child component, ommit @page attribute and define the params you want passed from the parent.
```razor
<div class="panel panel-default">
    <p>@(Title ?? "temp")</p>
</div>

@code {
    @*provide the prop the [Parameter] attribute*@
    [Parameter]
    public string Title { get; set; }
}
```
2. Reference child component in page
```razor
@page "/parent"
<h3>ParentComponent</h3>

@*You will need to fully qualify the path if the child and parent do not share the same dir*@
@*Use the prop name "Title" that was defined in the child component, then simply pass a value from the parent.*@
<BlazorEins.Pages.Components.ChildComponent Title="@MyProperty"/>

@code {
    public string MyProperty { get; set; } = "Cheese";
}
```
