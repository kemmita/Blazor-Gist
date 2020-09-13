1. Below is the counter component.
```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>
<p>Current count: @Singleton.Value</p>
<p>Current count: @Transient.Value</p>
<p>Current count: @Scoped.Value</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

<input type="text" @bind="currentCount" />
```
2. Below is the partial class, belowe we define a field, method, and props that we will use to bring in our services.
```cs
 public partial class Counter
    {
        [Inject] TestServices.SingletonService Singleton { get; set; }
        [Inject] TestServices.TransientService Transient { get; set; }
        [Inject] TestServices.ScopedService Scoped { get; set; }

        private int currentCount = 0;

        private void IncrementCount()
        {
            currentCount++;
            Singleton.Value = currentCount;
            Transient.Value = currentCount;
            Scoped.Value = currentCount;
        }
    }
```
