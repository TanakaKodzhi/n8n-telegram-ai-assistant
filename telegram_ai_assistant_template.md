{
  "name": "ИИ-Ассистент Telegram",
  "nodes": [
    {
      "parameters": {
        "content": "### 📌 Инструкция по настройке\n1. **Telegram Trigger / Telegram Node**: выберите или создайте свои учетные данные (Telegram Bot API Token).\n2. **DeepSeek Chat Model**: добавьте учетные данные API DeepSeek.\n3. **AI Agent**: в параметре System Message можно настроить характер и роль ассистента под себя.",
        "height": 160,
        "width": 360,
        "color": 6
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [
        -40,
        -200
      ],
      "id": "7f12a3d0-51a8-444a-9b88-12cfa8a2e101",
      "name": "Sticky Note"
    },
    {
      "parameters": {
        "updates": [
          "message"
        ],
        "additionalFields": {}
      },
      "type": "n8n-nodes-base.telegramTrigger",
      "typeVersion": 1.5,
      "position": [
        0,
        0
      ],
      "id": "0622cac5-679e-4e30-bb0a-0e0fc6f0a304",
      "name": "Telegram Trigger"
    },
    {
      "parameters": {
        "promptType": "define",
        "text": "={{ $json.message.text }}",
        "options": {
          "systemMessage": "Communicate with me like you would with a real person; always give detailed answers; rely only on truthful facts; argue with me; point out my weak points; don’t settle for polite radio communication; if I’m talking nonsense, tell the truth, even if I really won’t like it; communicate politely and professionally; don’t make up facts."
        }
      },
      "type": "@n8n/n8n-nodes-langchain.agent",
      "typeVersion": 3.1,
      "position": [
        224,
        0
      ],
      "id": "f5f17dc5-c766-4e7c-a1ee-a02140d0086f",
      "name": "AI Agent"
    },
    {
      "parameters": {
        "model": "deepseek-chat",
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatDeepSeek",
      "typeVersion": 1,
      "position": [
        96,
        208
      ],
      "id": "c3bade5c-fa30-43ab-b95e-5114aebf63eb",
      "name": "DeepSeek Chat Model"
    },
    {
      "parameters": {
        "sessionIdType": "customKey",
        "sessionKey": "={{ $json.message.chat.id }}"
      },
      "type": "@n8n/n8n-nodes-langchain.memoryBufferWindow",
      "typeVersion": 1.4,
      "position": [
        240,
        208
      ],
      "id": "954cb2c5-3eea-462d-a0c6-f4afe699480f",
      "name": "Simple Memory"
    },
    {
      "parameters": {
        "chatId": "={{ $('Telegram Trigger').first().json.message.chat.id }}",
        "text": "={{ $json.output }}",
        "additionalFields": {
          "appendAttribution": false
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        592,
        0
      ],
      "id": "3413e708-a0b6-4779-b98a-d126243a4ca6",
      "name": "Send a text message"
    }
  ],
  "pinData": {},
  "connections": {
    "Telegram Trigger": {
      "main": [
        [
          {
            "node": "AI Agent",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "DeepSeek Chat Model": {
      "ai_languageModel": [
        [
          {
            "node": "AI Agent",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "Simple Memory": {
      "ai_memory": [
        [
          {
            "node": "AI Agent",
            "type": "ai_memory",
            "index": 0
          }
        ]
      ]
    },
    "AI Agent": {
      "main": [
        [
          {
            "node": "Send a text message",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": false,
  "settings": {
    "executionOrder": "v1",
    "binaryMode": "separate",
    "timezone": "UTC"
  },
  "meta": {
    "templateCredsSetupCompleted": false
  },
  "tags": []
}