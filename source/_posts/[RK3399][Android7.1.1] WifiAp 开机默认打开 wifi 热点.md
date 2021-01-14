---
title: RK3399-Android7.1.1-WifiAp 开机默认打开 wifi 热点
date: 2020-12-22 15:06:15
img: '/medias/14.jpg'
categories:
- Android
---

## [RK3399][Android7.1.1] WifiAp 开机默认打开 wifi 热点

``` CPP
diff --git a/frameworks/opt/net/wifi/service/java/com/android/server/wifi/WifiServiceImpl.java b/frameworks/opt/net/wifi/service/java/com/android/server/wifi/WifiServiceImpl.java
index 0f3e901..060563c 100644
--- a/frameworks/opt/net/wifi/service/java/com/android/server/wifi/WifiServiceImpl.java
+++ b/frameworks/opt/net/wifi/service/java/com/android/server/wifi/WifiServiceImpl.java
@@ -408,14 +408,21 @@ public class WifiServiceImpl extends IWifiManager.Stub {
 
    public void checkAndStartWifi() {
         
         .....
         .....
         .....
         
         
         
         registerForBroadcasts();
         registerForPackageOrUserRemoval();
         mInIdleMode = mPowerManager.isDeviceIdleMode();

         mWifiController.start();
        
+        int wifiap_on = Settings.Global.getInt(mContext.getContentResolver(), Settings.Global.WIFIAP_ON, 0);
+        if(wifiap_on == 1) {
+            setWifiApEnabled(getWifiApConfiguration(),true);
+        } else if (wifiEnabled){
+            setWifiEnabled(wifiEnabled); 
+        } 
     }

    public void setWifiApEnabled(WifiConfiguration wifiConfig, boolean enabled) {
        enforceChangePermission();
        ConnectivityManager.enforceTetherChangePermission(mContext);
        if (mUserManager.hasUserRestriction(UserManager.DISALLOW_CONFIG_TETHERING)) {
            throw new SecurityException("DISALLOW_CONFIG_TETHERING is enabled for this user.");
        }
        
        if (wifiConfig == null || isValid(wifiConfig)) {
+	  		Settings.Global.putInt(mContext.getContentResolver(), Settings.Global.WIFIAP_ON, enabled ? 1 : 0);
            mWifiController.obtainMessage(CMD_SET_AP, enabled ? 1 : 0, 0, wifiConfig).sendToTarget();
        } else {
            Slog.e(TAG, "Invalid WifiConfiguration");
        }
    }

diff --git a/frameworks/base/packages/SettingsProvider/src/com/android/providers/settings/DatabaseHelper.java b/frameworks/base/packages/SettingsProvider/src/com/android/providers/settings/DatabaseHelper.java
index 20b95b9..545af52 100644
--- a/frameworks/base/packages/SettingsProvider/src/com/android/providers/settings/DatabaseHelper.java
+++ b/frameworks/base/packages/SettingsProvider/src/com/android/providers/settings/DatabaseHelper.java
@@ -2581,6 +2581,9 @@ class DatabaseHelper extends SQLiteOpenHelper {
		private void loadGlobalSettings(SQLiteDatabase db) {
		     ......
		     ......
             loadBooleanSetting(stmt, Settings.Global.AIRPLANE_MODE_ON,
                     R.bool.def_airplane_mode_on);
 
+            loadBooleanSetting(stmt, Settings.Global.WIFIAP_ON,
+                    R.bool.def_wifiap_on);
+
             loadBooleanSetting(stmt, Settings.Global.THEATER_MODE_ON,
                     R.bool.def_theater_mode_on);

diff --git a/frameworks/base/packages/SettingsProvider/res/values/defaults.xml b/frameworks/base/packages/SettingsProvider/res/values/defaults.xml
index a2e0012..3cc42eb 100644
--- a/frameworks/base/packages/SettingsProvider/res/values/defaults.xml
+++ b/frameworks/base/packages/SettingsProvider/res/values/defaults.xml
@@ -7,4 +7,7 @@
+    <!--if true while boot_completed will start wifiap on  -->
+    <bool >true</bool>



diff --git a/frameworks/base/core/java/android/provider/Settings.java b/frameworks/base/core/java/android/provider/Settings.java
index d8e8e45..82a50f3 100755
--- a/frameworks/base/core/java/android/provider/Settings.java
+++ b/frameworks/base/core/java/android/provider/Settings.java
@@ -4249,7 +4249,7 @@ public final class Settings {
             MOVED_TO_GLOBAL.add(Settings.Global.WIFI_ON);
             MOVED_TO_GLOBAL.add(Settings.Global.WIFI_P2P_DEVICE_NAME);
             MOVED_TO_GLOBAL.add(Settings.Global.WIFI_SAVED_STATE);
+            MOVED_TO_GLOBAL.add(Settings.Global.WIFIAP_ON);
             MOVED_TO_GLOBAL.add(Settings.Global.WIFI_SUPPLICANT_SCAN_INTERVAL_MS);
             MOVED_TO_GLOBAL.add(Settings.Global.WIFI_SUSPEND_OPTIMIZATIONS_ENABLED);
             MOVED_TO_GLOBAL.add(Settings.Global.WIFI_VERBOSE_LOGGING_ENABLED);
@@ -7764,7 +7764,7 @@ public final class Settings {
       
       public static final String BLE_SCAN_ALWAYS_AVAILABLE =
               "ble_scan_always_enabled";

+        
+       public static final String WIFIAP_ON = "wifiap_on";

```

转自：https://www.it610.com/article/1280824794740178944.htm