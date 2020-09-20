1. This will be our modal component. Only if _displayConfirmation is true will the modal appear.
```razor
@if (_displayConfirmation)
{
    <div class="modal-backdrop show"></div>

    <div class="modal fade show" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true" style="display: block;">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <h5>Confirm</h5>
                    <button class="close" @onclick="OnCancel" type="button">&times;</button>
                </div>
                <div class="modal-body">
                    @ChildContent
                </div>
                <div class="modal-footer">
                    <button class="btn btn-primary" @onclick="OnConfirm" type="button">Confirm</button>
                    <button class="btn btn-warning" @onclick="OnCancel" type="button">Cancel</button>
                </div>
            </div>
        </div>
    </div>
}

@code {
    private bool _displayConfirmation = false;
    [Parameter]
    public RenderFragment ChildContent { get; set; }
    [Parameter]
    public EventCallback OnConfirm { get; set; }
    [Parameter]
    public EventCallback OnCancel { get; set; }

    public void Show() => _displayConfirmation = true;
    public void Hide() => _displayConfirmation = false;
}
```
2. Here in the parent component we will include our modal
```razor
<input type="checkbox" @bind="_showButtons" />

<GenericList List="Movies">
    <ElementTemplate Context="movie">
        @*The DeleteMovie below is sent to a child comoonent as a eventcallback, when clicked in the child component that I will not show in this gist, it will trigger the function on line 62*@
        <IndividualMovie Movie="movie" DisplayButtons="_showButtons" DeleteMovie="@(() => DeleteMovie(movie))" />
    </ElementTemplate>
</GenericList>

<Confirmation @ref="_confirmation" OnCancel="OnCancel" OnConfirm="OnConfirm"> 
    <div>Do you wish to @_movieToDelete.Title</div>
</Confirmation>

@code {
    @*_confirmation field used to create a child component ref*@
    private Confirmation _confirmation;
    private bool _showButtons = false;
    private Movie _movieToDelete;
    [Parameter]
    public List<Movie> Movies { get; set; }


    private void DeleteMovie(Movie movie)
    {
        @*Grab a ref to the movie we want to cancel.*@ 
        _movieToDelete = movie;
        @*Then using a component ref defined on line 50, call the public Show method from the child component Modal to open the modal*@
        _confirmation.Show();
    }
    
    @*This is funciton that will be passed to the event call back method in the child component Modal, once executed there by a click action
    we will use the modal component ref to call a public function Hide to close the modal.*@
    private void OnCancel()
    {
        _confirmation.Hide();
        _movieToDelete = null;
    }

    @*This is funciton that will be passed to the event call back method in the child component Modal, once executed there by a click action
    we will use the modal component ref to call a public function Hide to close the modal.*@
    private void OnConfirm()
    {
        Movies.Remove(_movieToDelete);
        _confirmation.Hide();
        _movieToDelete = null;
    }
}
```
