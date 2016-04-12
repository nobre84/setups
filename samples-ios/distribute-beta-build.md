## Distribute iOS builds via Crashlytics Beta

*If you want to confirm that `fastlane` is installed before proceeding, open the Terminal and run `fastlane -v`. You should see the current version of `fastlane` displayed.*

Navigate to your iOS project directory, run `fastlane init`, and follow the prompts.

The `init` will create the following:

- A fastlane directory within your project directory
  - fastlane/Fastfile, which stores your deployment pipelines (lanes)
  - fastelane/Appfile, which stores your Apple ID and Bundle ID

If you're looking for your Crashlytics Beta API key and build secret, select your org on the [organizations settings page](https://www.fabric.io/settings/organizations), and click on the API key and build secret links right underneath your org’s name.
In this example, the API key and build secret are hardcoded in the Fastfile; refer to this [guide](https://github.com/fastlane/setups/blob/master/Keys.md) for best practices regarding managing your keys.

This lane uses two fastlane [actions](https://github.com/fastlane/fastlane/blob/master/fastlane/docs/Actions.md): `gym` and `crashlytics`. `gym` builds and packages iOS apps for you; it takes care of all the heavy lifting and makes it easy to generate a signed ipa file. The `gym` action invokes the `gym` [tool](https://github.com/fastlane/fastlane/tree/master/gym) behind the scenes. The `crashlytics` actions uses the Crashlytics `submit` [binary](https://docs.fabric.io/ios/beta/build-tools.html) behind the scenes to upload the ipa file.

Open the fastlane/Fastfile in your preferred text editor, change the syntax highlighting to Ruby, and overwrite the contents with the following. Make sure to update the values of `api_token`, `build_secret`, `groups`, and `notes_path` arguments.


```ruby
default_platform :ios

platform :ios do
  lane :beta do
    cert
    sigh
    gym
    crashlytics(
      api_token: "...",
      build_secret: "...",
      groups: "qa-team",
      notes_path: "./release-notes.txt"
      # ipa_path: "..." Path to your IPA file. Optional if you use the `gym` or `xcodebuild` action.
      # crashlytics_path: "..." Path to the submit binary in the Crashlytics bundle (default: './Crashlytics.framework')
      # emails: "..." Pass email addresses of testers, separated by commas
      # notifications: "..." Crashlytics notification option (true/false) (default: 'true')
      # debug: "..." Crashlytics debug option (true/false)
    )
  end
end
```

Run `fastlane ios beta` to execute the lane.

To get the full list of options for each action, run `fastlane action action_name`, eg. `fastlane action crashlytics` or `fastlane action gym`.
