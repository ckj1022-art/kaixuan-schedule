# 開瑄成長時刻表（只用 GitHub，資料加密）

只用 GitHub，不用其他服務：

- 課表資料用「檢視密碼」加密成 `data.enc.json`，放在 GitHub 上是一串看不懂的亂碼。
- **老師**：打開網頁，輸入檢視密碼就能看課表（在瀏覽器裡解密）。
- **管理者（你）**：先輸入檢視密碼，再按「管理登入」貼上 GitHub 金鑰，就能新增、修改、拖曳課程和改後台設定。修改會加密後直接存回 GitHub。
- 存檔後大約 **1 分鐘**，其他人重新整理就會看到；開著網頁的人每 2 分鐘也會自動檢查一次。

## 一、建立 repository 並開啟 GitHub Pages

1. 在 GitHub 新增一個 **Public** repository，例如 `kaixuan-schedule`（免費帳號的 Pages 只能用 Public；資料有加密，別人看不懂）。
2. 上傳這個資料夾裡的 `index.html` 和 `README.md`（`data.enc.json` 會在第三步自動產生）。
3. Settings > Pages > Source 選 `Deploy from a branch`，Branch 選 `main`、資料夾 `/ (root)`，按 Save。
4. 約一分鐘後會出現網址：`https://你的帳號.github.io/kaixuan-schedule/`。

> 網頁會從網址自動判斷是哪個帳號、哪個 repository。如果用自訂網域，請在 `index.html` 裡找到 `window.SCHEDULE_CONFIG`，填上 `owner`（帳號）和 `repo`（repository 名稱）。

## 二、申請 GitHub 金鑰（管理用）

1. GitHub 右上頭像 > Settings > Developer settings > Personal access tokens > **Fine-grained tokens** > Generate new token。
2. Token name 隨便取，例如「課表管理」；Expiration 最長可選 1 年，到期再重新申請。
3. Repository access 選 **Only select repositories**，只勾 `kaixuan-schedule`。
4. Permissions > Repository permissions > **Contents** 選 **Read and write**，其他不用動。
5. 按 Generate，複製那串 `github_pat_...`。這就是你的「管理密碼」，不要給別人。

## 三、第一次使用

1. 打開網頁，會看到「第一次使用」畫面。
2. 貼上 GitHub 金鑰，設定一組**檢視密碼**（給老師用，至少 8 碼，建議 10 碼以上、不要用生日或學校名稱），按「建立」。
3. 右上齒輪 > 資料備份，把 `kaixuan-schedule-initial-data.json` 的內容整個貼上，按「匯入」。
4. 看到「已存到 GitHub」就完成了。把網址和檢視密碼告訴老師即可。
5. `kaixuan-schedule-initial-data.json` 不要上傳到 GitHub；匯入後可以刪掉，或收在私人的地方。

## 四、平常使用

- **老師**：輸入檢視密碼；勾「這台裝置記住密碼」下次就不用再打。
- **你要修改**：先進入課表，再按「管理登入」貼金鑰；勾「這台裝置記住金鑰」，下次會自動進入管理模式。只在自己的電腦勾。
- **新增課程**：滑鼠移到空格按「＋」。**修改或刪除**：點課程卡片。**換時段**：拖曳，或在卡片裡改星期、節次。
- **後台**：右上齒輪，可以改學生（資優組／支持組）、老師、課程時間、外觀（鴨鴨池塘／森林光影）、資料備份、密碼。
- **換檢視密碼**：齒輪 > 密碼。換完要告訴老師新的密碼。
- **鎖定**：按右上「鎖定」會清掉這台裝置記住的檢視密碼和金鑰，公用電腦用完記得按。
- **金鑰外流或電腦遺失**：到 GitHub 的 Fine-grained tokens 把那把金鑰刪掉，再申請一把新的。

## 安全性說明

- 加密方式：AES-256-GCM，密碼經 PBKDF2-SHA256 運算 31 萬次後才變成金鑰。
- 加密檔放在公開的 GitHub 上，有心人可以下載回去慢慢猜密碼，所以**檢視密碼越長越安全**。
- 只有持有 GitHub 金鑰的人才能改課表；知道檢視密碼的人只能看。
- 忘記檢視密碼的話資料解不開。請用齒輪 > 資料備份 > 複製資料，定期在自己的電腦留一份備份。
