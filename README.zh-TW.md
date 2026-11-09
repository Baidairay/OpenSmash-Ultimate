# OpenSmash-Ultimate for Android (繁體中文版 / Traditional Chinese)

[English README](README.md)

**任天堂明星大亂鬥 特別版 (Super Smash Bros. Ultimate) Android 移植版，重編譯為原生 arm64 機器碼並透過 Vulkan 進行繪圖。**

本專案為 [OpenSmash-Ultimate (Denkishi)](https://github.com/Denkishi/ssbu-recomp) 以及 [hapqe/ssbu-recomp](https://github.com/hapqe/ssbu-recomp) 的 Android 移植版 Fork，並在此基礎上進行了諸多針對 Android 手機／平板的本地化、連線功能、震動反饋與遊戲體驗優化。

![Platform](https://img.shields.io/badge/platform-Android%20arm64-lightgrey)
![Game versions](https://img.shields.io/badge/game-13.0.4%20%7C%2013.0.5-green)
![License: GPL-3.0](https://img.shields.io/badge/license-GPL--3.0-blue)

> **本儲存庫遵循 GNU GPL v3.0 開源協議。**
> 
> **本儲存庫不包含任何遊戲專利代碼、遊戲資源（Assets）、金鑰（Keys）或系統韌體（Firmware）。**
> 使用者必須自行提供合法獲取的遊戲檔案（版本 13.0.4 或 13.0.5）以及提取自自己 Switch 的 `prod.keys`。

---

## 本 Fork 主要修改與增強項目 (Modifications & Improvements)

相較於原版移植，本版本進行了以下針對行動裝置與玩家體驗的深度優化：

1. **應用程式名稱與圖示更新 (App Branding & Icons)**：
   - 應用程式名稱正式更新為「**任天堂明星大亂鬥**」。
   - 更換高品質大亂鬥標誌（Super Smash Bros. Logo）為應用程式啟動圖示（適配 mdpi ~ xxxhdpi 各種解析度）。
2. **啟動流程無縫直入與返回控制 (Seamless Launch & Navigation)**：
   - App 點擊開啟後，若遊戲檔案準備完成即自動進入遊戲（略過每次手動點擊 Play），達到如原機一般的流暢體驗。
   - 遊戲進行時可隨時透過系統側滑返回手勢（Back Gesture）直接退回設定選單，以便快速調整驅動或圖形選項。
3. **後台智慧音訊暫停與恢復 (Audio Focus & Background Pause)**：
   - 修正遊戲在滑動切換到後台或回桌面時音樂持續播放的問題。
   - 完整支援 Android 系統音訊焦點（AudioFocus）與應用週期監聽：切換到後台即刻靜音暫停，切回遊戲無縫恢復。
4. **字型與方框缺字修復 (Font Glyphs & Fallback)**：
   - 解決部分中文介面文字顯示為方框的問題，確保遊戲與選單中的文字正常顯示。
5. **手把/機身智慧震動調度與獨立開關 (Smart Vibration Dispatch)**：
   - **外接實體手把**：震動效果優先且精準輸出至手把馬達。
   - **螢幕觸控操作**：未連接外接手把時，自動調度至 Android 裝置內建震動馬達，提供打擊與受擊的震動回饋。
   - **多人獨立震動**：本地 1～8 人對戰時，各玩家角色的震動僅傳遞至其對應的手把，互不干擾。
   - **設定開關**：啟動器設定中新增「外接手把震動」與「裝置機身震動」獨立切換開關。
   - **手把識別**：所有外接控制器與虛擬觸控預設均以 Switch Pro Controller (`STYLE_FULLKEY`) 規格回報給遊戲。
6. **語言預設以英文為主並支援快速切換 (Language Switcher)**：
   - 預設語言調整為 **English (US)**，同時在啟動器中新增直覺的語言選單（Language Spinner）。
   - 支援快速切換為 **繁體中文 (Traditional Chinese)**、**简体中文 (Simplified Chinese)**、**日本語 (Japanese)**、法語、德語、義大利語、西班牙語、韓語等全球 14 種官方語言，免去手動編輯環境變數。
7. **跨裝置鄰近通訊連線 (Local Wireless / nn::ldn)**：
   - 啟用並鏈接完整 `net_relay.c` 模組，修復 Linux/Android 下的符號依賴，使 Android 裝置具備跨裝置鄰近通訊能力。

---

## 系統需求 (Requirements)

- **Android 11+**，**arm64** 架構手機或平板，建議 **8 GB RAM** 以上，剩餘儲存空間約 **25 GB**。
- 支援 **Vulkan 1.3** 的 GPU。Qualcomm Snapdragon (Adreno) 裝置建議在啟動器內安裝 **Mesa Turnip** 驅動包（AdrenoTools `.zip`）。
- 您自己合法擁有的 Switch 檔案：`prod.keys`（可選 `title.keys`）、本體遊戲 `.nsp`（或 `.xci`）、更新檔 `.nsp`（13.0.4 或 13.0.5），以及可選的 DLC 與存檔資料。
- 僅需在電腦端執行一次遊戲程式庫重編譯（步驟 1）。

---

## 安裝與使用指南 (Setup Guide)

### 1. 在電腦上編譯遊戲庫（僅需一次）
```bash
python3 android/tools/build_game_android.py --dump <您的 Dump 資料夾> --package
```
完成後會生成 `android/build/game/OpenSmash-libraries-<版本>.zip`（約 1 GB），傳輸至手機／平板中。

### 2. 在 Android 裝置上設定
1. 安裝並開啟本 APK（允許所有檔案管理權限）。
2. 進入 **Game files > Set up from Switch files…**，依精靈引導選取遊戲本體、更新檔、金鑰檔案以解包 RomFS。
3. 在 **Game files > Import libraries (.zip)…** 匯入步驟 1 的遊戲庫。
4. Adreno 處理器裝置可於 **GPU driver > Install a driver (.zip)…** 載入 Turnip 驅動。
5. 點擊 **Play** 開始遊戲（初次啟動會進行著色器編譯，請耐心等待）。

---

## 自行編譯 APK (Building from Source)

詳細步驟請參閱 [docs/ANDROID.md](docs/ANDROID.md)。
在具備 Android SDK、NDK 29、JDK 17 與建置依賴的環境下執行：
```bash
cd android
./tools/build_native.sh
./build_apk.sh
```
建置產物將輸出至 `android/build/OpenSmash.apk`。

---

## 開源許可與致謝 (License & Credits)

- 本專案基於 **GNU General Public License v3.0 (GPL-3.0)** 條款分發。詳細許可條款見 [LICENSE](LICENSE)。
- 致敬並感謝 [Denkishi/ssbu-recomp](https://github.com/Denkishi/ssbu-recomp) 以及 [hapqe/ssbu-recomp](https://github.com/hapqe/ssbu-recomp) 開發者們的卓越工作。
- 感謝 **Mesa Turnip**、**libadrenotools**、**SDL3**、**glslang**、**libopus** 等開源社群專案。
