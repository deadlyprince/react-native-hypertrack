# react-native-hypertrack
React native module for hypertrack-android and hypertrack-ios SDKs. Methods in the Driver SDK are covered in the current release. Our [Example React Native app](https://github.com/hypertrack/react-native-sdk-onboarding) app is built on top of this module.

[![Slack Status](http://slack.hypertrack.io/badge.svg)](http://slack.hypertrack.io) [![npm version](https://badge.fury.io/js/react-native-hypertrack.svg)](https://badge.fury.io/js/react-native-hypertrack)

### To build live location features within your own React Native app
Follow [this step-by-step onboarding guide](https://dashboard.hypertrack.com/onboarding/sdk-reactnative) that will walk you through the sdk integration within your own app in a matter of few minutes.

## Getting started - Android
1. In your project directory, install and link the module package from npm.
    ```bash
    $ npm install react-native-hypertrack --save
    $ react-native link react-native-hypertrack
    ```

2. To use the HyperTrack Android SDKs, the following urls need to be added to your `android/build.gradle` file. This will configure the repository urls for the SDKs.

    ```groovy
    allprojects {
        repositories {
            ...
            maven { url 'http://hypertrack-android-sdk.s3-website-us-west-2.amazonaws.com/' }
        }
    }
    ```
    
3. Import inside Javascript
    ```js
    import RNHyperTrack from 'react-nativee-hypertrack';
    ```

## Getting started - iOS
1. The native iOS SDKs need to be setup using Cocoapods. In your project's `ios` directory, create a Podfile.
    ```bash
    $ cd ios
    $ pod init
    ```

2. Edit the Podfile to include `HyperTrack` as a dependency for your project, and then install the pod with `pod install`.
    ```ruby
    use_frameworks!
    platform :ios, '9.0'

    target 'AwesomeProject' do

      # Pods for AwesomeProject
      pod 'HyperTrack'

      post_install do |installer|
            installer.pods_project.targets.each do |target|
                target.build_configurations.each do |config|
                    config.build_settings['SWIFT_VERSION'] = '3.0'
                end
            end
      end
    end
    ```

3. Open the iOS project with **.xcworkspace** file in Xcode and also, open the `node_modules/react-native-hypertrack/` directory. Move the `_ios/RNHyperTrack.h` and `_ios/RNHyperTrack.m` files to your project as shown below.

![iOS link](link.gif)

4. Import inside Javascript.
    ```js
    import { NativeModules } from 'react-native';
    var RNHyperTrack = NativeModules.RNHyperTrack;
    ```

## API usage

#### 1. Initialize the SDK

```javascript
import RNHyperTrack from 'react-native-hypertrack';
...

export default class MyApp extends Component {
  constructor() {
   super();

   // Initialize HyperTrack wrapper
   RNHyperTrack.initialize("YOUR_PUBLISHABLE_KEY");
  }
}
...
```

#### 2. Requesting Location & Motion (iOS) Authorizations 

```javascript
// Call this method to check location authorization status.
RNHyperTrack.locationAuthorizationStatus((callback) => {
  // Handle locationAuthorizationStatus API callback here
  console.log('locationAuthorizationStatus: ', callback);
});

// Call this method to request Location Authorization for Android & iOS (Always Authorization).
// NOTE: In Android, the Permission dialog box's title and message can be customized by passing them as parameters.
RNHyperTrack.requestLocationAuthorization(title, message);

// Call this method to check location services are enabled or not.
RNHyperTrack.locationServicesEnabled((callback) => {
  // Handle locationServicesEnabled API callback here
  console.log('locationServicesEnabled: ', callback);
});

// Call this method to check if Motion Activity API is available on the device
// NOTE: Motion Authorization is required only for iOS. This API will return an error in Android.
RNHyperTrack.canAskMotionPermissions((callback) => {
  // Handle canAskMotionPermissions API callback here
  console.log('canAskMotionPermissions: ', callback);
});

// Call this method to request Motion Authorization for iOS.
// NOTE: Motion Authorization is required only for iOS. This API will return an error in Android.
RNHyperTrack.requestMotionAuthorization();
```

#### 3. Set or create user
If you have a [user](https://docs.hypertrack.com/v3/api/entities/user.html) that is to be associated with this device, set the user id.
```javascript
RNHyperTrack.setUserId("YOUR_USER_ID");
```

In case you do not have a user, you can create a new user. Calling this API configures the sdk by creating a new user or fetching the existing one, if one exists with the given lookupId.

```javascript
RNHyperTrack.getOrCreateUser(name, phoneNumber, lookupId, (success) => {
      // Handle getOrCreateUser API success here
      console.log("getOrCreateUser success: ", success);
    }, (error) => {
      // Handle getOrCreateUser API error here
      console.log("getOrCreateUser error: ", error);
    });
```

#### 4. Start tracking
To start tracking on the SDK, use the following method.

```javascript
RNHyperTrack.startTracking((success) => {
      // Handle startTracking API success here
      console.log("startTracking success: ", success);
    },
    (error) => {
      // Handle startTracking API error here
      console.log("startTracking error: ", error);
    });
```

#### 5. Create and assign an `Action`
Create and assign an Action object to the user. The createAndAssignAction method accepts a js dictionary object with `expected_place_id`, `type`, `lookup_id` and `expected_at` keys.

```javascript
var params = {
  'expected_place_id': '8166a3c6-5a55-42be-8c04-d73367b0ad9c',
  'expected_at': '2017-07-06T01:00:00.000Z'
}

RNHyperTrack.createAndAssignAction(params,
    (success) => {
        // Handle createAndAssignAction API success here
        console.log('createAndAssignAction: ', success);
    }, (error) => {
        // Handle createAndAssignAction API error here
        console.log('createAndAssignAction: ', error);
    }
);
```

#### 6. Completing an action
If you are using actions for your use-case, you can complete actions through the SDK.

```javascript
RNHyperTrack.completeAction("YOUR_ACTION_ID");
```

#### 7. Stop tracking
To stop tracking on the SDK, use the following method.

```javascript
RNHyperTrack.stopTracking();
```

## Documentation
The HyperTrack documentation is at [docs.hypertrack.com](http://docs.hypertrack.com/).

## Support
For any questions, please reach out to us on [Slack](http://slack.hypertrack.io/) or on help@hypertrack.io. Please create an [issue](https://github.com/hypertrack/react-native-hypertrack/issues) for bugs or feature requests.

## Acknowledgements
Thanks to [react-native-create-library](https://github.com/frostney/react-native-create-library) which saved a few hours.
