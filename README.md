# LEDOMARS 點陣式印表機驅動程式與操作手冊

收錄 13 款 LEDOMARS（富泰科技）點陣式印表機的 Windows 驅動程式、操作手冊與網路設定工具，方便部署時直接取得。

> [!NOTE]
> 本 repo **並非 LEDOMARS 官方維護**。最新版本請優先至 [LEDOMARS 官方下載專區](http://www.ledomars.com.tw/download.html) 取得；版權說明見文末。

[English summary](#english)

## 工程師memo （Eryet）

找他們家的驅動程式有夠難找，他們網站驅動不齊全還經常挂掉，這邊整理它們的驅動，至少讓未來有需要的人用 AI 爬得到

## 收錄機型

表中版本號取自各驅動程式 INF 檔的 `DriverVer`。

| 機型                    | Windows 8 / 10         | Windows 2000 – 7       | Windows 98 / Me | 操作手冊                                      |
| ----------------------- | ---------------------- | ---------------------- | --------------- | --------------------------------------------- |
| [BP-2000](BP-2000/)     | 1.0.0.1（2018-07-24）  | 1.2.0.0（2019-01-08）  | ✓               | [PDF](BP-2000/Manual/BP-2000操作手冊.pdf)     |
| [BP-880](BP-880/)       | 1.2.0.0（2018-10-16）  | 1.3.0.0（2019-01-08）  | ✓               | [PDF](BP-880/Manual/BP-880操作手冊.pdf)       |
| [FB-500](FB-500/)       | 1.3.0.2（2020-06-22）  | 1.3.0.2（2020-06-22）  | —               | [PDF](FB-500/Manual/FB-500操作手冊.pdf)       |
| [LP-1000](LP-1000/)     | 1.0.0.2（2020-09-10）  | 1.0.0.4（2020-10-15）  | —               | [PDF](LP-1000/Manual/LP-1000操作手冊.pdf)     |
| [LP-2000](LP-2000/)     | 1.0.0.2（2020-09-11）  | 1.0.0.3（2020-10-16）  | —               | [PDF](LP-2000/Manual/LP-2000操作手冊.pdf)     |
| [LP-3000](LP-3000/)     | 1.0.0.1（2020-09-11）  | 1.0.0.1（2020-09-16）  | —               | [PDF](LP-3000/Manual/LP-3000操作手冊.pdf)     |
| [LP-7000E](LP-7000E/)   | 1.1.0.0（2018-07-24）  | 1.2.0.0（2019-01-08）  | ✓               | [PDF](LP-7000E/Manual/LP-7000E操作手册.pdf)   |
| [LP-7000S](LP-7000S/)   | —                      | 1.4.0.0（2016-02-01）¹ | ✓               | [PDF](LP-7000S/Manual/LP-7000S操作手冊.pdf)   |
| [LP-7580](LP-7580/)     | 1.0.0.0（2018-07-26）² | 1.2.0.1（2019-01-08）  | ✓               | [PDF](LP-7580/Manual/LP-7580操作手冊.pdf)     |
| [LP-7800IV](LP-7800IV/) | 1.0.0.0（2018-07-25）² | 1.4.0.1（2019-01-08）  | ✓               | [PDF](LP-7800IV/Manual/LP-7800IV操作手冊.pdf) |
| [LP-8000A](LP-8000A/)   | 1.0.0.0（2020-09-23）² | 1.0.0.0（2020-09-23）  | —               | [PDF](LP-8000A/Manual/LP-8000A操作手冊.pdf)   |
| [LP-8000E](LP-8000E/)   | 1.0.0.0（2018-07-26）² | 1.4.0.1（2019-01-08）  | ✓               | [PDF](LP-8000E/Manual/LP-8000E操作手冊.pdf)   |
| [LP-8000II](LP-8000II/) | 1.0.0.0（2018-07-26）² | 1.0.0.0（2020-08-24）  | —               | [PDF](LP-8000II/Manual/LP-8000II操作手冊.pdf) |

1. LP-7000S 的資料夾標示為 XP – Vista，未列出 Windows 7。
2. 這些機型的 Windows 8 / 10 版使用通用 INF `JMDMP24B.INF`，而非以機型命名的 INF。

另附工具：[NetFinder](#網路設定工具-netfinder)（`Tool/NetFinder/`）。

## 目錄結構

```text
<機型>/
├── Drivers/
│   ├── WIN8(WIN10)/              Windows 8 / 10
│   │   ├── Setup.exe             安裝程式
│   │   ├── <機型>.INF            手動安裝用
│   │   ├── I386/  X64/           32 / 64 位元檔案（部分機型另有 IA64/）
│   │   └── Readme*.txt           廠商安裝說明（英文 / 繁體中文）
│   ├── WIN2000(XP-Vista-Win7)/   Windows 2000 – 7
│   └── WIN98(WINME)/             Windows 98 / Me（部分機型才有）
└── Manual/
    └── <機型>操作手冊.pdf
```

資料夾命名依機型分為兩種，下載時請留意：

| 作業系統         | LP-1000、LP-2000、LP-3000 | 其他機型                                                    |
| ---------------- | ------------------------- | ----------------------------------------------------------- |
| Windows 8 / 10   | `WIN10(WIN8)`             | `WIN8(WIN10)`                                               |
| Windows 2000 – 7 | `WIN7(XP-Vista-Win2000)`  | `WIN2000(XP-Vista-Win7)`（LP-7000S 為 `WIN2000(XP-Vista)`） |
| Windows 98 / Me  | —                         | `WIN98(WINME)`                                              |

## 只下載單一機型

完整 repo 解壓後約 388 MB，單一機型約 30 – 40 MB。只需要一款時，可用 sparse checkout：

```bash
git clone --filter=blob:none --sparse https://github.com/Hikaru-tw/ledomars-printer-driver.git
cd ledomars-printer-driver
git sparse-checkout set LP-2000
```

## 安裝驅動程式

開始前：

- 接好傳輸線並開啟印表機電源。
- 若電腦已安裝舊版驅動程式，請先移除。
- 安裝需要系統管理員權限。

### 方法一：執行 Setup.exe（建議）

1. 進入對應作業系統的資料夾，執行 `Setup.exe`。
2. 選擇印表機機型，點選安裝。
3. 若出現數位簽章或 Windows 安全性提示，選擇繼續安裝。
4. 出現安裝完成訊息後，關閉安裝視窗。

> [!IMPORTANT]
> Windows 8 / 10 資料夾中的 `Setup.exe` 會先執行同資料夾的 `addcert.exe` 匯入憑證（設定於 `Setup.ini` 的 `PreRun`）。若貴單位有端點管控政策，請先確認是否允許。

### 方法二：新增印表機精靈（手動指定 INF）

1. 開啟 Windows 的「新增印表機」精靈，選擇「從磁片安裝」。
2. 瀏覽至對應作業系統的資料夾，選擇該資料夾根目錄的 INF 檔（精靈會自動選用 `I386` 或 `X64` 內的檔案）：
   - 一般機型：機型名稱去掉連字號，例如 LP-2000 為 `LP2000.inf`。**LP-8000II 為 `LP80002.INF`**。
   - LP-7580、LP-7800IV、LP-8000A、LP-8000E、LP-8000II 的 Windows 8 / 10 版：`JMDMP24B.INF`。
3. 依精靈指示完成安裝。

Windows 98 / Me 以 USB 連接時，另需安裝 `WIN98(WINME)/USBdriver/` 中的 USB 列印驅動程式。各系統的詳細步驟見驅動程式資料夾內的 `ReadmeEN.txt` 或 `ReadmeCHT.txt`。

## 網路設定工具 NetFinder

`Tool/NetFinder/NetFinder.exe`（v1.0）用來顯示裝置的 MAC 位址，並設定 IP 位址與子網路遮罩。英文說明檔為同資料夾的 `help_en.chm`。

## 常見問題

**支援 Windows 11 嗎？**
廠商隨附的說明文件只列到 Windows 10，沒有提到 Windows 11。請自行實機測試，或洽詢廠商。

**下載後 `Setup.exe` 被 SmartScreen 攔截，或 `.chm` 說明檔開啟後一片空白？**
從網路下載的 ZIP 會被 Windows 標記為來自網際網路。請在**解壓縮前**對 ZIP 檔按右鍵 →「內容」→ 勾選「解除封鎖」，再解壓縮。

## 給開發者

- 以 RAW 模式（直接送 ESC/P 指令）列印時，資料不經驅動程式轉譯，但仍需要一個 Windows 印表機佇列作為送印目的地。安裝本驅動程式就會建立這個佇列。
- LP-2000 列印 Big5 中文時，送出 `FS &` 進入中文模式即可（出廠預設字集為 Big5）。實機測試發現：若在 `FS &` 之前送出 `ESC k 1`（選擇字型），會印出錯誤的中文字；`FS ! 2`、`FS t 3` 也不需要。

## 版權與免責聲明

- 本 repo 收錄的驅動程式、工具與操作手冊，著作權均屬 LEDOMARS 富泰科技或原開發廠商所有。本 repo 未對上述內容授予任何授權。
- 檔案以現狀提供，不附任何保證，使用風險自負。
- 官方最新版本請至 [LEDOMARS 官方下載專區](http://www.ledomars.com.tw/download.html)。
- 權利人如希望移除內容，請開 Issue 聯絡，我們會儘速處理。

---

## English

An **unofficial** mirror of Windows drivers, user manuals (Chinese), and the NetFinder network tool for 13 LEDOMARS dot-matrix printer models: BP-2000, BP-880, FB-500, LP-1000, LP-2000, LP-3000, LP-7000E, LP-7000S, LP-7580, LP-7800IV, LP-8000A, LP-8000E, and LP-8000II.

- **Supported OS:** Windows 98 through Windows 10, per the vendor's bundled documentation. Windows 11 is not mentioned by the vendor.
- **Install:** run `Setup.exe` from `<Model>/Drivers/<OS folder>/`, or use Add Printer → Have Disk and pick the `.INF` in that folder. On Windows 8/10, `Setup.exe` first runs `addcert.exe` to import a certificate.
- **One model only:** `git clone --filter=blob:none --sparse <repo-url>`, then `git sparse-checkout set <Model>`.
- **Copyright:** all files remain the property of LEDOMARS (富泰科技) or the original developers, and no license is granted by this repository. The files are provided as-is. Get official downloads from the [LEDOMARS download page](http://www.ledomars.com.tw/download.html). Rights holders can open an issue to request removal.
