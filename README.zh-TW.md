# agent-tool-map

一份有來源的精簡指南，協助人與 AI agent 找到適合工作的小工具。附帶零相依套件的 CLI，可列出、篩選與比較工具。

**Node.js 22 以上 · 無需 API key · 無需 LLM**

[English](README.md) · [貢獻指南](CONTRIBUTING.md) · [MIT 授權](LICENSE)

目錄審閱日期：**2026-10-04**。每筆來源的查核日期記錄於 `data/tools.json`。

## 開始使用

在專案目錄執行。無需安裝相依套件。

```sh
git clone https://github.com/bm1016bm-svg/agent-tool-map
cd agent-tool-map
node bin/agent-tool-map.mjs list
node bin/agent-tool-map.mjs list --category office-editing
node bin/agent-tool-map.mjs list --interface cli --json
node bin/agent-tool-map.mjs show ask-json
node bin/agent-tool-map.mjs compare ccphoto bifrost
```

## 工具指南

依工作需求閱讀用途與限制；來源連結供你核對原作者的文件。CLI 的 `--json` 輸出可供腳本或 agent 使用。

| 工具、分類與介面 | 用途 | 適合情境 | 限制 | 來源 |
| --- | --- | --- | --- | --- |
| [AskJSON](https://github.com/reuters-graphics/ask-json)<br>`human-input`, `structured-data`<br>`cli`, `library` | 把 JSON Schema 轉成終端互動提問，補齊缺漏或無效欄位，並驗證結果。 | 在腳本或發布流程使用設定前，向人取得必要資料。 | 需要人的互動輸入；只支援部分 schema 關鍵字及同類型陣列，缺少的非必填欄位不會觸發提問。 | [1](https://github.com/reuters-graphics/ask-json) |
| [Bifrost](https://github.com/axiom0x0/bifrost)<br>`file-transfer`, `human-input`<br>`cli`, `web` | 啟動本機 HTTP 傳檔頁並列印手機可掃的 QR，提供傳送、接收及選用加密模式。 | 不用安裝手機 App，在終端與附近手機間傳檔。 | 裝置需要可互通的區域網路；文件中的接收流程是一般上傳，並非綁定 AI 任務的拍照回呼。 | [1](https://github.com/axiom0x0/bifrost) |
| [CCPhoto](https://github.com/stepankaiser/ccphoto)<br>`photo-input`, `human-input`<br>`cli`, `mcp`, `web` | 把手機瀏覽器相機接到本機伺服器，可透過 MCP 交回照片，或以獨立 QR CLI 存到本機。 | 用附近手機把實體標籤、白板或零件提供給助手。 | 手機必須能連到區網伺服器；獨立模式持續收多張照片，直播另有 HTTPS 與瀏覽器限制。 | [1](https://github.com/stepankaiser/ccphoto) |
| [Ciacola](https://github.com/joshrotenberg/ciacola)<br>`agent-runtime`, `budget-control`<br>`mcp`, `web` | 以 SQLite 帳本維持 Claude 與 Codex 對話，提供重啟恢復、限定權限的 MCP 及依供應商設定的限制。 | 需要持久對話、恢復及權限範圍的本機代理操作。 | 需要從原始碼建置及已登入的供應商 CLI；接納門檻不是預留額度，回應邊界檢查也可能超額。 | [1](https://github.com/joshrotenberg/ciacola) |
| [codex-wrapper](https://github.com/joshrotenberg/codex-wrapper)<br>`cli-integration`, `budget-control`<br>`library` | 為已安裝的 Codex CLI 提供 Rust 命令建構及結構化執行整合，包含原生 rollout 預算。 | 在 Rust 程式中執行 Codex CLI，並設定每次執行的限制。 | 需要 Codex CLI；原生預算單位依 CLI／供應商版本而異，且在回應後檢查，並非精確總 token 或美元上限。 | [1](https://github.com/joshrotenberg/codex-wrapper#native-rollout-budgets) |
| [docx-cli](https://github.com/kklimuk/docx-cli)<br>`office-editing`, `document-review`<br>`cli` | 透過定位符讀寫 DOCX，以直接修改 XML 的方式加入註解及 Word 原生追蹤修訂。 | 填寫 Word 範本，或產生可在 Word 接受／拒絕的修訂稿。 | 預設編輯會覆寫檔案且沒有內建復原；渲染需要 Word 或 LibreOffice，本指南未重跑作者的保真度基準。 | [1](https://github.com/kklimuk/docx-cli) |
| [Evidrift](https://github.com/bm1016bm-svg/evidrift)<br>`dependency-drift`, `json-reproduction`<br>`cli`, `mcp` | 記錄並重查指定 TypeScript 或本機 JSON 契約，也能把 loopback HTTP JSON 失敗縮成可重播案例。 | 檢查升級後某個相依套件假設是否仍成立，或縮小本機 JSON 失敗輸入。 | 契約檢查限已安裝的 TypeScript 簽章與 repo 內 JSON 值；不支援遠端重播、YAML 或遠端參照，也不證明自由文字宣稱。 | [1](https://github.com/bm1016bm-svg/evidrift) |
| [excel-git / xlgit](https://github.com/MichaelFowler1/excel-git)<br>`office-editing`, `version-control`<br>`cli` | 透過修改活頁簿 XML 部件，為 Git 加入儲存格層級的差異檢視及三方合併。 | 檢視及合併不同 Git 分支上各自修改的 XLSX／XLSM。 | 衝突的儲存格需要人工處理，公式在 Excel 開啟時重算；保留物件與模糊測試結果來自作者報告，非本指南實測。 | [1](https://github.com/MichaelFowler1/excel-git) |
| [Grape](https://github.com/gael55x/Grape)<br>`repository-context`, `context-transport`<br>`cli`, `mcp` | 產生 repo 上下文包，提供依工作階段計算的差異、來源／摘錄雜湊、還原 handle 及過期上下文標記。 | 需要追蹤每輪上下文變化，避免反覆重送全部 repo 資料的任務。 | 代理必須呼叫其 CLI 或 MCP；它不證明答案正確，TypeScript／JavaScript 的索引較其他 fallback 語言完整。 | [1](https://github.com/gael55x/Grape) |
| [LoopGain](https://github.com/loopgain-ai/loopgain)<br>`loop-control`, `convergence`<br>`library` | 利用應用提供的數值誤差軌跡判斷收斂，並依目標、停滯、振盪或迭代上限停止。 | 具備明確數值誤差及目前最佳輸出的迭代代理或求解流程。 | 誤差訊號由呼叫方定義；趨勢分類不能證明任務正確，節省成本的基準來自作者報告，本指南未重跑。 | [1](https://github.com/loopgain-ai/loopgain) · [2](https://github.com/loopgain-ai/loopgain/blob/main/loopgain/core.py) |
| [LoopLens MCP](https://github.com/musaceylan/looplens-mcp)<br>`loop-control`, `debugging`<br>`mcp` | 收集代理工具事件，呈現重複模式、錯誤特徵及收斂代理指標。 | 在已接 MCP 的流程中檢視除錯工作階段及反覆工具呼叫。 | 需要事件輸入及 MCP 整合；工具多樣性、輸入／輸出大小等啟發式指標不能證明驗收條件通過。 | [1](https://github.com/musaceylan/looplens-mcp) · [2](https://github.com/musaceylan/looplens-mcp/blob/master/src/looplens/convergence/metrics.py) |
| [Pueue](https://github.com/Nukesor/pueue)<br>`task-queue`, `process-control`<br>`cli`, `library`, `daemon` | 由背景 daemon 排隊執行 shell 命令，提供群組、並行上限、任務狀態、日誌與等待控制。 | 讓助手排入耗時的本機命令，再查詢或等待結果。 | 需要 pueued 及正確 shell 引號；定位為單一使用者的簡單佇列，不是分散式、多使用者或進階負載平衡系統。 | [1](https://github.com/Nukesor/pueue) |
| [PyMuPDF4LLM](https://github.com/pymupdf/pymupdf4llm)<br>`pdf-extraction`, `document-data`<br>`library` | 把 PDF 擷取為 Markdown、JSON 或純文字，提供版面資訊、分頁 chunk 及選用 OCR。 | 在本機將 PDF 整理成助手或檢索流程可用的資料。 | OCR 需要已安裝的引擎；Office 文件支援使用 PyMuPDF Pro，文件列出 AGPL 或商業授權。本指南未測擷取品質。 | [1](https://github.com/pymupdf/pymupdf4llm) |
| [Repo Context Center](https://github.com/cyberturk01/repo-context-center)<br>`repository-context`, `change-impact`<br>`cli` | 建立 repo 地圖及任務路線，再依 repo 訊號建議受影響檔案、測試及驗證命令。 | 協助能執行 shell 的程式助手定位可能相關的檔案及規劃驗證。 | 影響分析是啟發式，並非完整相依引擎；verify 只列出建議，不執行測試或證明正確。 | [1](https://github.com/cyberturk01/repo-context-center) |
| [xlq](https://github.com/wms2537/aix)<br>`office-editing`, `structured-data`<br>`cli` | 提供 JSON 試算表檢視及具型別的儲存格 patch；作者描述其 OOXML 局部寫入會保留其他部件。 | 透過原生 CLI 套用明確的試算表局部變更，並取得支援範圍及拒絕原因。 | 公式支援不可靠時可能拒絕操作；部件保留及計算覆蓋率來自維護者報告，本指南未重測。 | [1](https://github.com/wms2537/aix) |

### 可用篩選值

分類：`agent-runtime`, `budget-control`, `change-impact`, `cli-integration`, `context-transport`, `convergence`, `debugging`, `dependency-drift`, `document-data`, `document-review`, `file-transfer`, `human-input`, `json-reproduction`, `loop-control`, `office-editing`, `pdf-extraction`, `photo-input`, `process-control`, `repository-context`, `structured-data`, `task-queue`, `version-control`.

介面：`cli`, `daemon`, `library`, `mcp`, `web`.

## CLI 指令

| 指令 | 用途 |
| --- | --- |
| `list [--category X] [--interface X] [--json]` | 列出工具，或同時依分類與介面篩選。 |
| `show ID [--json]` | 查看完整條目、用途、限制與來源。 |
| `compare ID ID [--json]` | 比較兩個工具的適合情境與取捨。 |
| `validate [--json]` | 離線檢查目錄資料的結構與一致性。 |
| `check-links [--json]` | 連線檢查目錄中的 URL 是否可存取。 |

每個指令都加在 `node bin/agent-tool-map.mjs` 後面。只有 `check-links` 需要網路；可存取的 URL 只代表連結狀態，工具功能仍需在你的工作流程中確認。

結束碼：`0` 成功；`1` 目錄不合法或找到失效連結（HTTP 404／410）；`2` 使用方式、讀取失敗或連結無法判定（例如 HTTP 403／429、網路錯誤、逾時）。失效與無法判定同時出現時，回傳 `1`。

## 審閱方式

條目記錄查核日期當時的一手文件，重點是介面、適合情境與明確限制。這份指南沒有安裝或執行第三方工具的驗證報告，也不以排序或星數判定品質。審閱日期不能保證未來功能或相容性；使用前請核對目前的來源與需求。

## 維護與檢查

`data/tools.json` 是工具條目的資料來源；這兩份 README 由 `scripts/build-readme.mjs` 產生。

```sh
npm test
npm run validate
npm run docs
npm run docs:check
```

CI 在 Windows 與 Linux 的 Node.js 22、24 執行測試、資料驗證及文件一致性檢查。線上連結檢查可由維護者另行執行。

## 授權

MIT © 2026 bm1016bm-svg。詳見 [LICENSE](LICENSE)。
