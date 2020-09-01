1. We bind the input below to the Name prop that we defined in the code seciton of the component.
```
@page "/learnblazor"

@*By default the the name below the input field will not be updated until you hit enter*@
<p>On Enter<input @bind="Name" /></p>
<p>@Name</p>

@code {
    private string Name = "";
}
```
