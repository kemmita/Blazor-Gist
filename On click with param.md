```razor
<p>@name</p>

@* On click method with parameter *@
<button @onclick="@(() =>NameLength(name))">Return Name Length</button>

@(nameLen > 0 ? nameLen : 0)

@code{
    string name = "Jane";
    int nameLen;

    private void NameLength(string namePassed)
    {
        nameLen = namePassed.Length;
    }
}

```
