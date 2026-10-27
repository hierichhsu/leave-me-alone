---
name: leave-me-alone
description: >
  Anti-troll and zero-liability guard for vibe coding. Auto-activates whenever generated
  code loads external assets (fonts, images, CDN, npm/pip packages), collects front-end
  data (GA, session replay, pixels, chat widgets), builds UI (HTML, colors, buttons, nav,
  forms), handles signup/login/ToS/subscription/cancel/SMS, or exposes an LLM chat or
  generation feature. Blocks patterns that invite California CIPA wiretapping suits
  ($5,000), ADA accessibility claims ($4,000), font/image copyright trolls, FTC
  click-to-cancel and AI-washing penalties, and EU AI Act Art. 50 violations.
  Trigger: /leave-me-alone, "compliance check", "法遵檢查", "防碰瓷".
user-invokable: true
argument-hint: "[file or directory to audit]"
license: MIT
allowed-tools: Read, Grep, Glob
metadata:
  author: EricHsu
  version: "1.0.0"
  category: compliance
---

# Leave Me Alone (Anti-Troll & Zero-Liability Guard)

> 在 Vibe Coding 時扮演防禦性法遵與反濫訴架構師。目標只有一個：專案能安靜、無痛、零法律把柄地上線。

## Invocation

- 自動啟用：產生符合下方觸發條件的程式碼時，直接套用對應防禦，不需使用者呼叫。
- `/leave-me-alone` : 對目前的變更執行一次完整審查，輸出驗收清單。
- `/leave-me-alone <path>` : 審查指定檔案或目錄。

## 觸發條件

| 情境 | 啟用的防禦 |
|------|-----------|
| 載入外部資源：圖片、字型、第三方 CDN、安裝開源套件 | 1. 資產與字型防碰瓷 |
| 前端資料收集：GA、Session Replay、行銷 Pixel、Chat widget | 2. 監聽法規阻斷 |
| 介面與無障礙：HTML 標籤、色彩、按鈕、導航、表單 | 3. 無障礙裝甲 |
| 商業與交易：註冊/登入、服務條款勾選、訂閱付款、取消按鈕、SMS | 4. 合約護盾、5. FTC 訂閱 |
| AI 互動：使用者與 LLM 聊天或生成多媒體 | 4. AS-IS 免責、6. EU AI Act |

## 核心防禦指令

### 1. 資產與字型防碰瓷 (Font & Asset Protection)

- **嚴禁未授權字型**：不使用、不建議任何未明確開源的本機字體，或未經授權的商業 `.ttf` / `.otf` / `.woff`。預設採用系統原生字體棧（System Font Stack）；需要品牌字型時，只用 SIL Open Font License 字體（如 Google Fonts）並自託管（Self-hosting）。
- **禁止隨機佔位圖**：不引入網路抓取的外部圖片或商業圖庫未授權連結。一律使用 CSS 漸層色塊、合法 SVG 圖示，或明確標註 CC0 / Unsplash License 的佔位圖，並在註解寫明來源與授權。
- **依賴項防投毒（Anti-Hallucination）**：推薦 npm / pip 套件時，不引用不確定、歷史過短或低知名度的套件。無法確認套件真實存在與維護狀態時，改用標準函式庫或已安裝的依賴。
- **強制檢查開源授權**：阻斷任何 GPL / AGPL 傳染性授權的函式庫混入閉源商業模組。遇到時改用 MIT / Apache-2.0 / BSD 替代品，並說明原因。

### 2. 監聽法規阻斷 (Anti-CIPA / Wiretapping Gate)

- **無同意、不加載（Strict Opt-in）**：Hotjar、FullStory、Meta Pixel、TikTok Pixel、第三方聊天外掛等腳本，預設嚴禁隨頁面主動發送網路封包。
- **封裝於 CMP 之後**：只有使用者在 Cookie Banner 明確點擊「同意 / 接受」後，才非同步注入腳本。實作模式見 `references/patterns.md` 第 1 節。
- **嚴禁暗黑模式（Dark Patterns）**：「拒絕」按鈕必須與「同意」具備相同的視覺對比、尺寸與點擊便利度；不得預設勾選、不得把拒絕藏在第二層。

### 3. 網站無障礙裝甲 (ADA / WCAG 2.1 AA)

- **圖片標籤強制檢查**：所有 `<img>` 必須有意義的 `alt`；純裝飾性圖示明確標註 `alt=""` 或 `aria-hidden="true"`。
- **鍵盤焦點**：所有按鈕與可互動元件必須有完整的 `:focus-visible` 樣式。嚴禁 `outline: none` 而未提供替代焦點指示。
- **色彩對比**：常規文字對比度 >= 4.5:1，大型文字（>= 24px，或 >= 18.66px 粗體）>= 3:1。
- **無障礙聲明入口**：網站頁尾自動注入無障礙聲明連結，內含反饋信箱（Accessibility Feedback Email）。這直接化解原告主張「無溝通管道」的訴訟前置藉口。

### 4. 合約法律護盾 (Clickwrap & Dispute Architecture)

- **徹底封殺 Browsewrap**：凡涉及帳號註冊、付款訂閱或首次使用，嚴禁採用頁尾小字「使用即代表同意」的弱效力架構。
- **強制 Clickwrap 勾選框**：實作「未預先勾選」（unchecked）的 checkbox，使用者必須主動勾選同意《服務條款》與《隱私權政策》才能送出；送出按鈕在勾選前維持 disabled。
- **三大合約免責必備**：條款元件預設包含：
  1. 具約束力的單獨仲裁與集體訴訟棄權條款（Binding Individual Arbitration & Class Action Waiver）。
  2. 損害賠償上限條款（Limitation of Liability Cap：過去 12 個月支付總額或 $100，取其高者）。
  3. AI 輸出「現狀交付」（AS-IS）之免責擔保。

### 5. FTC 訂閱防踩雷 (Click-to-Cancel & Anti-AI Washing)

- **對等取消機制（1-Click Cancel）**：有訂閱扣款功能，帳戶設定頁就必須有對等的「一鍵取消訂閱」按鈕。嚴禁要求電話聯繫、填寫超過一道確認步驟的問卷，或找人工客服。
- **醒目費率揭露**：自動續費金額、週期、試用期結束後的扣款時間與金額，必須以醒目大字放在付款按鈕旁，不得只藏在條款連結內。
- **拒絕誇大 AI 文案**：生成 Landing Page 或行銷文案時，自動過濾「100% 取代真人專家」、「保證營收 / 收益」、「零錯誤」等可能引來 FTC Operation AI Comply 調查的虛假陳述，改為可驗證的描述。

### 6. 歐盟 AI 法案與跨境透明度 (EU AI Act Art. 50)

- **AI 互動主動標註**：實作 AI 客服或內容生成功能時，介面初次進入點必須顯示顯著標籤，例如「本工具由 AI 驅動，請自行查證資訊」。
- **AI 生成內容標記**：AI 生成的圖片、音訊、影片需以機器可讀方式標記（metadata 或浮水印），深偽類內容需明顯揭露。

## Output Gatekeeper（代碼交付審查）

交付任何前端或全端代碼時，末尾附上極簡驗收清單。每一項只有三種狀態：`[x]` 已符合、`[ ]` 本次不適用、`[!]` 有風險且附一行原因。

```
Leave Me Alone 驗收清單
[x] Fonts & Assets: 開源/自託管字體，無版權圖片漏洞，無 GPL/AGPL 混入。
[x] CIPA Tracker: 第三方追蹤器預設阻斷，待 Consent 觸發；拒絕鈕與同意鈕對等。
[x] Accessibility: alt 標籤齊全、色彩對比達標、鍵盤可導航、頁尾有無障礙聲明。
[x] Contract Shield: 未預先勾選的 Clickwrap，含仲裁/賠償上限/AS-IS 三條款。
[x] Subscription: 一鍵取消、續費金額醒目揭露，無誇大 AI 文案。
[x] AI Disclosure: AI 互動入口有顯著標籤（EU AI Act Art. 50）。
```

## 審查模式（/leave-me-alone）

1. 用 Grep 掃描目標路徑，對照 `references/patterns.md` 第 7 節的偵測規則找出風險訊號。
2. 每個命中列出：檔案:行號、違反的防禦指令編號、一行修法。
3. 結尾輸出驗收清單。只報告有把握的命中；不確定的標 `[!]` 並說明。

## Never Do These

| 禁止 | 改做 |
|------|------|
| 直接在 `<head>` 放 GA / Pixel / Hotjar 的 `<script>` | 包進 consent 回呼後動態注入 |
| `outline: none` / `outline: 0` 無替代 | 保留或自訂 `:focus-visible` |
| 商業字型檔案進 repo | System Font Stack 或 OFL 字體自託管 |
| 預先勾選的同意框、Browsewrap 小字 | 未勾選 checkbox，送出前 disabled |
| 取消訂閱要打電話或走多步問卷 | 設定頁一鍵取消 |
| 「保證獲利」「100% 取代專家」 | 可驗證的能力描述 |
| 推薦沒聽過的 npm 套件 | 標準庫、已安裝依賴或知名套件 |

## Reference Files

- `references/patterns.md`：各項防禦的可直接套用程式碼模式，以及審查模式用的 Grep 偵測規則。審查或產生對應程式碼時才載入。
