# Personal Platform Services

- [Introduction](README.md)
- [Build a server](build_a_server.md)
- [Decommission a server](decommission_a_server.md)
- [App Store Deployment of iOS App from Scratch](app_store_deployment_from_scratch.md)

# App Store Deployment of iOS App from Scratch

To upload an iOS app using industry standards as of 2026, it is important to move beyond manual uploads and implement a structured App Release Lifecycle. This process must integrate automated testing, beta distribution, and Continuous Integration/Continuous Deployment (CI/CD) to ensure stability and professional quality.

## Phase 1: Setup & Compliance (The Foundation)

### Enroll in the Apple Developer Program: 
An active Apple Developer account ($99 USD/year or $149 NZD/year) to submit to the App Store.
### Configure App Privacy Manifests: 
As of 2026, Apple strictly requires a `PrivacyInfo.xcprivacy` file. Audit the app and any third-party SDKs to declare data collection and API usage with approved reason codes.
Let's atleast define the `PrivacyInfo.xcprivacy` so that the app is compliant for dependencies that using authentication and have chances of storing user's data.

#### Step 1: Create the Privacy Manifest File
- In Xcode, go to `File > New > File from Template`
- Search for "App Privacy" in the filter bar.
- Select the App Privacy file type and click Next.
- Ensure your app target is selected in the "Targets" list.
- Save it with the default name: PrivacyInfo.xcprivacy. 
#### Step 2: Audit and Declare "Required Reason" APIs 
- Right-click `PrivacyInfo.xcprivacy` in your project navigator.
- Select `Open As > Source Code`.
- Paste the following:
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://apple.com">
<plist version="1.0">
<dict>
    <key>NSPrivacyAccessedAPITypes</key>
    <array>
        <dict>
            <key>NSPrivacyAccessedAPIType</key>
            <string>NSPrivacyAccessedAPICategoryUserDefaults</string>
            <key>NSPrivacyAccessedAPIReasons</key>
            <array>
                <string>CA92.1</string>
            </array>
        </dict>
        <dict>
            <key>NSPrivacyAccessedAPIType</key>
            <string>NSPrivacyAccessedAPICategoryDiskSpace</string>
            <key>NSPrivacyAccessedAPIReasons</key>
            <array>
                <string>E174.1</string>
            </array>
        </dict>
    </array>
    <key>NSPrivacyCollectedDataTypes</key>
    <array>
        <dict>
            <key>NSPrivacyCollectedDataType</key>
            <string>NSPrivacyDataTypeEmailAddress</string>
            <key>NSPrivacyCollectedDataTypeLinked</key>
            <false/>
            <key>NSPrivacyCollectedDataTypeTracking</key>
            <false/>
            <key>NSPrivacyCollectedDataTypePurposes</key>
            <array>
                <string>NSPrivacyPurposeAppFunctionality</string>
            </array>
        </dict>
        <dict>
            <key>NSPrivacyCollectedDataType</key>
            <string>NSPrivacyDataTypeName</string>
            <key>NSPrivacyCollectedDataTypeLinked</key>
            <false/>
            <key>NSPrivacyCollectedDataTypeTracking</key>
            <false/>
            <key>NSPrivacyCollectedDataTypePurposes</key>
            <array>
                <string>NSPrivacyPurposeAppFunctionality</string>
            </array>
        </dict>
        <dict>
            <key>NSPrivacyCollectedDataType</key>
            <string>NSPrivacyDataTypeUserID</string>
            <key>NSPrivacyCollectedDataTypeLinked</key>
            <false/>
            <key>NSPrivacyCollectedDataTypeTracking</key>
            <false/>
            <key>NSPrivacyCollectedDataTypePurposes</key>
            <array>
                <string>NSPrivacyPurposeAppFunctionality</string>
            </array>
        </dict>
    </array>
</dict>
</plist>

```
- Save the file in Xcode.
- Go to Product > Archive to create a fresh build.
- In the Organizer, right-click the archive and select Generate Privacy Report. This is to make sure that the report does not show any potential errors.


### Create an App Record: 
Log in to App Store Connect and create a new app record. You will need a Bundle ID that exactly matches your Xcode project settings.
This is the process of registering your app's "shell" in Apple's system. This record connects the actual code (the Bundle ID) to the marketing details (Name, Screenshots, etc.). 
As of 2026, industry standards require you to first register your Bundle ID in the Developer Portal before you can select it in App Store Connect. 

#### Step 1: Find and Match Your Bundle ID
Bundle ID is a unique "reverse-domain" string (e.g., com.yourname.mapguessr) that identifies the app across the entire Apple ecosystem. 
- In Xcode, Select the project > Target > General tab.
- Look for Bundle Identifier under the "Identity" section. Copy the Bundle Identifier string.

#### Step 2: Register the ID in the Developer Portal 
Before the App Store can see the ID, it must be registered as an "Identifier".
- Log in to the Apple Developer Portal.
- Navigate to Certificates, Identifiers & Profiles > Identifiers.

If the Xcode is already linked to the Apple ID, it would have automatically been created, else follow the below steps:
- Click the (+) button to add a new identifier and select App IDs.
- Description: Enter a simple name (e.g., "Map Guessr App").
- Bundle ID: Select Explicit and paste the exact string you copied from Xcode.
- Click Continue, then Register. 

#### Step 3: Create the Record in App Store Connect 
Now let's create the "store page" that users will see.
- On App Store Connect homepage after login, click `Apps`, then click the (+) button and select New App.
- Fill in the "New App" dialog:
- Platform: Select iOS.
- Name: This is your public store name (up to 30 characters). It must be unique across the entire App Store.
- Primary Language: The default language for your listing.
- Bundle ID: Choose the identifier you just registered in Step 2 from the dropdown menu.
- SKU: An internal ID for your records (e.g., name_of_application_1234). It is not visible to users.
- User Access: Set to Full Access unless you have a large team and need to restrict who can see this specific app.
- Click Create. 

#### Key Rules for 2026
Immutable Bundle ID: Once the app record is created, you cannot change the Bundle ID.
SKU Restrictions: Your SKU must be unique among your own apps and cannot be edited later.
Unique Name: If your desired name is taken, you will get an error. You may need to add a small keyword to make it unique.

### Prepare Metadata: 
Collect your marketing assets, including App Store screenshots for all required device sizes (iPhone and iPad), a description, keywords, and a mandatory Privacy Policy URL.
