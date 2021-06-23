# windows terminal 配置範例
# Terminal，win10 的最佳終端

阿新 • 來源：網路 • 發佈：2021-01-03

(adsbygoogle = window.adsbygoogle || \[]).push({});  

Terminal

-   是我開發歷程中，接觸的、第二個微軟 Microsoft 開源的開發者工具
    -   第一個是 VS Code）。
-   吸引我的是
    -   高自定義性
    -   可擴充套件性
    -   UI（個人審美比較……hh

# 詳細配置

## 毛玻璃

-   在配置檔案`profiles.json`中的`profiles`，設定引數

```shell
"profiles": 
[
	{
	//開啟毛玻璃特效
	"useAcrylic": true,
	}
],

```

## 設定 Powerline

> 來自[微軟官方文件](https://docs.microsoft.com/zh-cn/windows/terminal/tutorials/powerline-setup)

-   Powerline 提供自定義的命令提示符體驗，提供 Git 狀態顏色編碼和提示符。

# 右鍵新增 “在此處開啟 Terminal”

-   新建一個登錄檔檔案`.reg`，內容如下：

```bash
Windows Registry Editor Version 5.00
 
[HKEY_CLASSES_ROOT\Directory\Background\shell\wt]
@="Windows Terminal Here"
"Icon"="C:\\Users\\Administrator\\terminal.ico"

[HKEY_CLASSES_ROOT\Directory\Background\shell\wt\command]
@="C:\\Program 

```

# 參考文章

1.  [微軟官方文件](https://docs.microsoft.com/zh-cn/windows/terminal/)
2.  [windows terminal 安裝與毛玻璃教程](https://blog.n0ts.cn/1139.html)
3.  [Windows Terminal 完美配置 PowerShell 7.1](https://zhuanlan.zhihu.com/p/137595941)

# 完整配置檔案

```json

// To view the default settings, hold "alt" while clicking on the "Settings" button.
// For documentation on these settings, see: https://aka.ms/terminal-documentation

{
    "$schema": "https://aka.ms/terminal-profiles-schema",
    "theme": "dark",
        "profiles": [
            {
                "name" : "Powershell",
                "source" : "Windows.Terminal.PowershellCore",
                "acrylicOpacity" : 0.3,
                "colorScheme" : "Campbell",
                "cursorColor" : "#FFFFFD",
                "fontFace" : "Cascadia Code PL",
                "useAcrylic" : true,
                "acrylic" : 0.3
            }
        ],

    "defaultProfile": "{574e775e-4f2a-5b96-ac1e-a2962a402336}",

    "profiles":
    {
        "defaults":
        {
            // Put settings here that you want to apply to all profiles
            //將"在此處啟動Windows Terminal"新增到右鍵選單
            "startingDirectory" : ".",
        },
        "list":
        [
			{
				// Powershell 7.1.0-preview.2 配置
				"guid": "{574e775e-4f2a-5b96-ac1e-a2962a402336}",
				"hidden": false,
				"name": "pwsh",
				// 注意：一定要寫上 -nologo，否則開啟 powershll 會有一段話輸出，很討厭！
				"commandline": "C:/Program Files/PowerShell/7-preview/pwsh.exe -nologo",
				"source": "Windows.Terminal.PowershellCore",
				// 啟動選單一定要設定為 <.>，否則後面重要的一步將會無效！
				"startingDirectory": ".",
				// 字型
				"fontFace": "Cascadia Code PL",
				"fontSize": 11,
				"historySize": 9001,
				"padding": "5, 5, 20, 25",
				"snapOnInput": true,
				"useAcrylic": false,
				//配色方案
				"colorScheme": "Homebrew",
				//透明度
				"backgroundImageOpacity" : 0.1,
				//毛玻璃
				"useAcrylic" : true,
				//標題
				"tabTitle" : "PowerShell",
				//未知
				"acrylicOpacity": 0.1,
			},
            {
                // Make changes here to the powershell.exe profile
                "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
                "name": "Windows PowerShell",
                "commandline": "powershell.exe",
                "hidden": false,
                //配色方案
				"colorScheme": "Raspberry",
				//透明度
				"backgroundImageOpacity" : 0.1,
				//毛玻璃
				"useAcrylic" : true,
            },
            {
                // Make changes here to the cmd.exe profile
                "guid": "{0caa0dad-35be-5f56-a8ff-afceeeaa6101}",
                "name": "cmd",
                "commandline": "cmd.exe",
                "hidden": false,
                //配色方案
				"colorScheme": "Frost",
				//透明度
				"backgroundImageOpacity" : 0.3,
				//毛玻璃
				"useAcrylic" : true,
				//未知
				"acrylicOpacity": 0.1,
				"acrylic" : 0.3,
            },
            {
                "guid": "{b453ae62-4e3d-5e58-b989-0a998ec441b8}",
                "hidden": false,
                "name": "Azure Cloud Shell",
                "source": "Windows.Terminal.Azure"
            },
            {
                "guid": "{574e775e-4f2a-5b96-ac1e-a2962a402336}",
                "hidden": false,
                "name": "PowerShell",
                "source": "Windows.Terminal.PowershellCore"
            }
        ]
    },

    // Add custom color schemes to this array
    "schemes": [
		{
			//預設
			//配色方案
			"name": "Homebrew",
			"black": "#000000",
			"red": "#FC5275",
			"green": "#00a600",
			"yellow": "#999900",
			"blue": "#6666e9",
			"purple": "#b200b2",
			"cyan": "#00a6b2",
			"white": "#bfbfbf",
			"brightBlack": "#666666",
			"brightRed": "#e50000",
			"brightGreen": "#00d900",
			"brightYellow": "#e5e500",
			"brightBlue": "#0000ff",
			"brightPurple": "#e500e5",
			"brightCyan": "#00e5e5",
			"brightWhite": "#e5e5e5",
			"background": "#757575",
			"foreground": "#00ff00"
		},
		{
			//配色方案
			//Ubuntu款式
            "name" : "Raspberry",
            "background" : "#3C0315",
            "black" : "#282A2E",
            "blue" : "#0170C5",
            "brightBlack" : "#676E7A",
            "brightBlue" : "#80c8ff",
            "brightCyan" : "#8ABEB7",
            "brightGreen" : "#B5D680",
            "brightPurple" : "#AC79BB",
            "brightRed" : "#BD6D85",
            "brightWhite" : "#FFFFFD",
            "brightYellow" : "#FFFD76",
            "cyan" : "#3F8D83",
            "foreground" : "#FFFFFD",
            "green" : "#76AB23",
            "purple" : "#7D498F",
            "red" : "#BD0940",
            "white" : "#FFFFFD",
            "yellow" : "#E0DE48",
            "foreground": "#00ff00"
         },
{
			"name" : "Frost",
			"background" : "#FFFFFF",
			"black" : "#3C5712",
			"blue" : "#17b2ff",
			"brightBlack" : "#749B36",
			"brightBlue" : "#27B2F6",
			"brightCyan" : "#13A8C0",
			"brightGreen" : "#89AF50",
			"brightPurple" : "#F2A20A",
			"brightRed" : "#F49B36",
			"brightWhite" : "#741274",
			"brightYellow" : "#991070",
			"cyan" : "#3C96A6",
			"foreground" : "#000000",
			"green" : "#6AAE08",
			"purple" : "#991070",
			"red" : "#8D0C0C",
			"white" : "#6E386E",
			"yellow" : "#991070"
         },
    ],

    // Add any keybinding overrides to this array.
    // To unbind a default keybinding, set the command to "unbound"
    "keybindings": [
		//自定義搜尋鍵繫結, Press ctrl+shift+f to open the search box
        { "command": "find", "keys": "ctrl+f" },
    ]
}

```

(adsbygoogle = window.adsbygoogle || \[]).push({});

 [https://www.796t.com/article.php?id=214605](https://www.796t.com/article.php?id=214605)
