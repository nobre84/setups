## fastlane keys guide

This guide will show you the best ways to store and use your keys for services like Crashlytics, Slack, etc.

### In your `Fastfile`

```ruby
lane :beta do
  crashlytics(api_token: "123abc", build_secret: "secret_key")
end
```

or if you want to only define the keys once and use it multiple times


```ruby
ENV["CRASHLYTICS_API_TOKEN"] = "123abc"
ENV["CRASHLYTICS_BUILD_SECRET"] = "secret_key"

lane :beta do
  crashlytics
end
```

This is the most straight forward and easiest solution, but might cause a few issues:

- If you ever decide to open source your application you'll have to make sure to remove the keys from your git history
- If your keys got invalidated for whatever reason and you decide to rollback your code base to an earlier stage the keys will also be rolled back.

### Bash Profile

To not store your keys in `git`, you can pass all parameters of all actions using environment variables.

You can edit your `~/.bash_profile` to include something like

```sh
export SLACK_URL="https://hooks.slack.com/services/T03NA19Q5/..."
export CRASHLYTICS_API_TOKEN="123abc"
```

If you use a different shell (e.g. `zshell`) you'll need to edit `~/.zshrc` instead.

**Disadvantages**

- Every terminal tool you run gets access to your environment variables. 
- You have to edit your bash profile on every computer you want to run `fastlane` from
- The bash profile isn't automatically loaded by some CI-systems like Jenkins


### [dotenv](https://github.com/bkeepers/dotenv)

You can store a default configuration in `.env.default` which will be loaded by `fastlane` automatically.

```sh
SLACK_URL="https://hooks.slack.com/services/T03NA19Q5/..."
CRASHLYTICS_API_TOKEN="123abc"
```

You might want different configurations depending on your environment.

```
fastlane beta --env development
```

and store the configuration in `.env.development` with all keys for the development environment.

Install `sudo gem install dotenv` or add `dotenv` to your `Gemfile`. More information about the [recommend way to install gems](https://guides.cocoapods.org/using/a-gemfile.html).
