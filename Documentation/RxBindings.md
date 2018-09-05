# RxBindings
RxBindings can simplify connecting the UI fields with the View Model Input/Output. Furthermore, with the help of Kotlin extension functions, this can save lots of time and code.

### Extension functions

The following extension functions are very helpful at minimizing the amount of code. The examples and explanations from this point on refer to these functions.
```

var View.rxClick: Observable<Unit>
    get() = RxView.clicks(this).map { Unit }
    set(value) {}


var EditText.rxTextChanges: Observable<String>
    get() = RxTextView.textChanges(this).map { it.toString() }
    set(value) {}


```

## Output
 An extension function from the View Model Output to the View Binding can be made and would look something like this:

```
private fun YourViewModelOutput.bind(binding: DialogFragmentYourBinding): List<Disposable> {
    return listOf(
            //put your bindings here
    )
}
```
For example, assume a View is visible/gone depending on a View Model output stream:
```
isUpdating.subscribe(binding.commentEditLl.visibility()),
```
Every time that the stream emits a new value, the visibility of the view will be changed.

## Input

The same applies for the Input: an extension function is created accepting the View Binding as a parameter:
```

private fun YourViewModelInput.bind(binding: DialogFragmentYourBinding): List<Disposable> {
    return listOf(
      //put your bindings here
)
```
__Note:__ The code snippets shown below are placed within the upper code block, hence *this* refers to the ViewModelInput in this case.

This is the place to connect any On Click Listeners or Text Watchers. For example, let's assume we have a submit button that needs to communicate to the view model the intention of the user:

```
   binding.yourButton.rxClick.subscribe { this.update() }
```
Or, we have an Edit Text whose text will be used in the View Model to validate data and eventually submit it:

```
  binding.yourEditText.rxTextChanges.subscribe { this.setCommentText(it) }
```
