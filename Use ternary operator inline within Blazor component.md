```
@page "/learnblazor"
<p>On Enter<input @bind="Name" @bind:event="oninput"/></p>
<p>@(string.IsNullOrEmpty(Name)? "____": Name)</p>

@code {
    private string Name;
}
```
