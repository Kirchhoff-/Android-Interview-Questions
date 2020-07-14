# Robolectric
Robolectric is the industry-standard unit testing framework for Android. With Robolectric, your tests run in a simulated Android environment inside a JVM, without the overhead of an emulator.

Unlike traditional emulator-based Android tests, Robolectric tests run inside a sandbox which allows allows the Android environment to be precisely configured to the desired conditions for each test, isolates each test from its neighbors, and extends the Android framework with test APIs which provide minute control over the Android framework’s behavior and visibility of state for assertions.

While much of the Android framework will work as expected inside a Robolectric test, some Android components’ regular behavior doesn’t translate well to unit tests: hardware sensors need to be simulated, system services need to be loaded with test fixture data. In those cases, Robolectric provides a test double that’s suitable for most unit testing scenarios. Robolectric replaced all Android classes by so-called shadow objects. If a method is implemented by Robolectric, it forwards these method calls to the shadow object. Shadow objects behave similar to the Android implementation. If a method is not implemented by the shadow object, it simply returns a default value, e.g., `null` or 0.

Robolectric handles inflation of views, resource loading, and lots of other stuff that’s implemented in native C code on Android devices. This allows tests to do most things you could do on a real device. It’s easy to provide your own implementation for specific SDK methods too, so you could simulate error conditions or real-world sensor behavior, for example. 

Robolectric allows a test style that is closer to black box testing, making the tests more effective for refactoring and allowing the tests to focus on the behavior of the application instead of the implementation of Android. 

Example of test:
```
@RunWith(AndroidJUnit4.class)
public class MyActivityTest {

  @Test
  public void clickingButton_shouldChangeResultsViewText() throws Exception {
    Activity activity = Robolectric.setupActivity(MyActivity.class);

    Button button = (Button) activity.findViewById(R.id.press_me_button);
    TextView results = (TextView) activity.findViewById(R.id.results_text_view);

    button.performClick();
    assertThat(results.getText().toString(), equalTo("Testing Android Rocks!"));
  }
}
```

## Links
http://robolectric.org/  
https://github.com/robolectric/robolectric  
https://www.vogella.com/tutorials/Robolectric/article.html  
