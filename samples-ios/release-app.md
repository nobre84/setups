## Release your app to the App Store

*If you want to confirm that `fastlane` is installed before proceeding, open the Terminal and run `fastlane -v`. You should see the current version of `fastlane` displayed.*

Navigate to your iOS project directory, run `fastlane init`, and follow the prompts. The command will create the following:

- A fastlane directory within your project directory
  - fastlane/Fastfile, which stores your deployment pipelines (lanes)
  - fastelane/Appfile, which stores your Apple ID and Bundle ID

This lane uses two fastlane [actions](https://github.com/fastlane/fastlane/blob/master/fastlane/docs/Actions.md): `gym` and `deliver`. `gym` builds and packages iOS apps for you; it takes care of all the heavy lifting and makes it easy to generate a signed ipa file. `deliver` uploads screenshots, metadata, and your app to the App Store. Both actions use the corresponding tools behind the scenes: [`gym`](https://github.com/fastlane/fastlane/tree/master/gym) and [`deliver`](https://github.com/fastlane/fastlane/tree/master/deliver).

Open the fastlane/Fastfile in your preferred text editor, change the syntax highlighting to Ruby, and overwrite the contents with the following.


```ruby
default_platform :ios

platform :ios do
  lane :deploy do
    gym
    deliver
  end
end
```

Run `fastlane deploy` to execute the lane.

To get the full list of options for each action, run `fastlane action action_name`, eg. `fastlane action gym` or `fastlane action deliver`.
