# AI-AUTOMATION-2
curso Coder Automation
[checkpoint1_carlos_Bustos.json](https://github.com/user-attachments/files/32981137/checkpoint1_carlos_Bustos.json)
{
  "name": "1 Sistema de gestion de siniestros de automotores",
  "nodes": [
    {
      "parameters": {
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.chatTrigger",
      "typeVersion": 1.5,
      "position": [
        0,
        0
      ],
      "id": "f6f2732f-e1ac-42fd-a670-9c45a9e9670b",
      "name": "When chat message received",
      "webhookId": "560da6f4-0bd2-428f-9ffe-546195d2a52a"
    },
    {
      "parameters": {
        "promptType": "define",
        "text": "=Analiza el siguiente formulario y la documentación adjunta del siniestro:\n\n- Datos y relato del cliente:{{ $json.chatInput }}\n- \n\nDetermina si hay inconsistencias en el relato del cliente, posibles fraudes o montos que superen los $2.000.000 pesos argentinos. Responde solo con: \"Requiere Humano\" o \"Gestion Administrativa\".\n\nGenerá un registro en Hoja de calculo con un numero de ID.\n ",
        "options": {
          "systemMessage": "Eres un asistente de seguros de autos. Tu tarea es leer el formulario y los documentos adjuntos de un siniestro para decidir si el caso va a revisión humana o a gestión automática.\n\nRestricciones:\n- No utilices lenguaje inclusivo (prohibido la \"e\", \"x\" o \"@\"). Usa español tradicional.\n- Devuelve únicamente una de estas dos opciones exactas como respuesta: \"Requiere Humano\" o \"Gestion Administrativa\".\n",
          "maxIterations": 5
        }
      },
      "type": "@n8n/n8n-nodes-langchain.agent",
      "typeVersion": 3.1,
      "position": [
        224,
        0
      ],
      "id": "5d0f6aa2-d464-4986-bee0-2dcc5ef687d9",
      "name": "AI Agent"
    },
    {
      "parameters": {
        "model": {
          "__rl": true,
          "value": "gpt-5-mini",
          "mode": "list",
          "cachedResultName": "gpt-5-mini"
        },
        "responsesApiEnabled": false,
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatOpenAi",
      "typeVersion": 1.3,
      "position": [
        96,
        208
      ],
      "id": "42812d9f-f662-42df-a4e9-486b91395559",
      "name": "OpenAI Chat Model",
      "credentials": {
        "openAiApi": {
          "id": null,
          "name": "",
          "__aiGatewayManaged": true
        }
      }
    },
    {
      "parameters": {},
      "type": "@n8n/n8n-nodes-langchain.memoryBufferWindow",
      "typeVersion": 1.4,
      "position": [
        240,
        208
      ],
      "id": "8234b301-a22d-4ea7-bc2d-44164afbe4a1",
      "name": "Simple Memory"
    },
    {
      "parameters": {
        "descriptionType": "manual",
        "toolDescription": "=hoja de calculo",
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "1d0-6K76-79KMWIs-QSdSvHJGJIoQe6EOMxnG-KQtc4U",
          "mode": "list",
          "cachedResultName": "make  Cotizacion Dolar",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1d0-6K76-79KMWIs-QSdSvHJGJIoQe6EOMxnG-KQtc4U/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": "gid=0",
          "mode": "list",
          "cachedResultName": "Hoja 1",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1d0-6K76-79KMWIs-QSdSvHJGJIoQe6EOMxnG-KQtc4U/edit#gid=0"
        },
        "columns": {
          "mappingMode": "autoMapInputData",
          "value": {},
          "matchingColumns": [
            "action"
          ],
          "schema": [
            {
              "id": "action",
              "displayName": "action",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheetsTool",
      "typeVersion": 4.7,
      "position": [
        384,
        208
      ],
      "id": "6e6943ae-4949-479b-9cc4-c7a6185c2b55",
      "name": "Append row in sheet in Google Sheets",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "7JPD5C7ep1EHGgLY",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "sendTo": "carlosebustos25@gmail.com",
        "subject": "=Nuevo Siniestro ID {{ $('When chat message received').item.json.sessionId }}",
        "emailType": "text",
        "message": "=Hola, tenemos una nueva denuncia de siniestro, la misma clasifica como  {{ $json.output }}\n\nDetalles del siniestro: {{ $('When chat message received').item.json.chatInput }}",
        "options": {}
      },
      "type": "n8n-nodes-base.gmail",
      "typeVersion": 2.2,
      "position": [
        592,
        0
      ],
      "id": "d2c9ee70-cdfd-44a9-b1e5-35d667b7f27c",
      "name": "Send a message",
      "webhookId": "83c70c71-4cc2-4f15-b382-a05513bfb46f",
      "credentials": {
        "gmailOAuth2": {
          "id": "weBHjiCyXylhojyo",
          "name": "Gmail account"
        }
      }
    }
  ],
  "pinData": {},
  "connections": {
    "When chat message received": {
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
    "OpenAI Chat Model": {
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
            "node": "Send a message",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Append row in sheet in Google Sheets": {
      "ai_tool": [
        [
          {
            "node": "AI Agent",
            "type": "ai_tool",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": false,
  "settings": {
    "executionOrder": "v1",
    "binaryMode": "separate"
  },
  "versionId": "5f0695db-9cf3-449e-ba74-fa9b46b9d0da",
  "meta": {
    "templateCredsSetupCompleted": true,
    "instanceId": "f2a7fd80bd480efaa082e1becbc404380ab9d816cf925619d0e5a312b904bdb7"
  },
  "nodeGroups": [],
  "id": "cWohH4FHGh1BYITA",
  "tags": []
}
