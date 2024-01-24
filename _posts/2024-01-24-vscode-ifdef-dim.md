---
layout: post
title: "VSCode 的 #ifdef 套色"
---

在用 VSCode 處理 C/C++ 檔案的時候，如果裝了官方的 C/C++ extension ，會發現 `#ifdef` 預設是沒有套色的，查了一輪資料又再試了一下有找到有幾個方法來解決。

![](/assets/images/2024-01-24/ifdef.png)

---

### 取消 Dim Inactive Regions

在 VSCode 的設定中，找到 `C_Cpp: Dim Inactive Regions` 這個選項，把它關掉：

![Dim Inactive Regions](/assets/images/2024-01-24/dim-inactive-regions.png)

如果是直接改 `settings.json` （或是 `.vscode/settings.json` ） 的話就是加上 `"C_Cpp.dimInactiveRegions": false` 。

---

### 設定 C/C++ IntelliSense

用 GUI 設定的話就是找到 `C/C++: 編輯組態 (UI)` ，然後設定 `進階設定` 裡面的 `編譯命令` ，把它設成 `${workspaceFolder}/build/compile_commands.json` 。

![C/C++ IntelliSense](/assets/images/2024-01-24/c-cpp-edit-ui.png)

![C/C++ IntelliSense](/assets/images/2024-01-24/compile-commands.png)

或是也可以直接新增 `.vscode/c_cpp_properties.json` ，內容可以參考 [Configure C/C++ IntelliSense](https://code.visualstudio.com/docs/cpp/configure-intellisense) 和 [c_cpp_properties.json reference](https://code.visualstudio.com/docs/cpp/c-cpp-properties-schema-reference) ，主要就是要設定 `"compileCommands": "${workspaceFolder}/build/compile_commands.json"` 。

設定好之後要想辦法產生 `compile_commands.json` ，如果是用 CMake 的話就是直接加上 `-DCMAKE_EXPORT_COMPILE_COMMANDS=ON` ，可以參考 [CMAKE_EXPORT_COMPILE_COMMANDS](https://cmake.org/cmake/help/latest/variable/CMAKE_EXPORT_COMPILE_COMMANDS.html) 。

---

### 使用 CMake Tools Extensions

不過如果是用 CMake 的話，最簡單的方法應該是直接用官方的 [CMake Tools extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cmake-tools) ，裝完照著推薦跑完一次 configure 就會自動吃到 `CMakeLists.txt` 裡的設定了，不用設定什麼 `compileCommands` 也不用手動加 CMake 的參數。

這種情況下如果想要修改 CMake 的參數，可以參考 [Configure CMake Tools settings](https://github.com/microsoft/vscode-cmake-tools/blob/main/docs/cmake-settings.md) 裡的 `cmake.configureArgs` 和 `cmake.buildArgs` ，如果常常有不同的參數組合的話，可以參考 [Configure with CMake Tools tasks](https://github.com/microsoft/vscode-cmake-tools/blob/main/docs/tasks.md) 來考慮直接新增 `tasks.json` 來處理。
