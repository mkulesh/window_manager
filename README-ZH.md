# window_manager

[![pub version][pub-image]][pub-url] [![][discord-image]][discord-url]

[pub-image]: https://img.shields.io/pub/v/window_manager.svg
[pub-url]: https://pub.dev/packages/window_manager

[discord-image]: https://img.shields.io/discord/884679008049037342.svg
[discord-url]: https://discord.gg/zPa6EZ2jqb

这个插件允许 Flutter **桌面** 应用调整窗口的大小和位置。

---

[English](./README.md) | 简体中文

---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [window_manager](#window_manager)
  - [平台支持](#平台支持)
  - [快速开始](#快速开始)
    - [安装](#安装)
    - [用法](#用法)
      - [监听事件](#监听事件)
      - [关闭时退出](#关闭时退出)
        - [macOS](#macos)
        - [Windows](#windows)
      - [在启动时隐藏](#在启动时隐藏)
        - [macOS](#macos-1)
  - [谁在用使用它？](#谁在用使用它)
  - [API](#api)
    - [WindowManager](#windowmanager)
      - [Methods](#methods)
        - [focus](#focus)
        - [blur  `macos`  `windows`](#blur--macos--windows)
        - [show](#show)
        - [hide](#hide)
        - [isVisible](#isvisible)
        - [isMaximized](#ismaximized)
        - [maximize](#maximize)
        - [unmaximize](#unmaximize)
        - [isMinimized](#isminimized)
        - [minimize](#minimize)
        - [restore](#restore)
        - [isFullScreen](#isfullscreen)
        - [setFullScreen](#setfullscreen)
        - [setBackgroundColor](#setbackgroundcolor)
        - [getBounds](#getbounds)
        - [setBounds](#setbounds)
        - [getPosition](#getposition)
        - [setPosition](#setposition)
        - [getSize](#getsize)
        - [setSize](#setsize)
        - [setMinimumSize](#setminimumsize)
        - [setMaximumSize](#setmaximumsize)
        - [isResizable](#isresizable)
        - [setResizable](#setresizable)
        - [isMovable  `macos`](#ismovable--macos)
        - [setMovable  `macos`](#setmovable--macos)
        - [isMinimizable  `macos`  `windows`](#isminimizable--macos--windows)
        - [setMinimizable  `macos`  `windows`](#setminimizable--macos--windows)
        - [isClosable  `macos`  `windows`](#isclosable--macos--windows)
        - [setClosable  `macos`  `windows`](#setclosable--macos--windows)
        - [isAlwaysOnTop](#isalwaysontop)
        - [setAlwaysOnTop](#setalwaysontop)
        - [getTitle](#gettitle)
        - [setTitle](#settitle)
        - [setTitleBarStyle  `macos`  `windows`](#settitlebarstyle--macos--windows)
        - [getTitleBarHeight  `macos`  `windows`](#gettitlebarheight--macos--windows)
        - [setSkipTaskbar](#setskiptaskbar)
        - [setProgressBar  `macos`](#setprogressbar--macos)
        - [hasShadow  `macos`](#hasshadow--macos)
        - [setHasShadow  `macos`](#sethasshadow--macos)
        - [getOpacity  `macos`  `windows`](#getopacity--macos--windows)
        - [setOpacity  `macos`  `windows`](#setopacity--macos--windows)
        - [startDragging](#startdragging)
    - [WindowListener](#windowlistener)
      - [Methods](#methods-1)
        - [onWindowFocus](#onwindowfocus)
        - [onWindowBlur](#onwindowblur)
        - [onWindowMaximize](#onwindowmaximize)
        - [onWindowUnmaximize](#onwindowunmaximize)
        - [onWindowMinimize](#onwindowminimize)
        - [onWindowRestore](#onwindowrestore)
        - [onWindowResize](#onwindowresize)
        - [onWindowMove](#onwindowmove)
        - [onWindowEnterFullScreen](#onwindowenterfullscreen)
        - [onWindowLeaveFullScreen](#onwindowleavefullscreen)
        - [onWindowEvent](#onwindowevent)
  - [许可证](#许可证)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## 平台支持

| Linux | macOS | Windows |
| :---: | :---: | :-----: |
|   ✔️   |   ✔️   |    ✔️    |

## 快速开始

### 安装

将此添加到你的软件包的 pubspec.yaml 文件：

```yaml
dependencies:
  window_manager: ^0.1.4
```

或

```yaml
dependencies:
  window_manager:
    git:
      url: https://github.com/leanflutter/window_manager.git
      ref: main
```

### 用法

```dart
import 'package:flutter/material.dart';
import 'package:window_manager/window_manager.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  // 必须加上这一行。
  await windowManager.ensureInitialized();

  // Use it only after calling `hiddenWindowAtLaunch`
  windowManager.waitUntilReadyToShow().then((_) async{
    // 设置为无边框窗口
    await windowManager.setAsFrameless();
    await windowManager.setSize(Size(600, 600));
    await windowManager.setPosition(Offset.zero);
    windowManager.show();
  });

  runApp(MyApp());
}

```

> 请看这个插件的示例应用，以了解完整的例子。

#### 监听事件

```dart
import 'package:flutter/cupertino.dart';
import 'package:window_manager/window_manager.dart';

class HomePage extends StatefulWidget {
  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> with WindowListener {
  @override
  void initState() {
    windowManager.addListener(this);
    super.initState();
  }

  @override
  void dispose() {
    windowManager.removeListener(this);
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    // ...
  }

  @override
  void onWindowEvent(String eventName) {
    print('[WindowManager] onWindowEvent: $eventName');
  }

  @override
  void onWindowFocus() {
    // do something
  }

  @override
  void onWindowBlur() {
    // do something
  }

  @override
  void onWindowMaximize() {
    // do something
  }

  @override
  void onWindowUnmaximize() {
    // do something
  }

  @override
  void onWindowMinimize() {
    // do something
  }

  @override
  void onWindowRestore() {
    // do something
  }

  @override
  void onWindowResize() {
    // do something
  }

  @override
  void onWindowMove() {
    // do something
  }

  @override
  void onWindowEnterFullScreen() {
    // do something
  }

  @override
  void onWindowLeaveFullScreen() {
    // do something
  }
}
```
#### 关闭时退出

如果你需要使用 `hide` 方法，你需要禁用 `QuitOnClose`。

##### macOS

`macos/Runner/AppDelegate.swift`

```diff
import Cocoa
import FlutterMacOS

@NSApplicationMain
class AppDelegate: FlutterAppDelegate {
  override func applicationShouldTerminateAfterLastWindowClosed(_ sender: NSApplication) -> Bool {
-    return true
+    return false
  }
}
```

##### Windows

`windows/runner/main.cpp`

```diff
int APIENTRY wWinMain(_In_ HINSTANCE instance, _In_opt_ HINSTANCE prev,
                      _In_ wchar_t *command_line, _In_ int show_command) {
  // ...

-  window.SetQuitOnClose(true);
+  window.SetQuitOnClose(false);

  // ...

  return EXIT_SUCCESS;
}
```

#### 在启动时隐藏

##### macOS

```diff
import Cocoa
import FlutterMacOS
+import window_manager

class MainFlutterWindow: NSWindow {
    override func awakeFromNib() {
        let flutterViewController = FlutterViewController.init()
        let windowFrame = self.frame
        self.contentViewController = flutterViewController
        self.setFrame(windowFrame, display: true)

        RegisterGeneratedPlugins(registry: flutterViewController)

        super.awakeFromNib()
    }

+    override public func order(_ place: NSWindow.OrderingMode, relativeTo otherWin: Int) {
+        super.order(place, relativeTo: otherWin)
+        hiddenWindowAtLaunch()
+    }
}

```

## 谁在用使用它？

- [AuthPass](https://authpass.app/) - 基于Flutter的密码管理器，适用于所有平台。兼容Keepass 2.x（kdbx 3.x）。
- [Biyi (比译)](https://biyidev.com/) - 一个便捷的翻译和词典应用程序。
- [BlueBubbles](https://github.com/BlueBubblesApp/bluebubbles-app) - BlueBubbles is an ecosystem of apps bringing iMessage to Android, Windows, and Linux
- [Yukino](https://github.com/yukino-app/yukino/tree/flutter-rewrite) - Yukino lets you read manga or stream anime ad-free from multiple sources.
- [LunaSea](https://github.com/CometTools/LunaSea) - A self-hosted controller for mobile and macOS built using the Flutter framework.

## API

<!-- README_DOC_GEN -->
### WindowManager

#### Methods

##### focus

Focuses on the window.

##### blur  `macos`  `windows`

Removes focus from the window.


##### show

Shows and gives focus to the window.

##### hide

Hides the window.

##### isVisible

Returns `bool` - Whether the window is visible to the user.

##### isMaximized

Returns `bool` - Whether the window is maximized.

##### maximize

Maximizes the window.

##### unmaximize

Unmaximizes the window.

##### isMinimized

Returns `bool` - Whether the window is minimized.

##### minimize

Minimizes the window. On some platforms the minimized window will be shown in the Dock.

##### restore

Restores the window from minimized state to its previous state.

##### isFullScreen

Returns `bool` - Whether the window is in fullscreen mode.

##### setFullScreen

Sets whether the window should be in fullscreen mode.

##### setBackgroundColor

Sets the background color of the window.

##### getBounds

Returns `Rect` - The bounds of the window as Object.

##### setBounds

Resizes and moves the window to the supplied bounds.

##### getPosition

Returns `Offset` - Contains the window's current position.

##### setPosition

Moves window to position.

##### getSize

Returns `Size` - Contains the window's width and height.

##### setSize

Resizes the window to `width` and `height`.

##### setMinimumSize

Sets the minimum size of window to `width` and `height`.

##### setMaximumSize

Sets the maximum size of window to `width` and `height`.

##### isResizable

Returns `bool` - Whether the window can be manually resized by the user.

##### setResizable

Sets whether the window can be manually resized by the user.

##### isMovable  `macos`

Returns `bool` - Whether the window can be moved by user.


##### setMovable  `macos`

Sets whether the window can be moved by user.


##### isMinimizable  `macos`  `windows`

Returns `bool` - Whether the window can be manually minimized by the user.


##### setMinimizable  `macos`  `windows`

Sets whether the window can be manually minimized by user.


##### isClosable  `macos`  `windows`

Returns `bool` - Whether the window can be manually closed by user.


##### setClosable  `macos`  `windows`

Sets whether the window can be manually closed by user.


##### isAlwaysOnTop

Returns `bool` - Whether the window is always on top of other windows.

##### setAlwaysOnTop

Sets whether the window should show always on top of other windows.

##### getTitle

Returns `String` - The title of the native window.

##### setTitle

Changes the title of native window to title.

##### setTitleBarStyle  `macos`  `windows`

Changes the title bar style of native window.


##### getTitleBarHeight  `macos`  `windows`

Returns `int` - The title bar height of the native window.


##### setSkipTaskbar

Makes the window not show in the taskbar / dock.

##### setProgressBar  `macos`

Sets progress value in progress bar. Valid range is [0, 1.0].


##### hasShadow  `macos`

Returns `bool` - Whether the window has a shadow.


##### setHasShadow  `macos`

Sets whether the window should have a shadow.


##### getOpacity  `macos`  `windows`

Returns `double` - between 0.0 (fully transparent) and 1.0 (fully opaque). On Linux, always returns 1.


##### setOpacity  `macos`  `windows`

Sets the opacity of the window.


##### startDragging

Starts a window drag based on the specified mouse-down event.

### WindowListener

#### Methods

##### onWindowFocus

Emitted when the window gains focus.

##### onWindowBlur

Emitted when the window loses focus.

##### onWindowMaximize

Emitted when window is maximized.

##### onWindowUnmaximize

Emitted when the window exits from a maximized state.

##### onWindowMinimize

Emitted when the window is minimized.

##### onWindowRestore

Emitted when the window is restored from a minimized state.

##### onWindowResize

Emitted after the window has been resized.

##### onWindowMove

Emitted when the window is being moved to a new position.

##### onWindowEnterFullScreen

Emitted when the window enters a full-screen state.

##### onWindowLeaveFullScreen

Emitted when the window leaves a full-screen state.

##### onWindowEvent

Emitted all events.


<!-- README_DOC_GEN -->

## 许可证

[MIT](./LICENSE)
