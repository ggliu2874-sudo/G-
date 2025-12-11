# LINE Bot + Pinecone RAG 建立 Prompt

請使用以下 Prompt 一次完成 LINE Bot 專案建立：

---

## Prompt

```
請幫我建立一個 LINE Bot 客服機器人，整合 Pinecone RAG 知識庫問答功能。

## 技術需求
- 語言：Python + Flask
- SDK：line-bot-sdk v3.x（使用 linebot.v3 模組）
- RAG：Pinecone Assistant API
- 部署平台：Vercel (Serverless)

## 功能需求
1. 關鍵字自動回覆（服務時間、聯絡方式、幫助）
2. 非關鍵字訊息 → 使用 Pinecone RAG 回答
3. 健康檢查端點 (/) 和 Webhook 端點 (/api/webhook)

## 專案結構
LineBot/
├── api/
│   └── index.py          # Flask 主程式 + Webhook
├── vercel.json           # Vercel 設定
├── requirements.txt      # 依賴（flask, line-bot-sdk, pinecone, pinecone-plugin-assistant）
├── .env.example          # 環境變數範例
├── .gitignore
└── README.md

## 環境變數（Vercel 設定）
- LINE_CHANNEL_SECRET
- LINE_CHANNEL_ACCESS_TOKEN
- PINECONE_API_KEY

## 重要技術細節
1. requirements.txt 不要鎖定版本，讓 pip 自動解決依賴
2. 使用 line-bot-sdk v3 的 linebot.v3 模組（非 v2）
3. Pinecone Assistant 名稱：readpdf
4. Webhook 路由：/api/webhook
5. 在 webhook POST handler 內部 import LINE SDK（避免初始化錯誤）

## 程式碼邏輯
1. get_response(user_message) → 先檢查關鍵字 → 沒有則呼叫 ask_pinecone_rag()
2. ask_pinecone_rag(question) → 使用 Pinecone Assistant API 回答
3. webhook() → 解析 LINE 事件 → 取得用戶訊息 → 回覆

## 部署步驟
1. 推送到 GitHub
2. Vercel 連結 repository 並設定環境變數
3. LINE Developers Console 設定 Webhook URL

請提供所有檔案的完整程式碼。
```

---

## 快速參考

### requirements.txt
```
flask
line-bot-sdk
pinecone
pinecone-plugin-assistant
```

### vercel.json
```json
{
  "version": 2,
  "builds": [
    { "src": "api/index.py", "use": "@vercel/python" }
  ],
  "routes": [
    { "src": "/api/webhook", "dest": "api/index.py" },
    { "src": "/(.*)", "dest": "api/index.py" }
  ]
}
```

### LINE Developers Console 設定
| 設定項目 | 值 |
|---------|---|
| Webhook URL | https://YOUR_PROJECT.vercel.app/api/webhook |
| Use webhook | ✅ 開啟 |
| Auto-reply messages | ❌ 關閉 |
