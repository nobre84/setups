## Take screenshots of your iOS app

*If you want to confirm that `fastlane` is installed before proceeding, open the Terminal and run `fastlane -v`. You should see the current version of `fastlane` displayed.*

Navigate to your iOS project directory, run `fastlane init`, and follow the prompts. The command will create the following:

- A fastlane directory within your project directory
  - fastlane/Fastfile, which stores your deployment pipelines (lanes)
  - fastelane/Appfile, which stores your Apple ID and Bundle ID

Then, run `snapshot init`, and follow the prompts. The command will create the following:

- A fastlane/Snapfile, which stores `snapshot` config

This lane uses a single fastlane [action](https://github.com/fastlane/fastlane/blob/master/fastlane/docs/Actions.md): `snapshot`. The action invokes the `snapshot` [tool](https://github.com/fastlane/fastlane/tree/master/snapshot) which automates taking of localized screenshots.

`snapshot` relies on the UI Testing framework introduced in Xcode 7. As such, you will need to add a new iOS UI Testing Bundle target to your project. Once that’s done, add the fastlane/SnapshotHelper.swift file to the newly created UI Testing target.

Open FooUITests.swift (the name will depend on what you’ve named the new target) and replace the contents with the following.

```swift
import XCTest

class FooUITests: XCTestCase {
    override func setUp() {
        super.setUp()

        let app = XCUIApplication()
        setupSnapshot(app)
        app.launch()
    }

    func testExample() {
        snapshot("01MainScreen")
    }
}
```

Open the fastlane/Fastfile in your preferred text editor, change the syntax highlighting to Ruby, and overwrite the contents with the following.

```ruby
default_platform :ios

platform :ios do
  lane :screenshots do
    snapshot
  end
end
```

Then, open the fastlane/Snapfile in your preferred text editor, change the syntax highlighting to Ruby, and change the devices and [languages](https://github.com/fastlane/fastlane/tree/master/snapshot#available-language-codes) that you want `snapshot` to use.

Run `fastlane screenshots` to execute the lane. By default, screenshots will be generated in the fastlane/screenshots directory.

To get the full list of options for the `snapshot` action, run `fastlane action snapshot`.
