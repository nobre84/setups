## Distribute Android builds via Crashlytics Beta

*If you want to confirm that `fastlane` is installed before proceeding, open the Terminal and run `fastlane -v`. You should see the current version of `fastlane` displayed.*

Navigate to your Android project directory, run `fastlane init`, and follow the prompts.

The `init` will create the following:

- A fastlane directory within your project directory
  - fastlane/Fastfile, which stores your deployment pipelines (lanes)
  - fastelane/Appfile, which stores your package name

If you're looking for your Crashlytics Beta API key and build secret, select your org on the [organizations settings page](https://www.fabric.io/settings/organizations), and click on the API key and build secret links right underneath your org’s name.
In this example, the API key and build secret are hardcoded in the Fastfile; refer to this [guide](https://github.com/fastlane/setups/blob/master/Keys.md) for best practices regarding managing your keys.

This lane uses two fastlane [actions](https://github.com/fastlane/fastlane/blob/master/fastlane/docs/Actions.md): `gradle` and `crashlytics`. The `gradle` action executes Gradle tasks, while the `crashlytics` actions calls the Gradle [crashlyticsUploadDistributionRelease](https://docs.fabric.io/android/beta/gradle.html) task behind the scenes to upload the apk file.

Open the fastlane/Fastfile in your preferred text editor, change the syntax highlighting to Ruby, and find the `beta` lane. You will need pass `api_token`, `build_secret`, `groups`, and `notes_path` as arguments to the `crashlytics` action.

```ruby
desc "Submit a new Beta Build to Crashlytics Beta"
 lane :beta do
   gradle(task: "assembleRelease")
   crashlytics(
     api_token: "...",
     build_secret: "...",
     groups: "qa-team",
     notes_path: "./release-notes.txt"
     # apk_path: "..." Path to your APK file
     # crashlytics_path: "..." Path to `crashlytics-devtools.jar` file
     # emails: "..." Pass email addresses of testers, separated by commas
     # notifications: "..." Crashlytics notification option (true/false) (default: 'true')
     # debug: "..." Crashlytics debug option (true/false)
   )
 end
```

Run `fastlane android beta` to execute the lane.

To see the full list of options for the crashlytics action, run `fastlane action crashlytics`.
