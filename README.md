# fastlane setups

This repository shows you best practice on how to use `fastlane` in your project.

## Installing fastlane

The easiest way to install `fastlane` on a new Mac is to run `sudo gem install fastlane --verbose`. 

It is recommended to use the [sudo-less gem installation](https://guides.cocoapods.org/using/getting-started.html#sudo-less-installation). 

## Standard Setup

- Run tests
- Build and sign your application
- Distribute the build to [Crashlytics](https://crashlytics.com)

```ruby
default_platform :ios

platform :ios do
  desc "Runs all the tests"
  lane :test do
    xctool
  end

  desc "Build and distribute build to Crashlytics"
  lane :beta do
    gym(scheme: "Release")
    crashlytics(crashlytics_path: "./Crashlytics.framework/submit")
  end
end
```

Additionally you'll have to store your keys somewhere, take a look at the [Keys Guide](Keys.md).

## Advanced Setup

- Automatic version number increment
- Basic git actions
- User inputs
- Slack notifications

```ruby
default_platform :ios

platform :ios do
  before_all do
    # optional: prepare your project here, e.g. cocoapods or carthage
  end

  desc "Build and distribute build to Crashlytics"
  lane :beta do
    # Make sure we don't have any uncommited changes on master and are up to date with the remote
    ensure_git_status_clean
    ensure_git_branch(branch: 'master')
    git_pull
    push_to_git_remote

    # Ask the user of fastlane to enter a changelog
    changelog = prompt(text: "Enter the change log: ",
                       multi_line_end_keyword: "END")

    # Increment the version number of your Xcode project
    version = increment_version_number(bump_type: "patch")

    # Build and sign your app
    gym(scheme: "Release")

    # Distribute via Crashlytics
    crashlytics(crashlytics_path: "./Crashlytics.framework/submit",
                notes: changelog)

    # Post Slack notification
    slack(
      channel: "development",
      default_payloads: [], # reduce the notification to the minimum
      message: "Successfully distributed version #{version} :rocket:",
      payload: {
        "Changes" => changelog
      }
    )
  end

  error do |lane, exception|
    slack(message: exception.to_s, success: false)
  end
end
```

Additionally you'll have to store your keys somewhere, take a look at the [Keys Guide](Keys.md).
