```razor
@* Call a function in Razor *@
<p>@(name != null ? CustomToUpper(name) : "Default")</p>

@* Call a var in Razor *@
<p>@name</p>

@* Explicit expressions in Razor *@
<p>@(4 > 3 ? "Yes" : "No")</p>


@code{
    string name = "Jane";
    int nameLen;

    private string CustomToUpper(string str)
    {
        return str.ToUpper();
    }
}
```
