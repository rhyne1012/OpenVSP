# OpenVSP 官方正式版本同步

本 fork 的 **`official-release` 分支**追蹤 `OpenVSP/OpenVSP` 的最新正式版本原始碼。
`main` 保留既有原始碼，並存放這份說明、自動同步工作流程與維護紀錄。
開始自己的開發工作時，請從 `official-release` 建立另外的分支。

## 排程與手動執行

- GitHub Actions 工作流程：**Sync official OpenVSP release**。
- 每天 **Asia/Taipei 09:17（UTC 01:17）**檢查；GitHub 繁忙時可能延遲。
- 在 Actions 選擇此工作流程，按 **Run workflow** 可立即執行。
- 勾選 **Inspect without changing branches or tags** 可先做唯讀檢查。
- 使用 GitHub 自動提供、限本儲存庫的 `GITHUB_TOKEN`；不需要個人存取權杖。

## 同步規則

1. 讀取所有分頁的官方 tags，只接受 `OpenVSP_數字.數字.數字`。
2. 按數字版本選最高版本，排除測試、候選版及平台專用標籤。
3. 複製當次最新版本的原始 tag，保留 annotated tag 的物件；先前同步的 tags 會保留。
   初次設定只匯入目前最新版，不回填全部歷史 tags，也不逐一收錄兩次檢查間被取代的版本。
4. 將 `official-release` 建立或快轉至該版本的精確 commit，驗證分支與 tag。
5. 不自動合併自己的開發分支。若 `official-release` 已分歧、同名 tag 不一致或版本倒退，
   工作流程會失敗停止，不強制覆蓋；可在 Actions 查看失敗原因。

同步的是 Git 原始碼與版本標籤；不建立 GitHub Release、複製下載安裝包或編譯軟體。
原有 `build.yml` 保留，其 `build` 分支觸發條件未變。

## 防止長期閒置停用

GitHub 可能在公開儲存庫連續 60 天沒有活動時停用排程。因此工作流程在成功同步新版後，
或距離上次紀錄滿 30 天時，會只更新 `main` 的 `.github/openvsp-sync-status.json`。
這筆小型維護提交記錄官方版本、commit 與檢查時間，提供儲存庫活動及稽核紀錄。
無新版且未滿 30 天時不建立提交。

持續 API 錯誤、權限變更或 Actions 停用仍需人工處理；維護提交不是外部監控服務。
如已被停用，請在 Actions 選此工作流程並 **Enable workflow**，再手動執行一次。
執行通知依你的 GitHub Actions 通知偏好設定。

## 暫停及復原

- 暫停：Actions → 此工作流程 → 選單 → **Disable workflow**。
- 要使用舊版：切換到已保存的版本 tag，或从该 tag 建立自己的分支。
- 不要直接修改 `official-release`；它專門用於官方版本快轉。
- 不要把 `official-release` 設為預設分支；排程必須留在含工作流程的 `main`。
- 設定前 `main` 基準為 `73d3b99341e035d7d3316e20105e98dcbe1c6033`（OpenVSP 3.43.0），歷史保留。

參考：[官方 tags](https://github.com/OpenVSP/OpenVSP/tags)、
[GitHub 排程行為](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#schedule)。
