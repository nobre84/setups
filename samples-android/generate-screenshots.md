## Take screenshots of your Android app

*If you want to confirm that `fastlane` is installed before proceeding, open the Terminal and run `fastlane -v`. You should see the current version of `fastlane` displayed.*

Navigate to your Android project directory, run `fastlane init`, and follow the prompts. The command will create the following:

- A fastlane directory within your project directory
  - fastlane/Fastfile, which stores your deployment pipelines (lanes)
  - fastelane/Appfile, which stores your package name

Then, run `screengrab init`, and follow the prompts. The command will create the following:

- A fastlane/Screengrabfile, which stores the `screengrab` config

This lane uses two fastlane [actions](https://github.com/fastlane/fastlane/blob/master/fastlane/docs/Actions.md): `gradle` and `screengrab`. The `gradle` action executes Gradle tasks, while the `screengrab` invokes the `screengrab` [tool](https://github.com/fastlane/fastlane/tree/master/screengrab) which automates taking of localized screenshots.

To setup your project for `screengrab`, do the following:

1. Add the Gradle dependency
    ```java
    androidTestCompile 'tools.fastlane:screengrab:0.3.0'
    ```

2. Configure your Manifest Permissions
    Ensure that the following permissions exist in your **src/debug/AndroidManifest.xml**

    ```xml
    <!-- Allows unlocking your device and activating its screen so UI tests can succeed -->
    <uses-permission android:name="android.permission.DISABLE_KEYGUARD"/>
    <uses-permission android:name="android.permission.WAKE_LOCK"/>

    <!-- Allows for storing and retrieving screenshots -->
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />

    <!-- Allows changing locales -->
    <uses-permission android:name="android.permission.CHANGE_CONFIGURATION" />
    ```

3. Configure your [UI Tests](https://github.com/fastlane/fastlane/blob/master/screengrab/README.md#ui-tests) for screenshots
    - Add `@ClassRule public static final LocaleTestRule localeTestRule = new LocaleTestRule();` to your tests class to handle automatic switching of locales
    - To capture screenshots, add the following to your tests `Screengrab.screenshot("name_of_screenshot_here");` on the appropriate screens


Then, open the fastlane/Fastfile in your preferred text editor, change the syntax highlighting to Ruby, and overwrite the contents with the following:

```ruby
default_platform :android

platform :android do
  lane :screenshots do
    gradle(task: "assembleDebug assembleAndroidTest")
    screengrab
  end
end
```

Finally, open the fastlane/Screengrabfile in your preferred text editor, change the syntax highlighting to Ruby, and set the devices and languages that you want `screengrab` to use.

Run `fastlane android screenshots` to execute the lane. By default, screenshots will be generated in the fastlane/screenshots directory.

To see the full list of options for generating screenshots, run `fastlane action screengrab`.
