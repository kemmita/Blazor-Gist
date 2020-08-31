1. It starts in the Pages/_Host.cshtml file. Note the "App" component being specified.
```html
    <app>
        <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>
```
2. Note the default layout on line 12. This layout will be used if specified here. 
```
    <CascadingAuthenticationState>
        @* This is the router and based off of what is typed in the browser it will render the correct component. *@
        <Router AppAssembly="@typeof(Program).Assembly">
            <Found Context="routeData">
                @* You can specify a default layout *@
                <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
            </Found>
            <NotFound>
                <LayoutView Layout="@typeof(NewLayout)">

                </LayoutView>
            </NotFound>
        </Router>
    </CascadingAuthenticationState>
```
3. Below is a layout page
```html
@inherits LayoutComponentBase

<div class="sidebar">
    <NavMenu />
</div>

<div class="main">
    <div class="top-row px-4 auth">
        <LoginDisplay />
        <a href="https://docs.microsoft.com/aspnet/" target="_blank">About</a>
    </div>

    <div class="content px-4">
        @* Here we define @Body now any component we register will be displayed below *@
        @Body
    </div>
</div>
```
4. Below is a component that is rendered in the layout page.
```html
@page "/counter"

    <NavLink class="nav-link" href="fetchdata">
        <span class="oi oi-list-rich" aria-hidden="true"></span> Fetch data
    </NavLink>

    <h1>Counter</h1>

    <p>Current count: @currentCount</p>

    <button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

    @code {
        private int currentCount = 0;

        private void IncrementCount()
        {
            currentCount++;
        }
    }
```
5. A component may choose to have a different main layout 
```html
@layout NewLayout
@page "/counter"

    <NavLink class="nav-link" href="fetchdata">
        <span class="oi oi-list-rich" aria-hidden="true"></span> Fetch data
    </NavLink>

    <h1>Counter</h1>

    <p>Current count: @currentCount</p>

    <button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

    @code {
        private int currentCount = 0;

        private void IncrementCount()
        {
            currentCount++;
        }
    }

```
