1. In the same dir, we want to create a code file for our razor component LearnBlazor.razor. Name the file LearnBlazor.razor.cs. You will then need to modify the class name in
your code to LearnBlazorBase which in turn will inherit from ComponentBase. Make your props and functions protected so that when our component inherits from the class below
we can use them.
```cs
    public class LearnBlazorBase : ComponentBase
    {
        protected string Name;
        protected string WelcomeText = "Time to Learn Blazor";
        protected void GetName()
        {
            Name = "Bob";
        }
    }
```
2. Below is the component
```razor
@inherits LearnBlazorBase
@page "/learnblazor"
<h3 class="text-info pb-3">@WelcomeText</h3>


<p>On Enter<input @bind="Name" /></p>

<button @onclick="GetName">Click</button>

<p>@(string.IsNullOrEmpty(Name)? "____": Name)</p>
```
