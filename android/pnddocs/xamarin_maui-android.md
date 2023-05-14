## MAUI

### Step 1. Install Pendo SDK

1. In **Visual Studio** Solution Explorer, right-click on your project, then select "Add" - > "Add NuGet Packages…".
2. Search for: **PendoMAUIPlugin** with latest version.<br/>
3. Press **Add Package**.

-------------

### Step 2. Pendo SDK Integration

Note: PendoSDKXamarin plugin requires TargetFrameworkVersion v12.0.

1. #### Open the shared application **App.xaml.cs**

    Add the following under 'using':

    ```c#
    ...
    using PendoMAUIPlugin;
    ...   
    ``` 

    In the **protected override void OnStart()** method, add the following code:

    ```c#
    protected override void OnStart()
    {
       PendoInterface Pendo = new PendoInterface();
       string appKey = "YOUR_APPKEY_HERE";
       Pendo.Setup(appKey);
       ...
    ```

2. #### Start the visitor's session in the page where your visitor is being identified (e.g. login, register, etc.).

    ```c#
    ...
    using PendoMAUIPlugin;
    ...

    namespace ExampleApp
    {
        class ExampleLoginClass
        {

        public void MethodExample()
        {
            ....
            PendoInterface Pendo = new PendoInterface();
            
            var visitorId = "VISITOR-UNIQUE-ID";
            var accountId = "ACCOUNT-UNIQUE-ID";

            var visitorData = new Dictionary<string, object>
            {
                { "age", 27 },
                { "country", "USA" }
            };

            var accountData = new Dictionary<string, object>
            {
                { "Tier", 1 },
                { "Size", "Enterprise" }
            };

            Pendo.StartSession(visitorId, accountId, visitorData, accountData);
            ...
        }
        ...
    ```

   **visitorId**: a user identifier (e.g. John Smith)  
   **visitorData**: the user metadata (e.g. email, phone, country, etc.)  
   **accountId**: an affiliation of the user to a specific company or group (e.g. Acme inc.)  
   **accountData** : the account metadata (e.g. tier, level, ARR, etc.)

   This code ends the previous mobile session (if applicable), starts a new mobile session and retrieves all guides based on the provided information.

   **Tip:** Passing `null` or `""` as the visitorId generates <a href="https://help.pendo.io/resources/support-library/analytics/anonymous-visitors.html" target="_blank">anonymous visitor id</a>.

-------------

### Step 3. Mobile device connectivity for tagging and testing

These steps allow page <a href="https://support.pendo.io/hc/en-us/articles/360033609651-Tagging-Mobile-Pages#HowtoTagaPage" target="_blank">tagging.</a>
and <a href="https://support.pendo.io/hc/en-us/articles/360033487792-Creating-a-Mobile-Guide#test-guide-on-device-0-6" target="_blank">guide</a> testing capabilities.

1. #### Add the following **activity** to the application **AndroidManifest.xml** in the **<Application>** tag:

    ```xml
    <activity android:name="sdk.pendo.io.activities.PendoGateActivity" android:launchMode="singleInstance" android:exported="true">
     <intent-filter>
       <action android:name="android.intent.action.VIEW"/>
       <category android:name="android.intent.category.DEFAULT"/>
       <category android:name="android.intent.category.BROWSABLE"/>
       <data android:scheme="YOUR_SCHEME_HERE"/>
     </intent-filter>
    </activity>
    ```
-------------

### Step 4. Verify Installation
1. Using Visual Studio: Run the app and search in the device log for:  
   `Pendo SDK was successfully integrated and connected to the server.`
2. Click to go through a <a href="#" data-start-verification>verification process</a> for the SDK integration.
3. Confirm that you can see your app under <a href="https://app.pendo.io/admin" target="_blank">subscription settings</a> as Integrated.

-------------

### Developer Documentation

* API documentation available <a href="https://support.pendo.io/hc/en-us/articles/360057203531-Android-Developer-API-Documentation" target="_blank">here.</a>

-------------

### Troubleshooting

* Review the <a href="https://developers.pendo.io/category/mobile-sdk/" target="_blank">release notes</a> for any backward compatibility issues.
* Review Android minimum requirements (compileSdkVersion, minSdkVersion, etc.) <a href="https://support.pendo.io/hc/en-us/articles/4404065352987-Developer-s-Guide-to-Installing-the-Pendo-Android-SDK#requirements-0-0" target="_blank">here.</a>
* If you are encountering **Dex** problems, please refer to:
  * <a href="https://developer.android.com/studio/build/multidex" target="_blank">https://developer.android.com/studio/build/multidex </a>
  * <a href="https://docs.microsoft.com/en-us/xamarin/android/deploy-test/release-prep/?tabs=macos#multi-dex" target="_blank">https://docs.microsoft.com/en-us/xamarin/android/deploy-test/release-prep/?tabs=macos#multi-dex </a>