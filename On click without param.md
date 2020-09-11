```razor
@* On click method *@
<button @onclick="@LogThis">Trigger void method on click!</button>

@code{

    private void LogThis()
    {
        Console.WriteLine("Log this");
    }
}
```
