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
