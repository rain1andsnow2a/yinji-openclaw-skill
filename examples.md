# OpenClaw 提示词示例

如果当前 OpenClaw 平台暂时不能导入结构化 skill，可以先用这段：

```text
当我说“记一下”或“帮我写日记”时，请调用这个接口把内容写入映记：

POST http://yingjiapp.com/api/v1/integrations/openclaw/ingest
Authorization: Bearer {{YINJI_TOKEN}}
Content-Type: application/json

{
  "content": "用户刚刚说的内容",
  "title": "可选标题",
  "diary_date": "YYYY-MM-DD",
  "emotion_tags": ["开心"],
  "importance_score": 6,
  "mode": "append_today"
}

如果用户明确说“单独存一篇”，就把 mode 改为 create。
如果接口成功，就回复“已经帮你记进映记了”。
如果失败，就把接口返回的错误告诉我。
```

## 如果 JSON 模式不稳定

映记后端现在也兼容以下更宽松的写法：

### 纯文本

```text
POST http://yingjiapp.com/api/v1/integrations/openclaw/ingest?mode=append_today
Authorization: Bearer {{YINJI_TOKEN}}
Content-Type: text/plain

今天我想记一下：终于把接入流程跑通了。
```

### 表单

```text
POST http://yingjiapp.com/api/v1/integrations/openclaw/ingest
Authorization: Bearer {{YINJI_TOKEN}}
Content-Type: application/x-www-form-urlencoded

content=今天完成了部署&mode=append_today
```

## 字段说明（不只 content）

- `content`: 日记正文（必填）
- `title`: 日记标题（可选）
- `diary_date`: 日记日期（可选，格式 `YYYY-MM-DD`）
- `emotion_tags`: 情绪标签数组（可选）
- `importance_score`: 重要性评分（可选，1~10）
- `mode`: `append_today` 或 `create`
