1. in the parent component, call the child component and pass the method to the child
```razor
@page "/parent"
    <h3>ParentComponent</h3>
@*Note the event callback ShowMessageOnClick, we will need to define this in the child*@
<BlazorEins.Pages.Components.ChildComponent ShowMessageOnClick="@ShowMessage"/>

<p>@MessageText</p>

@code {
    public string MessageText { get; set; }

    private void ShowMessage()
    {
        MessageText = "Text from parent";
    }
}
```
2. In the child component
```razor
<div class="panel panel-default">
    <p>Child component</p>
    @*We will execute the event callback onclick here in the child component*@
    <button class="btn btn-danger" @onclick="ShowMessageOnClick">Click</button>
</div>

@code {
    @*Define the event call back as a param*@
    [Parameter]
    public EventCallback ShowMessageOnClick { get; set; }
}
```
