# Data Binding
### Usage
 Each XML file should wrap its layout (e.g. Constraint Layout, Linear layout etc.) in a `<layout>` tag, providing the variables in a `<data>` tag.
 For example, the base of the XML would look like this:

 ```

 <?xml version="1.0" encoding="utf-8"?>
 <layout>

     <data>

         <variable
             name="var1"
             type="String" />
     </data>

     <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
         android:layout_width="match_parent"
         android:layout_height="match_parent"
         android:orientation="vertical">
         <!--Root Layout-->
     </LinearLayout>

 </layout>

  ```

  At this point a new file will be built by the compiler bearing the name of the layout file with no underscores. For example, *activity_login* would generate a file with the name *ActivityLoginBinding*. This is the name that has to be referenced in the Activity itself:

`val binding: ActivityLoginBinding = DataBindingUtil.setContentView(this, R.layout.activity_login)`

The `var1` variable declared in the XML would be set as following:

`binding.var1 = "the value you'd like to set" `

or

`binding.setVariable(BR.var1, "the value you'd like to set")`

and can be used in such manner:
```
      <TextView
           android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="@{var1}" />
```

#### The Bad
Sometimes the XML has to respond in a complex manner to changes in variables, or a 3rd party library is involved in mapping the variable to the end UI. This would make the file difficult to read and some things are impossible to write in the XML. Binding Adapters and Binding Conversions come in handy when dealing with such cases.

### Binding Adapters and Binding Conversions
Adapters and Conversions can be used with binding variables to reduce the complexity of the code in the XML file itself. They must be declared outside of a class body in a file, or a new file consisting solely of Adapters and Conversions can be created.

__Binding Adapters__

Binding adapters are annotated with *@BindingAdapter* are used to apply some sort of transformation or add any effect based on value changes to an Android View.

For example, assume you have to load an image from an URL to an ImageView. It's quite probable that you'd use [Glide](https://github.com/bumptech/glide) or [Picasso](http://square.github.io/picasso/). The following can be done:

```
@BindingAdapter("bind:user_image")
fun setUserImage(imageView: ImageView, imageUrl: String?) {
    Glide.with(imageView)
            .load(imageUrl)
            .into(imageView)
}
```

Assuming you have such ImageView somewhere, it would look similar to this:
```
      <ImageView
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          bind:user_image="@{userPhoto}" />

```
The Binding Adapter will be called every time the `userPhoto` variable is set.

__Binding Conversions__

Binding Conversions are annotated with *@BindingConversion* and are used to automatically convert from the expression type to the value used in the setter. The converter should take one parameter, the expression type, and the return value should be the target value type used in the setter.

A very frequent situation is the one where a view has to change its visibility based on a Boolean. The Conversion in this case would be very simple:

```
@BindingConversion
fun convertBooleanToVisibility(visible: Boolean): Int {
    return if (visible) View.VISIBLE else View.GONE
}
```

Both Binding Adapters and Binding Conversions can be debugged.
