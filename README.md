# Hippo Flutter SDK

[![pub version](https://img.shields.io/pub/v/hippo_flutter_sdk.svg)](https://pub.dev/packages/hippo_flutter_sdk)
[![license](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)

A Flutter plugin for integrating the Hippo customer support platform into your mobile applications. Hippo provides a complete suite of tools for live chat, in-app messaging, and user support.

## Features

- Initialize the Hippo SDK with your application credentials.
- Display the chat conversations view.
- Initiate peer-to-peer chats between users.
- Get real-time updates on unread message counts.
- Clear all user data upon logout.

## Getting Started

This fork patches the iOS side for the latest Hippo SDK (adds `hippoAnnouncementCustomerUnreadCount` and bumps the minimum iOS to 15.1). Consume it via git:

```yaml
dependency_overrides:
  hippo_flutter_sdk:
    git:
      url: https://github.com/ouseqqam/hippo_flutter_sdk_fix.git
      ref: main
```

Then run `flutter pub get`.

## Android Setup

The Hippo SDK requires the `Java-WebSocket` library to function correctly. To avoid potential version conflicts with other plugins, this dependency is not bundled and must be added manually to your main application.

In your app-level `android/app/build.gradle` file, please add the following dependency:

```groovy
dependencies {
    // ... other dependencies
    implementation 'org.java-websocket:Java-WebSocket:1.5.1'
}
```

## iOS Setup (updated)

- Minimum iOS: 15.1
- Hippo is a static framework; use static linkage:

```ruby
platform :ios, '15.1'

target 'Runner' do
  use_frameworks! :linkage => :static
  pod 'Hippo', :git => 'https://github.com/Jungle-Works/Hippo-iOS-SDK', :branch => 'master'
  flutter_install_all_ios_pods File.dirname(File.realpath(__FILE__))
end

post_install do |installer|
  installer.pods_project.targets.each do |target|
    flutter_additional_ios_build_settings(target)
    target.build_configurations.each do |config|
      config.build_settings['IPHONEOS_DEPLOYMENT_TARGET'] = '15.1'
    end
  end
end
```

Then run:

```
cd ios && pod install --repo-update
```

## Usage

First, initialize the SDK with your app key and user details. This is typically done when your app starts.

```dart
import 'package:hippo_flutter_sdk/hippo_flutter_sdk.dart';
import 'dart:convert';

final _hippoFlutterSdkPlugin = HippoFlutterSdk();

void initHippo() {
  final config = {
    "appKey": "YOUR_APP_KEY",
    "userData": {
      "userUniqueKey": "UNIQUE_USER_ID",
      "fullName": "John Doe",
    },
  };
  _hippoFlutterSdkPlugin.initHippo(jsonEncode(config));
}
```

Once initialized, you can open the conversations view:

```dart
_hippoFlutterSdkPlugin.showConversations();
```

## Documentation

For more detailed information on installation, configuration, and advanced usage, please see the full documentation in the [`doc`](./doc) folder.
