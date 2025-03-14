---
title: 【Flutter】Android/iOS 常用权限
swiper_index: 1
top_group_index: 1
background: '#fff'
date: 2024-10-30 17:09:54
updated:
tags: [Flutter,Dart,Hybrid,Android,iOS]
categories:
keywords:
description:
top_img: https://www.developers-zone.com/wp-content/uploads/2022/08/bottom_navigation_bar-exmp-1-1.gif?t=1ab6989f-81d8-8049-b9fa-d7185f4365f4
comments:
cover: https://www.developers-zone.com/wp-content/uploads/2022/08/bottom_navigation_bar-exmp-1-1.gif?t=1ab6989f-81d8-8049-b9fa-d7185f4365f4
toc:
toc_number:
toc_style_simple:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
mathjax:
katex:
aplayer:
highlight_shrink:
aside:
---
##  iOS 常用权限
iOS应用开发中常见的权限列表及其描述：

| 权限 (Info.plist键名)                        | 名称                   | 描述                                   |
| -------------------------------------------- | ---------------------- | -------------------------------------- |
| NSCameraUsageDescription                     | 相机权限               | 访问设备的相机，用于拍摄照片或视频。   |
| NSPhotoLibraryUsageDescription               | 照片权限               | 访问设备相册及保存图片和视频。         |
| NSPhotoLibraryAddUsageDescription            | 添加照片到相册         | 保存照片或视频到相册                   |
| NSMicrophoneUsageDescription                 | 麦克风权限             | 访问麦克风录制音频                     |
| NSLocationWhenInUseUsageDescription          | 使用期间位置权限       | 允许应用在前台运行时获取用户位置       |
| NSLocationAlwaysUsageDescription             | 始终位置权限           | 应用可以始终获取用户位置，包括后台     |
| NSLocationAlwaysAndWhenInUseUsageDescription | 始终与使用期间位置权限 | 允许应用始终（前台和后台）访问用户位置 |
| NSPhotoLibraryUsageDescription               | 照片库权限             | 访问用户的照片库                       |
| NSPhotoLibraryAddUsageDescription            | 照片库添加权限         | 仅允许将内容添加到照片库               |
| NSContactsUsageDescription                   | 通讯录权限             | 允许访问用户通讯录                     |
| NSCalendarsUsageDescription                  | 日历权限               | 允许访问用户的日历事件                 |
| NSRemindersUsageDescription                  | 提醒事项权限           | 访问提醒事项列表                       |
| NSSpeechRecognitionUsageDescription          | 语音识别权限           | 使用语音识别服务                       |
| NSBluetoothPeripheralUsageDescription        | 蓝牙权限               | 访问蓝牙设备以进行数据传输             |
| NSBluetoothAlwaysUsageDescription            | 始终允许蓝牙权限       | 允许应用始终访问蓝牙                   |
| NSMotionUsageDescription                     | 运动与健身数据权限     | 访问设备的运动传感器数据               |
| NSFaceIDUsageDescription                     | 面容识别权限           | 使用Face ID进行用户验证                |
| NSHealthShareUsageDescription                | 健康数据读取权限       | 允许访问健康数据（HealthKit）          |
| NSHealthUpdateUsageDescription               | 健康数据更新权限       | 允许更新用户健康数据（HealthKit）      |
| NSSpeechRecognitionUsageDescription          | 语音识别权限           | 使用语音识别功能                       |
| NSCalendarsUsageDescription                  | 日历权限               | 访问用户日历事件                       |
| NSRemindersUsageDescription                  | 提醒权限               | 允许访问提醒事项                       |
| NSFaceIDUsageDescription                     | 面容识别权限           | 使用FaceID进行身份验证                 |
| NFCReaderUsageDescription                    | NFC权限                | 访问NFC设备进行数据交互                |
| NSAppleMusicUsageDescription                 | Apple Music权限        | 访问用户Apple Music媒体库              |
| NSHomeKitUsageDescription                    | 家庭权限               | 访问HomeKit设备                        |
| NSUserTrackingUsageDescription               | 用户追踪权限           | 请求用户允许应用跟踪（广告追踪）       |
| NSRemindersUsageDescription                  | 提醒事项权限           | 允许应用访问用户的提醒事项             |

这些权限必须在应用的`Info.plist`文件中声明，并提供用户可见的说明文字，以便用户在系统提示时清楚了解权限用途。

## 🤖 Android 常用权限
Android应用开发中常见的权限列表及其描述：


| 权限 (Android Manifest)                        | 名称                   | 描述                                   |
|------|------|------|
|android.permission.ACCESS_CHECKIN_PROPERTIES	|访问登记属性	|读取或写入登记check-in数据库属性表的权限|
| android.permission.ACCESS_COARSE_LOCATION | 获取错略位置 | 通过WiFi或移动基站的方式获取用户错略的经纬度信息,定位精度大概误差在30~1500米 |
| android.permission.ACCESS_FINE_LOCATION | 获取精确位置 | 通过GPS芯片接收卫星的定位信息,定位精度达10米以内 |
| android.permission.ACCESS_LOCATION_EXTRA_COMMANDS | 访问定位额外命令 | 允许程序访问额外的定位提供者指令 |
| android.permission.ACCESS_MOCK_LOCATION | 获取模拟定位信息 | 获取模拟定位信息,一般用于帮助开发者调试应用 |
| android.permission.ACCESS_NETWORK_STATE | 获取网络状态 | 获取网络信息状态,如当前的网络连接是否有效 |
| android.permission.ACCESS_SURFACE_FLINGER | 访问Surface Flinger | Android平台上底层的图形显示支持,一般用于游戏或照相机预览界面和底层模式的屏幕截图 |
| android.permission.ACCESS_WIFI_STATE | 获取WiFi状态 | 获取当前WiFi接入的状态以及WLAN热点的信息 |
| android.permission.ACCOUNT_MANAGER | 账户管理 | 获取账户验证信息,主要为GMail账户信息,只有系统级进程才能访问的权限 |
| android.permission.AUTHENTICATE_ACCOUNTS | 验证账户 | 允许一个程序通过账户验证方式访问账户管理ACCOUNT_MANAGER相关信息 |
| android.permission.BATTERY_STATS | 电量统计 | 获取电池电量统计信息 |
| android.permission.BIND_APPWIDGET | 绑定小插件 | 允许一个程序告诉appWidget服务需要访问小插件的数据库,只有非常少的应用才用到此权限 |
| android.permission.BIND_DEVICE_ADMIN | 绑定设备管理 | 请求系统管理员接收者receiver,只有系统才能使用 |
| android.permission.BIND_INPUT_METHOD | 绑定输入法 | 请求InputMethodService服务,只有系统才能使用 |
| android.permission.BIND_REMOTEVIEWS | 绑定RemoteView | 必须通过RemoteViewsService服务来请求,只有系统才能用 |
| android.permission.BIND_WALLPAPER | 绑定壁纸 | 必须通过WallpaperService服务来请求,只有系统才能用 |
| android.permission.BLUETOOTH | 使用蓝牙 | 允许程序连接配对过的蓝牙设备 |
| android.permission.BLUETOOTH_ADMIN | 蓝牙管理 | 允许程序进行发现和配对新的蓝牙设备 |
| android.permission.BRICK | 变成砖头 | 能够禁用手机,非常危险,顾名思义就是让手机变成砖头 |
| android.permission.BROADCAST_PACKAGE_REMOVED | 应用删除时广播 | 当一个应用在删除时触发一个广播 |
| android.permission.BROADCAST_SMS | 收到短信时广播 | 当收到短信时触发一个广播 |
| android.permission.BROADCAST_STICKY | 连续广播 | 允许一个程序收到广播后快速收到下一个广播 |
| android.permission.BROADCAST_WAP_PUSH | WAP PUSH广播 | WAP PUSH服务收到后触发一个广播 |
| android.permission.CALL_PHONE | 拨打电话 | 允许程序从非系统拨号器里输入电话号码 |
| android.permission.CALL_PRIVILEGED | 通话权限 | 允许程序拨打电话,替换系统的拨号器界面 |
| android.permission.CAMERA | 拍照权限 | 允许访问摄像头进行拍照 |
| android.permission.CHANGE_COMPONENT_ENABLED_STATE | 改变组件状态 | 改变组件是否启用状态 |
| android.permission.CHANGE_CONFIGURATION | 改变配置 | 允许当前应用改变配置,如定位 |
| android.permission.CHANGE_NETWORK_STATE | 改变网络状态 | 改变网络状态如是否能联网 |
| android.permission.CHANGE_WIFI_MULTICAST_STATE | 改变WiFi多播状态 | 改变WiFi多播状态 |
| android.permission.CHANGE_WIFI_STATE | 改变WiFi状态 | 改变WiFi状态 |
| android.permission.CLEAR_APP_CACHE | 清除应用缓存 | 清除应用缓存 |
| android.permission.CLEAR_APP_USER_DATA | 清除用户数据 | 清除应用的用户数据 |
| android.permission.CWJ_GROUP | 底层访问权限 | 允许CWJ账户组访问底层信息 |
| android.permission.CELL_PHONE_MASTER_EX | 手机优化大师扩展权限 | 手机优化大师扩展权限 |
| android.permission.CONTROL_LOCATION_UPDATES | 控制定位更新 | 允许获得移动网络定位信息改变 |
| android.permission.DELETE_CACHE_FILES | 删除缓存文件 | 允许应用删除缓存文件 |
| android.permission.DELETE_PACKAGES | 删除应用 | 允许程序删除应用 |
| android.permission.DEVICE_POWER | 电源管理 | 允许访问底层电源管理 |
| android.permission.DIAGNOSTIC | 应用诊断 | 允许程序到RW到诊断资源 |
| android.permission.DISABLE_KEYGUARD | 禁用键盘锁 | 允许程序禁用键盘锁 |
| android.permission.DUMP | 转存系统信息 | 允许程序获取系统dump信息从系统服务 |
| android.permission.EXPAND_STATUS_BAR | 状态栏控制 | 允许程序扩展或收缩状态栏 |
| android.permission.FACTORY_TEST | 工厂测试模式 | 允许程序运行工厂测试模式 |
| android.permission.FLASHLIGHT | 使用闪光灯 | 允许访问闪光灯 |
| android.permission.FORCE_BACK | 强制后退 | 允许程序强制使用back后退按键,无论Activity是否在顶层 |
| android.permission.GET_ACCOUNTS | 访问账户Gmail列表 | 访问GMail账户列表 |
| android.permission.GET_PACKAGE_SIZE | 获取应用大小 | 获取应用的文件大小 |
| android.permission.GET_TASKS | 获取任务信息 | 允许程序获取当前或最近运行的应用 |
| android.permission.GLOBAL_SEARCH | 允许全局搜索 | 允许程序使用全局搜索功能 |
| android.permission.HARDWARE_TEST | 硬件测试 | 访问硬件辅助设备,用于硬件测试 |
| android.permission.INJECT_EVENTS | 注射事件 | 允许访问本程序的底层事件,获取按键、轨迹球的事件流 |
| android.permission.INSTALL_LOCATION_PROVIDER | 安装定位提供 | 安装定位提供 |
| android.permission.INSTALL_PACKAGES | 安装应用程序 | 允许程序安装应用 |
| android.permission.INTERNAL_SYSTEM_WINDOW | 内部系统窗口 | 允许程序打开内部窗口,不对第三方应用程序开放此权限 |
| android.permission.INTERNET | 访问网络 | 访问网络连接,可能产生GPRS流量 |
| android.permission.KILL_BACKGROUND_PROCESSES | 结束后台进程 | 允许程序调用killBackgroundProcesses(String).方法结束后台进程 |
| android.permission.MANAGE_ACCOUNTS | 管理账户 | 允许程序管理AccountManager中的账户列表 |
| android.permission.MANAGE_APP_TOKENS | 管理程序引用 | 管理创建、摧毁、Z轴顺序,仅用于系统 |
| android.permission.MTWEAK_USER | 高级权限 | 允许mTweak用户访问高级系统权限 |
| android.permission.MTWEAK_FORUM | 社区权限 | 允许使用mTweak社区权限 |
| android.permission.MASTER_CLEAR | 软格式化 | 允许程序执行软格式化,删除系统配置信息 |
| android.permission.MODIFY_AUDIO_SETTINGS | 修改声音设置 | 修改声音设置信息 |
| android.permission.MODIFY_PHONE_STATE | 修改电话状态 | 修改电话状态,如飞行模式,但不包含替换系统拨号器界面 |
| android.permission.MOUNT_FORMAT_FILESYSTEMS | 格式化文件系统 | 格式化可移动文件系统,比如格式化清空SD卡 |
| android.permission.MOUNT_UNMOUNT_FILESYSTEMS | 挂载文件系统 | 挂载、反挂载外部文件系统 |
| android.permission.NFC | 允许NFC通讯 | 允许程序执行NFC近距离通讯操作,用于移动支持 |
| android.permission.PERSISTENT_ACTIVITY | 永久Activity | 创建一个永久的Activity,该功能标记为将来将被移除 |
| android.permission.PROCESS_OUTGOING_CALLS | 处理拨出电话 | 允许程序监视,修改或放弃播出电话 |
| android.permission.READ_CALENDAR | 读取日程提醒 | 允许程序读取用户的日程信息 |
| android.permission.READ_CONTACTS | 读取联系人 | 允许应用访问联系人通讯录信息 |
| android.permission.READ_FRAME_BUFFER | 屏幕截图 | 读取帧缓存用于屏幕截图 |
| com.android.browser.permission.READ_HISTORY_BOOKMARKS | 读取收藏夹和历史记录 | 读取浏览器收藏夹和历史记录 |
| android.permission.READ_INPUT_STATE | 读取输入状态 | 读取当前键的输入状态,仅用于系统 |
| android.permission.READ_LOGS | 读取系统日志 | 读取系统底层日志 |
| android.permission.READ_PHONE_STATE | 读取电话状态 | 访问电话状态 |
| android.permission.READ_SMS | 读取短信内容 | 读取短信内容 |
| android.permission.READ_SYNC_SETTINGS | 读取同步设置 | 读取同步设置,读取Google在线同步设置 |
| android.permission.READ_SYNC_STATS | 读取同步状态 | 读取同步状态,获得Google在线同步状态 |
| android.permission.REBOOT | 重启设备 | 允许程序重新启动设备 |
| android.permission.RECEIVE_BOOT_COMPLETED | 开机自动允许 | 允许程序开机自动运行 |
| android.permission.RECEIVE_MMS | 接收彩信 | 接收彩信 |
| android.permission.RECEIVE_SMS | 接收短信 | 接收短信 |
| android.permission.RECEIVE_WAP_PUSH | 接收Wap Push | 接收WAP PUSH信息 |
| android.permission.RECORD_AUDIO | 录音 | 录制声音通过手机或耳机的麦克 |
| android.permission.REORDER_TASKS | 排序系统任务 | 重新排序系统Z轴运行中的任务 |
| android.permission.RESTART_PACKAGES | 结束系统任务 | 结束任务通过restartPackage(String)方法,该方式将在外来放弃 |
| android.permission.SEND_SMS | 发送短信 | 发送短信 |
| android.permission.SET_ACTIVITY_WATCHER | 设置Activity观察其 | 设置Activity观察器一般用于monkey测试 |
| com.android.alarm.permission.SET_ALARM | 设置闹铃提醒 | 设置闹铃提醒 |
| android.permission.SET_ALWAYS_FINISH | 设置总是退出 | 设置程序在后台是否总是退出 |
| android.permission.SET_ANIMATION_SCALE | 设置动画缩放 | 设置全局动画缩放 |
| android.permission.SET_DEBUG_APP | 设置调试程序 | 设置调试程序,一般用于开发 |
| android.permission.SET_ORIENTATION | 设置屏幕方向 | 设置屏幕方向为横屏或标准方式显示,不用于普通应用 |
| android.permission.SET_PREFERRED_APPLICATIONS | 设置应用参数 | 设置应用的参数,已不再工作具体查看addPackageToPreferred(String)介绍 |
| android.permission.SET_PROCESS_LIMIT | 设置进程限制 | 允许程序设置最大的进程数量的限制 |
| android.permission.SET_TIME | 设置系统时间 | 设置系统时间 |
| android.permission.SET_TIME_ZONE | 设置系统时区 | 设置系统时区 |
| android.permission.SET_WALLPAPER | 设置桌面壁纸 | 设置桌面壁纸 |
| android.permission.SET_WALLPAPER_HINTS | 设置壁纸建议 | 设置壁纸建议 |
| android.permission.SIGNAL_PERSISTENT_PROCESSES | 发送永久进程信号 | 发送一个永久的进程信号 |
| android.permission.STATUS_BAR | 状态栏控制 | 允许程序打开、关闭、禁用状态栏 |
| android.permission.SUBSCRIBED_FEEDS_READ | 访问订阅内容 | 访问订阅信息的数据库 |
| android.permission.SUBSCRIBED_FEEDS_WRITE | 写入订阅内容 | 写入或修改订阅内容的数据库 |
| android.permission.SYSTEM_ALERT_WINDOW | 显示系统窗口 | 显示系统窗口 |
| android.permission.UPDATE_DEVICE_STATS | 更新设备状态 | 更新设备状态 |
| android.permission.USE_CREDENTIALS | 使用证书 | 允许程序请求验证从AccountManager |
| android.permission.USE_SIP | 使用SIP视频 | 允许程序使用SIP视频服务 |
| android.permission.VIBRATE | 使用振动 | 允许振动 |
| android.permission.WAKE_LOCK | 唤醒锁定 | 允许程序在手机屏幕关闭后后台进程仍然运行 |
| android.permission.WRITE_APN_SETTINGS | 写入GPRS接入点设置 | 写入网络GPRS接入点设置 |
| android.permission.WRITE_CALENDAR | 写入日程提醒 | 写入日程,但不可读取 |
| android.permission.WRITE_CONTACTS | 写入联系人 | 写入联系人,但不可读取 |
| android.permission.WRITE_EXTERNAL_STORAGE | 写入外部存储 | 允许程序写入外部存储,如SD卡上写文件 |
| android.permission.WRITE_GSERVICES | 写入Google地图数据 | 允许程序写入Google Map服务数据 |
| com.android.browser.permission.WRITE_HISTORY_BOOKMARKS | 写入收藏夹和历史记录 | 写入浏览器历史记录或收藏夹,但不可读取 |
| android.permission.WRITE_SECURE_SETTINGS | 读写系统敏感设置 | 允许程序读写系统安全敏感的设置项 |
| android.permission.WRITE_SETTINGS | 读写系统设置 | 允许读写系统设置项 |
| android.permission.WRITE_SMS | 编写短信 | 允许编写短信 |