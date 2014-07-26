#使用设备管理策略加强安全
##定义声明你的策略

```
<device-admin xmlns:android="http://schemas.android.com/apk/res/android">
    <uses-policies>
        <limit-password />
    </uses-policies>
</device-admin>
```
```
<receiver android:name=".Policy$PolicyAdmin"
    android:permission="android.permission.BIND_DEVICE_ADMIN">
    <meta-data android:name="android.app.device_admin"
        android:resource="@xml/device_admin" />
    <intent-filter>
        <action android:name="android.app.action.DEVICE_ADMIN_ENABLED" />
    </intent-filter>
</receiver>
```
##创建一个设备管理软件

```
<receiver android:name=".Policy$PolicyAdmin"
    android:permission="android.permission.BIND_DEVICE_ADMIN">
    <meta-data android:name="android.app.device_admin"
        android:resource="@xml/device_admin" />
    <intent-filter>
        <action android:name="android.app.action.DEVICE_ADMIN_ENABLED" />
    </intent-filter>
</receiver>
```
##激活设备管理器

```
if (!mPolicy.isAdminActive()) {

    Intent activateDeviceAdminIntent =
        new Intent(DevicePolicyManager.ACTION_ADD_DEVICE_ADMIN);

    activateDeviceAdminIntent.putExtra(
        DevicePolicyManager.EXTRA_DEVICE_ADMIN,
        mPolicy.getPolicyAdmin());

    // It is good practice to include the optional explanation text to
    // explain to user why the application is requesting to be a device
    // administrator. The system will display this message on the activation
    // screen.
    activateDeviceAdminIntent.putExtra(
        DevicePolicyManager.EXTRA_ADD_EXPLANATION,
        getResources().getString(R.string.device_admin_activation_message));

    startActivityForResult(activateDeviceAdminIntent,
        REQ_ACTIVATE_DEVICE_ADMIN);
}
```![image](http://)

##实施设备策略控制

```
DevicePolicyManager mDPM = (DevicePolicyManager)
        context.getSystemService(Context.DEVICE_POLICY_SERVICE);
ComponentName mPolicyAdmin = new ComponentName(context, PolicyAdmin.class);
...
mDPM.setPasswordQuality(mPolicyAdmin, PASSWORD_QUALITY_VALUES[mPasswordQuality]);
mDPM.setPasswordMinimumLength(mPolicyAdmin, mPasswordLength);
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.HONEYCOMB) {
    mDPM.setPasswordMinimumUpperCase(mPolicyAdmin, mPasswordMinUpperCase);
}
```
```
if (!mDPM.isActivePasswordSufficient()) {
    ...
    // Triggers password change screen in Settings.
    Intent intent =
        new Intent(DevicePolicyManager.ACTION_SET_NEW_PASSWORD);
    startActivity(intent);
}
```
```
if (!mDPM.isAdminActive(..)) {
    // Activates device administrator.
    ...
} else if (!mDPM.isActivePasswordSufficient()) {
    // Launches password set-up screen in Settings.
    ...
} else {
    // Grants access to secure content.
    ...
    startActivity(new Intent(context, SecureActivity.class));
}
```

下一堂课:测试


