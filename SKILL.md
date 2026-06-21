---
description: 使用阿里云百炼 CosyVoice 将文本转为语音，适用于用户要求播放语音、朗读文本、或有声输出场景。优先尝试发送语音消息，若不支持则发送音频文件。
---
### 🎯 技能基本信息

*   **技能名称**：阿里云tts语音合成（CosyVoice）
*   **功能**：使用你指定的声音复刻模型进行语音合成
*   **模型ID**：`cosyvoice-v3.5-flash`
*   **预设音色ID**：`cosyvoice-v3.5-flash-bailian-xxxxx`
*   **API Key**：`sk-xxxx`
*   **API端点**：`https://dashscope.aliyuncs.com/api/v1/services/audio/tts/SpeechSynthesizer`
*   **默认音量**：`volume=50`（范围0-100）
*   **默认语速**：`rate=1.0`（范围0.5-2.0）
* **API Key必须引导用户去阿里云百炼平台去获取**
* **示例中的API Key仅为占位符，请替换为用户的有效API Key。**
* **音色ID获取方式：请通过阿里云百炼平台的声音复刻创建你自己的专属音色，并使用返回的音色ID进行语音合成。**
* **示例中的音色ID为占位符，请替换为用户实际创建的音色ID。**
*   **优先尝试给用户发送语音消息**
*   **如果遇到平台不支持直接发送语音就直接将生成好的文件直接发给用户**
### 📝 技能使用说明

要使用这个技能，你需要按照以下步骤操作。**请注意，使用前需要先通过声音复刻创建你自己的专属音色。**

####  使用音色合成语音



**Python 代码示例 (直接调用API):**

```python
import requests

api_key = "sk-xxxx"  # 替换为你的API Key
url = "https://dashscope.aliyuncs.com/api/v1/services/audio/tts/SpeechSynthesizer"
headers = {
    "Authorization": f"Bearer {api_key}",
    "Content-Type": "application/json"
}

# 使用预设音色ID合成语音
voice_id = "cosyvoice-v3.5-flash-bailian-xxxxx" # 替换为你创建的音色ID
text = "你好，这是我的专属语音。"

payload = {
    "model": "cosyvoice-v3.5-flash",
    "input": {
        "text": text,
        "voice": voice_id,
        "format": "mp3"
        "volume": 50,
        "rate": 1.0
    }
}

resp = requests.post(url, headers=headers, json=payload, timeout=30)
if resp.status_code == 200:
    result = resp.json()
    audio_url = result["output"]["audio"]["url"]
    audio_resp = requests.get(audio_url, timeout=30)
    with open("output.mp3", "wb") as f:
        f.write(audio_resp.content)
    print("语音合成成功！")
else:
    print(f"失败: {resp.text}")
```

### ⚠️ 重要提醒

* **API Key必须引导用户去阿里云百炼平台去获取**
* **示例中的API Key仅为占位符，请替换为用户的有效API Key。**
* **音色ID获取方式：请通过阿里云百炼平台的声音复刻创建你自己的专属音色，并使用返回的音色ID进行语音合成。**
* **示例中的音色ID为占位符，请替换为用户实际创建的音色ID。**
*   **模型正确传参**：`model` 参数固定为 `cosyvoice-v3.5-flash`，长ID `cosyvoice-v3.5-flash-bailian-...` 作为 `voice`（音色ID）参数传入，两者不要搞混。
*   **音量语速参数**：每次调用都需带上 `volume=50`（音量，范围0-100）和 `rate=1.0`（语速，范围0.5-2.0），否则音质可能不达标。
*   **地域限制**：`cosyvoice-v3.5-flash` 模型**仅支持北京地域**。请确保你的API Key和配置都是针对北京地域的。
*   **API Key安全**：请务必将你的API Key保存在安全的地方，不要在代码或公开场合中明文暴露。
*   **平台兼容性**：如果你使用的平台不支持直接发送语音消息，技能会自动将生成的语音文件发送给你。请确保你的平台能够接收文件消息。
