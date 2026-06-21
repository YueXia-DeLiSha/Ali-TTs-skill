```markdown
# 🎙️ 阿里云百炼 CosyVoice TTS 技能

> 基于阿里云百炼 `cosyvoice-v3.5-flash` 模型，使用您自己的复刻音色，将文本转为自然流畅的语音。  
> 适用于需要语音播报、有声内容输出、智能助手朗读等场景。

---

## ✨ 功能特点

- **专属音色复刻**：通过阿里云百炼的声音复刻功能，创建属于您自己的独特音色。
- **高质量合成**：基于 CosyVoice v3.5 模型，音质自然、情感丰富。
- **灵活参数调节**：支持音量（0～100）和语速（0.5～2.0）自定义。
- **多格式输出**：输出 MP3 音频，可直接播放或保存为文件。
- **即拿即用**：提供简洁的 Python 调用示例，快速集成到您的项目或平台中。
- **智能降级**：优先尝试发送语音消息，若不支持则自动转为发送音频文件。

---

## 📦 前置准备

在使用本技能前，您需要：

1. **注册/登录** [阿里云百炼平台](https://bailian.console.aliyun.com/)。
2. **开通语音合成服务**，并获取 API Key（需配置北京地域）。
3. **创建声音复刻**，生成您的专属音色 ID（`cosyvoice-v3.5-flash-bailian-xxxxx`）。

> ⚠️ 本技能中的 API Key 和音色 ID 均为占位符，请替换为您自己的有效凭证。

---

## 🚀 快速开始

### 安装依赖

```bash
pip install requests
```

### 核心代码

```python
import requests

api_key = "sk-xxxx"  # 您的 API Key
url = "https://dashscope.aliyuncs.com/api/v1/services/audio/tts/SpeechSynthesizer"

headers = {
    "Authorization": f"Bearer {api_key}",
    "Content-Type": "application/json"
}

voice_id = "cosyvoice-v3.5-flash-bailian-xxxxx"  # 您的音色 ID
text = "你好，这是我的专属语音。"

payload = {
    "model": "cosyvoice-v3.5-flash",
    "input": {
        "text": text,
        "voice": voice_id,
        "format": "mp3",
        "volume": 50,      # 0～100
        "rate": 1.0        # 0.5～2.0
    }
}

resp = requests.post(url, headers=headers, json=payload, timeout=30)
if resp.status_code == 200:
    result = resp.json()
    audio_url = result["output"]["audio"]["url"]
    audio_resp = requests.get(audio_url, timeout=30)
    with open("output.mp3", "wb") as f:
        f.write(audio_resp.content)
    print("✅ 语音合成成功！")
else:
    print(f"❌ 失败: {resp.text}")
```

---

## 📋 参数说明

| 参数 | 类型 | 范围/固定值 | 说明 |
|------|------|------------|------|
| `model` | string | `cosyvoice-v3.5-flash` | **固定**，模型名称 |
| `voice` | string | 复刻生成的长ID | 您的专属音色 ID |
| `text` | string | - | 要合成的文本（最长支持数千字符） |
| `format` | string | `mp3` | 输出音频格式 |
| `volume` | integer | 0～100（默认50） | 音量大小 |
| `rate` | float | 0.5～2.0（默认1.0） | 语速倍率 |

---

## ⚠️ 重要注意事项

- **地域限制**：`cosyvoice-v3.5-flash` 模型**仅支持北京地域**，请确保您的 API Key 和服务配置均在北京。
- **模型与音色 ID 区分**：  
  - `model` 固定为 `cosyvoice-v3.5-flash`  
  - 长 ID（如 `cosyvoice-v3.5-flash-bailian-xxxxx`）作为 `voice` 参数传入
- **API Key 安全**：请勿在公开代码、日志或前端暴露您的 API Key，建议使用环境变量或密钥管理服务。
- **音量/语速必传**：每次调用都需携带 `volume` 和 `rate`，否则可能导致音质不达标。
- **文件接收兼容**：若您的平台不支持直接播放语音，技能会自动输出 MP3 文件，请确保您的环境能接收文件消息。

---

## 🔧 常见问题

**Q：如何获取音色 ID？**  
A：登录阿里云百炼平台 → 进入“声音复刻” → 创建并训练您的专属音色 → 复制返回的音色 ID。

**Q：合成失败，返回 401？**  
A：请检查 API Key 是否正确，并确认已开通服务且地域为北京。

**Q：可以合成多长的文本？**  
A：建议控制在 1000 字以内，超长文本建议分段合成。

**Q：支持其他语言吗？**  
A：支持中文、英文及中英混读，具体取决于复刻音色的训练数据。

---

## 📄 许可证

本技能仅作为阿里云百炼服务的调用示例，使用时请遵守阿里云的相关服务条款。

---

## 🤝 贡献与反馈

欢迎提交 Issue 或 PR，共同完善本技能的使用体验。
---