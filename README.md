# figure-create-skill

一份 Claude skill：把動漫／角色概念圖做成可彩色 3D 列印的手辦模型，並以 Blender 檔交付。入口是 [`SKILL.md`](SKILL.md)。

這個 repo 只放文件，不放程式碼、圖片、模型，也不放任何金鑰或登入資料。

## 結構

```
SKILL.md                         入口：核心規則、流程一覽、文件導覽、接手步驟
references/                      通用（任何使用者都適用）
  coordinator.md                 主對話：溝通、派工、驗收把關、呈現成果、中斷接手
  agent-prompt-template.md       派工單範本
  pipeline.md                    agent：每一步的輸入／工具／輸出／驗收
  verification.md                驗收清單
  pitfalls.md                    已淘汰的做法與原因
  glossary.md                    名詞表、manifest 欄位
profiles/holo-maker/             一位使用者的範例設定（可替換）
  preferences.md                 溝通習慣、審美、過去的決定
  environment.md                 服務、儲存位置、頁面、接手步驟
  history.md                     系列時間線與目前進度
```

## 通用文件與使用者設定的分界

- `SKILL.md` 與 `references/`：只寫做法和規則，不寫任何人的帳號、桶名、路徑、喜好。
- `profiles/<使用者>/`：只寫這位使用者特有的事。通用文件提到「使用者的偏好」「儲存位置」時，一律指向這裡。

## 換給另一位使用者用

1. 複製 `profiles/holo-maker/` 成 `profiles/<新名稱>/`，三個檔案都改寫成新使用者的內容（舊內容全部刪掉）。
2. `environment.md` 至少要寫：用哪個圖轉 3D 服務與怎麼登入、圖像生成服務、成品／封存／manifest 放哪、給使用者看的頁面、期限。
3. `preferences.md` 至少要寫：使用的語言、成品格式（.blend？3MF？單色或彩色？）、審美限制。
4. 不要把金鑰或登入檔寫進任何檔案。
