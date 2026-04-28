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
