# 动态权限

**动态权限：这类权限在需要的时候，需要我们动态申请**

**比如：当我们需要打开相机拍摄照片的时候需要我们通过代码的方式在需要的地方去申请权限。**

**具体的权限分组情况如下表：**

group:android.permission-group.CONTACTS

permission:android.permission.WRITE_CONTACTS

permission:android.permission.GET_ACCOUNTS

permission:android.permission.READ_CONTACTS

group:android.permission-group.PHONE

permission:android.permission.READ_CALL_LOG

permission:android.permission.READ_PHONE_STATE

permission:android.permission.CALL_PHONE

permission:android.permission.WRITE_CALL_LOG

permission:android.permission.USE_SIP

permission:android.permission.PROCESS_OUTGOING_CALLS

permission:com.android.voicemail.permission.ADD_VOICEMAIL

group:android.permission-group.CALENDAR

permission:android.permission.READ_CALENDAR

permission:android.permission.WRITE_CALENDAR

group:android.permission-group.CAMERA

permission:android.permission.CAMERA

group:android.permission-group.SENSORS

permission:android.permission.BODY_SENSORS

group:android.permission-group.LOCATION

permission:android.permission.ACCESS_FINE_LOCATION

permission:android.permission.ACCESS_COARSE_LOCATION

group:android.permission-group.STORAGE

permission:android.permission.READ_EXTERNAL_STORAGE

permission:android.permission.WRITE_EXTERNAL_STORAGE

group:android.permission-group.MICROPHONE

permission:android.permission.RECORD_AUDIO

group:android.permission-group.SMS

permission:android.permission.READ_SMS

permission:android.permission.RECEIVE_WAP_PUSH

permission:android.permission.RECEIVE_MMS

permission:android.permission.RECEIVE_SMS

permission:android.permission.SEND_SMS

permission:android.permission.READ_CELL_BROADCASTS

<——————-华丽的分割线 —————————–>

# 普通权限

**普通权限：这些只是普通权限，我们开发的时候，正常使用就行了，需要的权限在清单文件配置即可。**

**普通权限的总结：**

ACCESS_LOCATION_EXTRA_COMMANDS 定位权限

ACCESS_NETWORK_STATE 网络状态权限

ACCESS_NOTIFICATION_POLICY 通知 APP通知显示在状态栏

ACCESS_WIFI_STATE WiFi状态权限

BLUETOOTH 使用蓝牙权限

BLUETOOTH_ADMIN 控制蓝牙开关

BROADCAST_STICKY 粘性广播

CHANGE_NETWORK_STATE 改变网络状态

CHANGE_WIFI_MULTICAST_STATE 改变WiFi多播状态，应该是控制手机热点（猜测）

CHANGE_WIFI_STATE 控制WiFi开关，改变WiFi状态

DISABLE_KEYGUARD 改变键盘为不可用

EXPAND_STATUS_BAR 扩展bar的状态

GET_PACKAGE_SIZE 获取应用安装包大小

INTERNET 网络权限

KILL_BACKGROUND_PROCESSES 杀死后台进程

MODIFY_AUDIO_SETTINGS 改变音频输出设置

NFC 支付

READ_SYNC_SETTINGS 获取手机设置信息

READ_SYNC_STATS 数据统计

RECEIVE_BOOT_COMPLETED 监听启动广播

REORDER_TASKS 创建新栈

REQUEST_INSTALL_PACKAGES 安装应用程序

SET_TIME_ZONE 允许应用程序设置系统时间区域

SET_WALLPAPER 设置壁纸

SET_WALLPAPER_HINTS 设置壁纸上的提示信息，个性化语言

TRANSMIT_IR 红外发射

USE_FINGERPRINT 指纹识别

VIBRATE 震动

WAKE_LOCK 锁屏

WRITE_SYNC_SETTINGS 改变设置

SET_ALARM 设置警告提示

INSTALL_SHORTCUT 创建快捷方式

UNINSTALL_SHORTCUT 删除快捷方式