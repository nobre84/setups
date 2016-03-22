## Release your app to the Play Store

*If you want to confirm that `fastlane` is installed before proceeding, open the Terminal and run `fastlane -v`. You should see the current version of `fastlane` displayed.*

First, you need to setup your Google Developers Service Account:

- Open the [Google Play Console](https://play.google.com/apps/publish/)
- Select **Settings** tab, followed by the **API access** tab
- Click the **Create Service Account** button and follow the **Google Developers Console** link in the dialog
- Click **Add credentials** and select **Service account**
- Select **JSON** as the Key type and click **Create**
- Make a note of the file name of the JSON file downloaded to your computer, and close the dialog
- Make a note of the **Email address** under **Service accounts** - this is the user which you will need later
- Back on the Google Play developer console, click **Done** to close the dialog
- Click on **Grant Access** for the newly added service account
- In the **Invite a New User** dialog, paste the service account email address you noted earlier into the **Email address** field
- Choose **Release Manager** from the **Role** dropdown and click **Send Invitation** to close the dialog

Once done, navigate to your Android project directory, run `fastlane init`, and follow the prompts. The command will create the following:

- A fastlane directory within your project directory
  - fastlane/Fastfile, which stores your deployment pipelines (lanes)
  - fastelane/Appfile, which stores your JSON key file path and package name

This lane uses two fastlane [actions](https://github.com/fastlane/fastlane/blob/master/fastlane/docs/Actions.md): `supply`. `supply` invokes the `supply` [tool](https://github.com/fastlane/fastlane/tree/master/supply) to upload Android apps and their metadata to the Play Store. Please note that the app
must already exist on the Play Store.

Open the fastlane/Fastfile in your preferred text editor, change the syntax highlighting to Ruby, and find the `deploy` lane:

```ruby
desc "Deploy a new version to the Google Play"
lane :deploy do
  gradle(task: "assembleRelease")
  supply
end
```

Run `fastlane android deploy` to execute the lane.

To see the full list of option to deploy your app, run `fastlane action supply`.
