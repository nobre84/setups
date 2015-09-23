## fastlane keys

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


