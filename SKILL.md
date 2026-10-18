---
name: figure-create
description: 動漫／角色手辦（figure）從概念圖做到可彩色 3D 列印模型的流程：概念圖 → 圖轉 3D 服務（例如 Tripo）→ 上下半身與配件分開生成 → 各零件補成封閉實體 → 精確布林接合 → 交付 Blender (.blend) 與 3MF。涵蓋主對話怎麼跟使用者溝通、派 agent、呈現成果，以及 agent 怎麼生成與修改模型。Use for anime/character figure work: concept image → image-to-3D service → split generation → closed, printable, colour mesh → Blender delivery with unmerged parts and live booleans.
---

# 手辦製作 Skill（figure-create）

把一張角色概念圖做成「封閉、單一整體、帶顏色、可 3D 列印」的模型，並以 Blender 檔交付。這份 skill 分兩塊：

- **(A) 主對話（協調者）**：怎麼跟使用者溝通、怎麼派工給 agent、怎麼呈現成果 → `references/coordinator.md`
- **(B) agent（執行者）**：怎麼生成與修改模型 → `references/pipeline.md`、`references/verification.md`

使用者個人的偏好、服務帳號、檔案位置**不寫在這裡**，放在 `profiles/<使用者>/`（見下方「接手工作」）。

## 核心規則

1. **主對話只協調**：重活（生成、修模型、做頁面、上傳）都派 agent 在背景做；主對話用白話跟使用者說話。
2. **先覆述再動手**：用白話把使用者的要求講一遍，確認理解後才派工；照使用者描述的方法做，不偷換成捷徑。
3. **一張圖、一個問題、一個 agent**：每個任務 30–60 分鐘；超過 30 分鐘回報一次；到時間上限就停下回報。
4. **修、驗、發布分開**：修的 agent 不能自己宣布過關；主對話必須**自己看過截圖**才給使用者。
5. **給使用者看原始品質**：不減面、不縮貼圖、不重建網格、不重烘貼圖。
6. **封閉之後絕不刪面**：只用精確布林（exact boolean）改形狀；刪面就是開洞。
7. **誠實回報**：瑕疵、做錯、超時都直說，附前後對照圖。
8. **不碰祕密**：金鑰、登入檔、Cookie 不進 repo、不進任何上傳檔。

## 流程一覽（細節見 `references/pipeline.md`）

0. 挑圖（使用者挑；使用者說「全部」就不用挑）
1. 基準模型：原圖直接生成，只當參照，**不能當方向標準**
2. 2D 準備：去配件正面圖、雙參考重畫背面、配件（例如尾巴）單獨一張圖
3. 分開生成：站姿 → 上半身＋下半身＋配件；坐／蹲／躺 → 整體＋配件
4. 各零件補成封閉實體（保留 >99.9% 原始面）
5. 粗略對齊方向（只做剛體旋轉；最終角度交給使用者在 Blender 調）
6. 精確布林接合：（上半身 − 切除體）∪ 下半身 ∪ 配件
7. 交付：`.blend`（零件分開、布林修改器不套用）＋ 3MF 彩色列印檔

## 文件導覽

| 檔案 | 誰讀 | 內容 |
|---|---|---|
| `references/coordinator.md` | 主對話 | 溝通、派工、驗收把關、呈現成果、中斷接手、儲存整理 |
| `references/agent-prompt-template.md` | 主對話 | 派工單範本（複製後填空） |
| `references/pipeline.md` | agent | 每一步的輸入 → 工具／做法 → 輸出 → 驗收 |
| `references/verification.md` | agent、主對話 | 驗收清單（目視＋數字） |
| `references/pitfalls.md` | 全部 | 試過失敗、已淘汰的做法與原因 |
| `references/glossary.md` | 全部 | 名詞白話解釋、manifest 欄位 |
| `profiles/<使用者>/` | 主對話 | 該使用者的偏好、環境與檔案位置、進度 |

## 接手工作（新 session 一定先做）

新 session **看不到舊對話，也聯絡不到舊 agent**。照順序：

1. 在 `profiles/` 找到目前使用者的資料夾（例：`profiles/holo-maker/`）；沒有就先問使用者，依 `README.md` 建一份。
2. 讀該資料夾的 `environment.md`：服務怎麼登入、檔案放哪、manifest 在哪、期限（登入／訂閱到期）。
3. 讀最新的 manifest（每張圖的任務 ID、版本、檔案路徑、狀態、使用者回饋）。
4. 用 Artifact 清單（list）找到挑圖頁、檢討頁，看使用者最近的選擇與備註。
5. 讀 `history.md` 的目前狀態，**向使用者確認**進度與下一步，再依 manifest 派新 agent 接著做。
6. 檢查容器硬碟空間與登入是否過期；工作檔不在本機就從儲存空間或服務的任務 ID 取回。
