# Leave Me Alone: 可套用的程式碼模式

每一節對應 SKILL.md 的防禦指令編號。片段以純 HTML / CSS / JS 為主，框架版本依同樣邏輯改寫。

## 1. Consent 閘門（CIPA）

追蹤腳本永遠不寫在 `<head>` 或 HTML 裡。只有 consent 回呼才注入。

```html
<!-- Cookie banner：拒絕與同意同樣醒目，無預設勾選 -->
<div role="dialog" aria-labelledby="cc-title" aria-describedby="cc-desc" class="cc">
  <h2 id="cc-title">Cookie 與分析工具</h2>
  <p id="cc-desc">同意後我們才會載入分析與行銷腳本。拒絕不影響使用。</p>
  <button type="button" class="cc-btn" data-consent="reject">拒絕</button>
  <button type="button" class="cc-btn" data-consent="accept">同意</button>
</div>
```

```css
/* 兩顆按鈕共用同一個 class：相同尺寸、對比、焦點樣式 */
.cc-btn { min-height: 44px; padding: 0 20px; font: inherit; }
.cc-btn:focus-visible { outline: 3px solid currentColor; outline-offset: 2px; }
```

```js
// 讀取先前的選擇；沒有紀錄就等於拒絕
const CONSENT_KEY = "consent.analytics";

function loadTrackers() {
  // 只在這裡注入。每個追蹤器都走這個函式，不得有例外。
  const s = document.createElement("script");
  s.async = true;
  s.src = "https://www.googletagmanager.com/gtag/js?id=G-XXXXXXX";
  document.head.appendChild(s);
}

document.querySelectorAll("[data-consent]").forEach((btn) => {
  btn.addEventListener("click", () => {
    const accepted = btn.dataset.consent === "accept";
    try { localStorage.setItem(CONSENT_KEY, accepted ? "1" : "0"); } catch {}
    btn.closest(".cc").remove();
    if (accepted) loadTrackers();
  });
});

let prior = null;
try { prior = localStorage.getItem(CONSENT_KEY); } catch {}
if (prior === "1") loadTrackers();
```

Session replay（Hotjar、FullStory）、Meta Pixel、TikTok Pixel、Intercom / Crisp 等聊天外掛，全部放進 `loadTrackers()`。

## 2. 字型與圖片（Asset Protection）

```css
/* 預設：系統字體棧，零授權問題 */
:root {
  --font-sans: system-ui, -apple-system, "Segoe UI", Roboto, "Noto Sans TC", sans-serif;
  --font-mono: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace;
}

/* 需要品牌字型時：OFL 字體自託管，檔案放 repo 並附 OFL.txt */
@font-face {
  font-family: "Inter";
  src: url("/fonts/Inter-Variable.woff2") format("woff2");
  font-weight: 100 900;
  font-display: swap;
}
```

```html
<!-- 佔位圖：CSS 漸層或 inline SVG，不抓外站圖片 -->
<div class="placeholder" role="img" aria-label="產品示意圖"></div>
<style>.placeholder { aspect-ratio: 16/9; background: linear-gradient(135deg, #dbeafe, #c7d2fe); }</style>

<!-- 使用真實圖片時，註解寫明來源與授權 -->
<!-- Source: Unsplash, photo by <author>, Unsplash License -->
<img src="/img/hero.jpg" alt="團隊在白板前討論產品路線圖" width="1200" height="675">
```

套件授權快速檢查（node 專案）：

```bash
npx -y license-checker-rseidelsohn --summary --excludePrivatePackages
```

輸出中出現 `GPL` 或 `AGPL` 即阻斷，改用相容授權的替代品。

## 3. 無障礙（ADA / WCAG 2.1 AA）

```css
/* 不移除 outline；若自訂外觀，一定補 :focus-visible */
:focus-visible { outline: 3px solid #1d4ed8; outline-offset: 2px; }
button:focus:not(:focus-visible) { outline: none; } /* 滑鼠點擊時才隱藏 */
```

```html
<!-- 裝飾性圖示 -->
<svg aria-hidden="true" focusable="false">...</svg>
<img src="/img/divider.svg" alt="">

<!-- 頁尾無障礙聲明：固定注入 -->
<footer>
  <a href="/accessibility">無障礙聲明</a>
  <span>遇到使用障礙請來信 <a href="mailto:accessibility@example.com">accessibility@example.com</a></span>
</footer>
```

`/accessibility` 頁面最少內容：目標標準（WCAG 2.1 AA）、已知限制、反饋信箱、回覆時限（例如 5 個工作天）。

對比度：常規文字 >= 4.5:1，大型文字 >= 3:1，UI 元件邊框與圖示 >= 3:1。驗證工具已在這台機器的 ux-ui-agent-skills 內（`validate_contrast` gate）。

## 4. Clickwrap 與條款（Contract Shield）

```html
<form id="signup">
  <!-- 其他欄位 -->
  <label class="clickwrap">
    <input type="checkbox" id="agree" name="agree" required>
    <!-- 未預先勾選，required 阻止原生送出 -->
    我已閱讀並同意
    <a href="/terms" target="_blank" rel="noopener">服務條款</a> 與
    <a href="/privacy" target="_blank" rel="noopener">隱私權政策</a>
  </label>
  <button type="submit" id="submit" disabled>建立帳號</button>
</form>
<script>
  const agree = document.getElementById("agree");
  const submit = document.getElementById("submit");
  agree.addEventListener("change", () => { submit.disabled = !agree.checked; });
</script>
```

後端同時記錄同意證據：使用者 ID、條款版本、時間戳、IP。沒有這筆紀錄，Clickwrap 在法庭上等於沒做。

服務條款必備三條（放在 `/terms`，標題需醒目）：

1. **Binding Individual Arbitration & Class Action Waiver**：爭議以個別仲裁解決，放棄集體訴訟與陪審團審判。附 30 天退出（opt-out）機制可強化可執行性。
2. **Limitation of Liability**：總賠償上限為過去 12 個月支付金額或 USD 100，取其高者；排除間接、附帶、懲罰性損害。
3. **AI Output AS-IS**：AI 生成內容按現狀提供，不保證正確性、完整性或適用性；使用者自行查證並承擔使用結果。

## 5. 訂閱（FTC Click-to-Cancel）

```html
<!-- 付款按鈕旁的費率揭露：與按鈕同一視線，字級不小於按鈕文字 -->
<p class="billing-notice">
  <strong>每月 USD 29</strong>，試用期於 2026-11-11 結束後自動扣款，可隨時在「帳戶設定」一鍵取消。
</p>
<button type="submit">開始 14 天免費試用</button>
```

```html
<!-- 帳戶設定頁：取消與訂閱同樣容易 -->
<section aria-labelledby="sub-title">
  <h2 id="sub-title">訂閱方案</h2>
  <p>Pro 月繳，下次扣款 2026-11-11</p>
  <form method="post" action="/api/subscription/cancel">
    <button type="submit">取消訂閱</button>
  </form>
  <!-- 最多一步確認（例如「確定取消？」），不得導向客服、問卷或電話 -->
</section>
```

取消後立即寄送確認信，並在頁面顯示生效日期。

## 6. AI 揭露（EU AI Act Art. 50）

```html
<!-- 聊天或生成介面的初次進入點 -->
<div role="note" class="ai-notice">
  本工具由 AI 驅動，回覆可能有誤，請自行查證重要資訊。
</div>
```

AI 生成的圖片、音訊、影片：輸出檔寫入 metadata（例如 C2PA 或 IPTC `DigitalSourceType=trainedAlgorithmicMedia`），介面上標示「AI 生成」。

## 7. 審查模式偵測規則（Grep）

| 指令 | 搜尋 pattern | 判讀 |
|------|-------------|------|
| 2 | `googletagmanager\|gtag\(\|fbq\(\|ttq\.\|hotjar\|fullstory\|clarity\.ms\|intercom\|crisp\.chat` | 出現在 HTML 或非 consent 回呼的 JS 內即違規 |
| 1 | `\.(ttf\|otf\|woff2?)` 搭配非 OFL 來源、`fonts\.adobe\|typekit\|myfonts` | 商業字型，阻斷 |
| 1 | `https?://[^"']+\.(jpe?g\|png\|webp\|gif)` 指向外站 | 外部圖片熱連結，要求換成本地並註明授權 |
| 1 | `"license":\s*"(A?GPL` 於 node_modules 或 lockfile | 傳染性授權，阻斷 |
| 3 | `outline:\s*(none\|0)` | 檢查同檔是否有 `:focus-visible` 替代 |
| 3 | `<img(?![^>]*alt=)` | 缺 alt |
| 4 | `checked` 出現在同意 checkbox、「使用即代表同意」「by using」 | 預先勾選 / Browsewrap |
| 5 | `cancel` 相關路由導向 `tel:`、`mailto:` 或多頁 survey | 取消流程不對等 |
| 5 | `保證.*(收益\|獲利)\|100%\s*取代\|guaranteed (revenue\|income)\|replaces? (your )?(doctor\|lawyer\|expert)` | 誇大 AI 文案 |
| 6 | 有 `/chat`、`completions`、`generate` 端點但無「AI 驅動」「AI-powered」字樣 | 缺 AI 揭露 |
