```
@page "/learnblazor"

@*Using @bind:event="oninput" we can capture the users change on keystroke*@
<p>On Enter<input @bind="Name" @bind:event="oninput"/></p>
<p>@Name</p>

@code {
    private string Name = "";
}
```
