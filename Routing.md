1. Go to the App.razor file to view the ruter
```
<CascadingAuthenticationState>
    @* Router here *@
    <Router AppAssembly="@typeof(Program).Assembly">
        @*If the route is found this will render*@
        <Found Context="routeData">
            <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
        </Found>
        @*Else this will render*@
        <NotFound>
            <LayoutView Layout="@typeof(MainLayout)">
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </NotFound>
    </Router>
</CascadingAuthenticationState>
```
2. Create a new Razor component and define it's route name.
```html
@* The @page is used to define the route name *@
@page "/learnblazor"

<h3>LearnBlazor</h3>

@code {

}
```
3. Below we create a link that will take us to the component we created above.
```html
        <li class="nav-item px-3">
            <NavLink class="nav-link" href="learnblazor">
                <span class="oi oi-list-rich" aria-hidden="true"></span> Learn Blazor
            </NavLink>
        </li>
```
4. Working with query params. We will not show the link that directs to this page as nothing is different from what is shown above. Below is the new component.
```html
@*An example: www.example.com/query?Parameter1=Fish -- based off of this example, wehn the page is rendered, the page will read First Parameter: Fish*@
@*Setting up the route name is still the same. We must inject Naviation Manager here.*@
@page "/query"
@inject NavigationManager NavManager

<h3>LearnRazorQuery</h3>
<p>First Parameter: @(Parameter1 ?? "Empty")</p>

@code {
    @*Create prop for param*@
    private string Parameter1 { get; set; }

    @*override OnInitialized so that the html is rendered correctly given a parameter value passed.*@
    protected override void OnInitialized()
    {
        var absUri = new Uri(NavManager.Uri);
        var queryParam = System.Web.HttpUtility.ParseQueryString(absUri.Query);

        Parameter1 = queryParam["Parameter1"];
    }
}
```
