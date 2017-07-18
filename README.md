# MediaBrix Air ANE
## Please see "Testing / Release Settings" section for new guidelines on testing and deploying your integration.

## Getting Started

### Prerequisites
* Ensure that Flash Builder is using Java 6.
* The MediaBrix Air ANE is compatiable with Adobe Air SDK 18.0 and above.

### Add the ANE to Your Project
* Right click your project
* Select "Project Properties"
* Select "ActionScript Build Path" and select the "Native Extensions"
* Press "Add ANE" and locate where you saved "mediabrix.ane"
* Press "OK"

*note: If your project already uses a google play services dependecny use mediabrix_nogps.ane instead

### Android Set-Up
Please the add following information for the Android manifest defined in your projectsname-app.xml file.


Add the following permissions:
```
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

The following permissions are **optional**, but for increased revenue opportunity add the following permissions:
```
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
<uses-permission android:name="android.permission.VIBRATE" />
<uses-permission android:name="android.permission.WRITE_CALENDAR"/>
```

Add the following elements within your project's Application tag:
```
<activity
     android:name="com.mediabrix.android.service.AdViewActivity"
     android:configChanges="orientation|screenSize|keyboard"
     android:hardwareAccelerated="true"
     android:theme="@android:style/Theme.Translucent.NoTitleBar.Fullscreen" >
</activity>
<service
      android:name="com.mediabrix.android.service.MediaBrixService" >
</service> 
```

## Implementing the MediaBrix SDK
Include the following imports in the class that will be implementing the MediaBrix SDK:
```
import com.mediabrix.air.IAdEventsListener;
import com.mediabrix.air.MediabrixAirAPI; 
```

The class in which you would like to display ads will need to implement the `IAdEventsListener` interface. The methods that the class will implement provides the class with information regarding the SDK's ad state.

### Initialization
To initialize the MediaBrix SDK you will need to create an instance of the MediaBrixAPI object in your class' Main() method:
```
public function Main()
{
     super();
     MediabrixAirAPI.instance.initialize(baseURL,appID,this); //this refers to the class that is implementing IAdEventsListener	
}
```

### Testing / Release Settings

To facilitate integrations and QA around the globe, MediaBrix has deployed an open Base URL for all of our world wide network partners to use while testing the MediaBrix SDK. This Test Base URL will eliminate the need for proxying your device to the US and ensure your app receives 100% fill during testing.

* **Test Base URL:** `https://test-mobile.mediabrix.com/v2/manifest/`

* **Production Base URL:** `https://mobile.mediabrix.com/v2/manifest/`

`https://test-mobile.mediabrix.com/v2/manifest/` should **ONLY** be used for testing purposes, as it will not deliver live campaigns to your app.

It is important to ensure that after testing, the Release build of your app uses the Production Base URL. **If you release your app using the Test Base URL, your app will not receive payable MediaBrix ads.**

### Load an Ad
After receiving the `onStarted()` callback, the SDK is now ready to load ads. To load an ad call the method below:
```
MediabrixAirAPI.instance.load(zone); //ZONE is a string that provided to you by MediaBrix
```

### Show an Ad
After receiving the `onAdReady(String zone)` callback, the SDK is ready to show an ad for that zone 
```
MediabrixAirAPI.instance.show(zone);
```

### SDK Callback Methods
The `IAdEventsListener` interface provides several callback methods that provide information regarding the SDK ad state after initialization, loading, and showing.
```
public function onStarted(status:String):void{
    // MediaBrix SDK has been initialized successfully, and can now attempt to load ads.
}

public function onAdReady(zone:String):void{
   // The SDK has successfully downloaded an ad, and is ready to show
}

public function onAdUnavailable(zone:String):void{
   // The SDK was not able to download an ad
}

public function onAdShown(zone:String)void{
   // The user is currently viewing the ad
}

public function onAdRewardConfirmation(zone:String):void{
   // The user has watched an ad that offers an incentive. 
   // The user should be granted some award. 
}

public function onAdClicked(zone:String):void{
    // The ad was clicked by the user
}

public function onAdClosed(zone:String):void{
    // The ad that was being displayed to the user has been closed
}
```

### Verbose Logging
The MediaBrix SDK prints out logs to reflect what state it is in. To turn off logs printed out by the SDK use the following command:
```
MediabrixAirAPI.instance.setDebug(false);//By setting setDebug to false, the SDK will not output any logs. 
```
