# MauiBlazorSample
dotnet sdkでmaui-blazorのWindowsアプリを作成するサンプル

## MAUIアプリを作成できるようにする
```
$ dotnet workload install maui
```

## maui-blazorのプロジェクトを作成
```
dotnet new maui-blazor -o .\MauiBlazorSample
```

## ビルドのための修正
```
diff --git a/MauiBlazorSample.csproj b/MauiBlazorSample.csproj
index 3785659..9fe59c8 100644
--- a/MauiBlazorSample.csproj
+++ b/MauiBlazorSample.csproj
@@ -1,8 +1,7 @@
 ﻿<Project Sdk="Microsoft.NET.Sdk.Razor">

     <PropertyGroup>
-        <TargetFrameworks>net8.0-android;net8.0-ios;net8.0-maccatalyst</TargetFrameworks>
-        <TargetFrameworks Condition="$([MSBuild]::IsOSPlatform('windows'))">$(TargetFrameworks);net8.0-windows10.0.19041.0</TargetFrameworks>
+        <TargetFrameworks>net8.0-windows10.0.19041.0</TargetFrameworks>^M
         <!-- Uncomment to also build the tizen app. You will need to install tizen by following this: https://github.com/Samsung/Tizen.NET -->
         <!-- <TargetFrameworks>$(TargetFrameworks);net8.0-tizen</TargetFrameworks> -->

@@ -20,6 +19,9 @@
         <ImplicitUsings>enable</ImplicitUsings>
         <EnableDefaultCssItems>false</EnableDefaultCssItems>
         <Nullable>enable</Nullable>
+        <WindowsPackageType>None</WindowsPackageType>^M
+        <PublishSingleFile>true</PublishSingleFile>^M
+        <SelfContained>true</SelfContained>^M

         <!-- Display name -->
         <ApplicationTitle>MauiBlazorSample</ApplicationTitle>
@@ -31,12 +33,8 @@
         <ApplicationDisplayVersion>1.0</ApplicationDisplayVersion>
         <ApplicationVersion>1</ApplicationVersion>

-        <SupportedOSPlatformVersion Condition="$([MSBuild]::GetTargetPlatformIdentifier('$(TargetFramework)')) == 'ios'">14.2</SupportedOSPlatformVersion>
-        <SupportedOSPlatformVersion Condition="$([MSBuild]::GetTargetPlatformIdentifier('$(TargetFramework)')) == 'maccatalyst'">14.0</SupportedOSPlatformVersion>
-        <SupportedOSPlatformVersion Condition="$([MSBuild]::GetTargetPlatformIdentifier('$(TargetFramework)')) == 'android'">24.0</SupportedOSPlatformVersion>
         <SupportedOSPlatformVersion Condition="$([MSBuild]::GetTargetPlatformIdentifier('$(TargetFramework)')) == 'windows'">10.0.17763.0</SupportedOSPlatformVersion>
         <TargetPlatformMinVersion Condition="$([MSBuild]::GetTargetPlatformIdentifier('$(TargetFramework)')) == 'windows'">10.0.17763.0</TargetPlatformMinVersion>
-        <SupportedOSPlatformVersion Condition="$([MSBuild]::GetTargetPlatformIdentifier('$(TargetFramework)')) == 'tizen'">6.5</SupportedOSPlatformVersion>
     </PropertyGroup>

     <ItemGroup>
diff --git a/Platforms/Android/AndroidManifest.xml b/Platforms/Android/AndroidManifest.xml
deleted file mode 100644
index c82c665..0000000
--- a/Platforms/Android/AndroidManifest.xml
+++ /dev/null
@@ -1,6 +0,0 @@
-﻿<?xml version="1.0" encoding="utf-8"?>
-<manifest xmlns:android="http://schemas.android.com/apk/res/android">
-    <application android:allowBackup="true" android:icon="@mipmap/appicon" android:roundIcon="@mipmap/appicon_round" android:supportsRtl="true"></application>
-    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
-    <uses-permission android:name="android.permission.INTERNET" />
-</manifest>
\ No newline at end of file
diff --git a/Platforms/Android/MainActivity.cs b/Platforms/Android/MainActivity.cs
deleted file mode 100644
index b1d1863..0000000
--- a/Platforms/Android/MainActivity.cs
+++ /dev/null
@@ -1,10 +0,0 @@
-﻿using Android.App;
-using Android.Content.PM;
-using Android.OS;
-
-namespace MauiBlazorSample;
-
-[Activity(Theme = "@style/Maui.SplashTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation | ConfigChanges.UiMode | ConfigChanges.ScreenLayout | ConfigChanges.SmallestScreenSize | ConfigChanges.Density)]
-public class MainActivity : MauiAppCompatActivity
-{
-}
diff --git a/Platforms/Android/MainApplication.cs b/Platforms/Android/MainApplication.cs
deleted file mode 100644
index 3b2e323..0000000
--- a/Platforms/Android/MainApplication.cs
+++ /dev/null
@@ -1,15 +0,0 @@
-﻿using Android.App;
-using Android.Runtime;
-
-namespace MauiBlazorSample;
-
-[Application]
-public class MainApplication : MauiApplication
-{
-       public MainApplication(IntPtr handle, JniHandleOwnership ownership)
-               : base(handle, ownership)
-       {
-       }
-
-       protected override MauiApp CreateMauiApp() => MauiProgram.CreateMauiApp();
-}
diff --git a/Platforms/Android/Resources/values/colors.xml b/Platforms/Android/Resources/values/colors.xml
deleted file mode 100644
index 5cd1604..0000000
--- a/Platforms/Android/Resources/values/colors.xml
+++ /dev/null
@@ -1,6 +0,0 @@
-<?xml version="1.0" encoding="utf-8"?>
-<resources>
-    <color name="colorPrimary">#512BD4</color>
-    <color name="colorPrimaryDark">#2B0B98</color>
-    <color name="colorAccent">#2B0B98</color>
-</resources>
\ No newline at end of file
diff --git a/Platforms/MacCatalyst/AppDelegate.cs b/Platforms/MacCatalyst/AppDelegate.cs
deleted file mode 100644
index a6c8a49..0000000
--- a/Platforms/MacCatalyst/AppDelegate.cs
+++ /dev/null
@@ -1,9 +0,0 @@
-﻿using Foundation;
-
-namespace MauiBlazorSample;
-
-[Register("AppDelegate")]
-public class AppDelegate : MauiUIApplicationDelegate
-{
-       protected override MauiApp CreateMauiApp() => MauiProgram.CreateMauiApp();
-}
diff --git a/Platforms/MacCatalyst/Entitlements.plist b/Platforms/MacCatalyst/Entitlements.plist
deleted file mode 100644
index 8e87c0c..0000000
--- a/Platforms/MacCatalyst/Entitlements.plist
+++ /dev/null
@@ -1,14 +0,0 @@
-﻿<?xml version="1.0" encoding="UTF-8"?>
-<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
-<plist version="1.0">
-    <!-- See https://aka.ms/maui-publish-app-store#add-entitlements for more information about adding entitlements.-->
-    <dict>
-        <!-- App Sandbox must be enabled to distribute a MacCatalyst app through the Mac App Store. -->
-        <key>com.apple.security.app-sandbox</key>
-        <true/>
-        <!-- When App Sandbox is enabled, this value is required to open outgoing network connections. -->
-        <key>com.apple.security.network.client</key>
-        <true/>
-    </dict>
-</plist>
-
diff --git a/Platforms/MacCatalyst/Info.plist b/Platforms/MacCatalyst/Info.plist
deleted file mode 100644
index 937415d..0000000
--- a/Platforms/MacCatalyst/Info.plist
+++ /dev/null
@@ -1,38 +0,0 @@
-<?xml version="1.0" encoding="UTF-8"?>
-<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
-<plist version="1.0">
-<dict>
-    <!-- The Mac App Store requires you specify if the app uses encryption. -->
-    <!-- Please consult https://developer.apple.com/documentation/bundleresources/information_property_list/itsappusesnonexemptencryption -->
-    <!-- <key>ITSAppUsesNonExemptEncryption</key> -->
-    <!-- Please indicate <true/> or <false/> here. -->
-
-    <!-- Specify the category for your app here. -->
-    <!-- Please consult https://developer.apple.com/documentation/bundleresources/information_property_list/lsapplicationcategorytype -->
-    <!-- <key>LSApplicationCategoryType</key> -->
-    <!-- <string>public.app-category.YOUR-CATEGORY-HERE</string> -->
-    <key>UIDeviceFamily</key>
-    <array>
-        <integer>2</integer>
-    </array>
-    <key>UIRequiredDeviceCapabilities</key>
-    <array>
-        <string>arm64</string>
-    </array>
-    <key>UISupportedInterfaceOrientations</key>
-    <array>
-        <string>UIInterfaceOrientationPortrait</string>
-        <string>UIInterfaceOrientationLandscapeLeft</string>
-        <string>UIInterfaceOrientationLandscapeRight</string>
-    </array>
-    <key>UISupportedInterfaceOrientations~ipad</key>
-    <array>
-        <string>UIInterfaceOrientationPortrait</string>
-        <string>UIInterfaceOrientationPortraitUpsideDown</string>
-        <string>UIInterfaceOrientationLandscapeLeft</string>
-        <string>UIInterfaceOrientationLandscapeRight</string>
-    </array>
-    <key>XSAppIconAssets</key>
-    <string>Assets.xcassets/appicon.appiconset</string>
-</dict>
-</plist>
diff --git a/Platforms/MacCatalyst/Program.cs b/Platforms/MacCatalyst/Program.cs
deleted file mode 100644
index 2d15419..0000000
--- a/Platforms/MacCatalyst/Program.cs
+++ /dev/null
@@ -1,15 +0,0 @@
-﻿using ObjCRuntime;
-using UIKit;
-
-namespace MauiBlazorSample;
-
-public class Program
-{
-       // This is the main entry point of the application.
-       static void Main(string[] args)
-       {
-               // if you want to use a different Application Delegate class from "AppDelegate"
-               // you can specify it here.
-               UIApplication.Main(args, null, typeof(AppDelegate));
-       }
-}
\ No newline at end of file
diff --git a/Platforms/Tizen/Main.cs b/Platforms/Tizen/Main.cs
deleted file mode 100644
index bb80aff..0000000
--- a/Platforms/Tizen/Main.cs
+++ /dev/null
@@ -1,16 +0,0 @@
-using System;
-using Microsoft.Maui;
-using Microsoft.Maui.Hosting;
-
-namespace MauiBlazorSample;
-
-class Program : MauiApplication
-{
-       protected override MauiApp CreateMauiApp() => MauiProgram.CreateMauiApp();
-
-       static void Main(string[] args)
-       {
-               var app = new Program();
-               app.Run(args);
-       }
-}
diff --git a/Platforms/Tizen/tizen-manifest.xml b/Platforms/Tizen/tizen-manifest.xml
deleted file mode 100644
index 6239c29..0000000
--- a/Platforms/Tizen/tizen-manifest.xml
+++ /dev/null
@@ -1,15 +0,0 @@
-﻿<?xml version="1.0" encoding="utf-8"?>
-<manifest package="maui-application-id-placeholder" version="0.0.0" api-version="7" xmlns="http://tizen.org/ns/packages">
-  <profile name="common" />
-  <ui-application appid="maui-application-id-placeholder" exec="MauiBlazorSample.dll" multiple="false" nodisplay="false" taskmanage="true" type="dotnet" launch_mode="single">
-    <label>maui-application-title-placeholder</label>
-    <icon>maui-appicon-placeholder</icon>
-    <metadata key="http://tizen.org/metadata/prefer_dotnet_aot" value="true" />
-  </ui-application>
-  <shortcut-list />
-  <privileges>
-    <privilege>http://tizen.org/privilege/internet</privilege>
-  </privileges>
-  <dependencies />
-  <provides-appdefined-privileges />
-</manifest>
\ No newline at end of file
diff --git a/Platforms/iOS/AppDelegate.cs b/Platforms/iOS/AppDelegate.cs
deleted file mode 100644
index a6c8a49..0000000
--- a/Platforms/iOS/AppDelegate.cs
+++ /dev/null
@@ -1,9 +0,0 @@
-﻿using Foundation;
-
-namespace MauiBlazorSample;
-
-[Register("AppDelegate")]
-public class AppDelegate : MauiUIApplicationDelegate
-{
-       protected override MauiApp CreateMauiApp() => MauiProgram.CreateMauiApp();
-}
diff --git a/Platforms/iOS/Info.plist b/Platforms/iOS/Info.plist
deleted file mode 100644
index 8ae2c74..0000000
--- a/Platforms/iOS/Info.plist
+++ /dev/null
@@ -1,32 +0,0 @@
-<?xml version="1.0" encoding="UTF-8"?>
-<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
-<plist version="1.0">
-<dict>
-    <key>LSRequiresIPhoneOS</key>
-    <true/>
-    <key>UIDeviceFamily</key>
-    <array>
-        <integer>1</integer>
-        <integer>2</integer>
-    </array>
-    <key>UIRequiredDeviceCapabilities</key>
-    <array>
-        <string>arm64</string>
-    </array>
-    <key>UISupportedInterfaceOrientations</key>
-    <array>
-        <string>UIInterfaceOrientationPortrait</string>
-        <string>UIInterfaceOrientationLandscapeLeft</string>
-        <string>UIInterfaceOrientationLandscapeRight</string>
-    </array>
-    <key>UISupportedInterfaceOrientations~ipad</key>
-    <array>
-        <string>UIInterfaceOrientationPortrait</string>
-        <string>UIInterfaceOrientationPortraitUpsideDown</string>
-        <string>UIInterfaceOrientationLandscapeLeft</string>
-        <string>UIInterfaceOrientationLandscapeRight</string>
-    </array>
-    <key>XSAppIconAssets</key>
-    <string>Assets.xcassets/appicon.appiconset</string>
-</dict>
-</plist>
diff --git a/Platforms/iOS/Program.cs b/Platforms/iOS/Program.cs
deleted file mode 100644
index f8121f8..0000000
--- a/Platforms/iOS/Program.cs
+++ /dev/null
@@ -1,15 +0,0 @@
-﻿using ObjCRuntime;
-using UIKit;
-
-namespace MauiBlazorSample;
-
-public class Program
-{
-       // This is the main entry point of the application.
-       static void Main(string[] args)
-       {
-               // if you want to use a different Application Delegate class from "AppDelegate"
-               // you can specify it here.
-               UIApplication.Main(args, null, typeof(AppDelegate));
-       }
-}
diff --git a/Properties/launchSettings.json b/Properties/launchSettings.json
index c16206a..f4c6c8d 100644
--- a/Properties/launchSettings.json
+++ b/Properties/launchSettings.json
@@ -1,7 +1,7 @@
 {
   "profiles": {
     "Windows Machine": {
-      "commandName": "MsixPackage",
+      "commandName": "Project",^M
       "nativeDebugging": false
     }
   }
```
## VSCode
ビルドできるけど、デバッグができない
現在、調査中

## ビルド
```
dotnet build -f net8.0-windows10.0.19041.0
```

## 実行
```
dotnet run -f net8.0-windows10.0.19041.0
```

## リリース
```
dotnet publish -f net8.0-windows10.0.19041.0
```
