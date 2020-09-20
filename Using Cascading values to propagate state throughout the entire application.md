1. Here in the MainLayout component we will declare a class called AppSate, this should be stored in its own file, but this is just a quick example.
```razor
@inherits LayoutComponentBase

<div class="sidebar">
    <NavMenu />
</div>

<div class="main">
    <div class="top-row px-4">
        <a href="http://blazor.net" target="_blank" class="ml-md-auto">About</a>
    </div>

    <div class="content px-4">
      @*Any component within the application will now have access to this state object*@
        <CascadingValue Value="@_appState">
            @Body
        </CascadingValue>
    </div>
</div>

@code
{
    private AppState _appState = new AppState();
    public class AppState
    {
        public string AppTheme { get; set; } = "Light";
    }
}
```
2. In the partial cs class of the child component we must create a cascading param
```cs
  public partial class Counter
    {
        [CascadingParameter] public MainLayout.AppState AppState { get; set; }
    }
```
3. Now in the child component we will use the app state and even update it via a dropdown.
```razor
@page "/counter"
    <div class="@(AppState.AppTheme == "Light" ? "bg-light" : "bg-dark")">
        <div style="display: flex">
            <div style="width: 150px">
                <select @bind="AppState.AppTheme">
                    <option value="Dark">Dark</option>
                    <option value="Light">Light</option>
                </select>
            </div>
        </div>
    </div>
```
