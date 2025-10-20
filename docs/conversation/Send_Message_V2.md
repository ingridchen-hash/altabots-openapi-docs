_Last updated at: 2025-10-20_

# **7.Send Message V2**

Send messages to a specified conversation ID (v2 version) and receive responses from the AI Agent. The API supports submitting text, images, audio, and documents as message content.

## **Request Method**

POST

## **Request URL**

`https://altatech.ai/v2/conversation/message`

## **Authentication**

Refer to the authentication instructions in the API Overview.

## **Request**

### **Request Example**

```
curl -X POST https://altatech.ai/v2/conversation/message \
--header 'Authorization: Bearer app-example_key_1234567890' \
--header 'Content-Type: application/json' \
--data '{
   "conversation_id": "67b590ca27008b39c60f30ef",
   "response_mode": "blocking",
   "messages": [
       {
           "role": "user",
           "content": "Hello"
       },
       {
           "role": "assistant",
           "content": "Hello! How can I assist you today?"
       },
       {
           "role": "user",
           "content": [
               {
                   "type": "text",
                   "text": "What is in this image?"
               },
               {
                   "type": "audio",
                   "audio": {
                       "url": "https://altatech.ai/example.mp3",
                       "name": "example audio",
                       "format": "mp3"
                   }
               },
               {
                   "type": "image",
                   "image": {
                       "url": "https://altatech.ai/example.png",
                       "format": "png",
                       "name": "example image"
                   }
               },
               {
                   "type": "document",
                   "document": {
                       "base64_content": "Your_file_base64_content",
                       "format": "pdf",
                       "name": "example pdf"
                   }
               }
           ]
       }
   ],
   "conversation_config": {
       "long_term_memory": false,
       "short_term_memory": false,
       "knowledge": {
           "data_ids": [
               "58c70da0403cc812641b9356",
               "59c70da0403cc812641df35k"
           ],
           "group_ids": [
               "67c70da0403cc812641b93je",
               "69c70da0403cc812641df35g"
           ]
       }
   }
}'
```

### **Request Headers**

| Field | Type | Description |
| :---- | :---- | :---- |
| Authorization | Bearer {token} | Use Authorization: Bearer {token} for authentication. Obtain the token from the API Key page. |
| Content-Type | application/json | Data type, must be application/json. |

### **Request Body**

| Field | Type | Required | Description |
| :---- | :---- | :---- | :---- |
| conversation\_id | string | Yes | Unique identifier for the conversation. Must provide the conversation\_id to continue the conversation. |
| response\_mode | string | Yes | Agent response mode: blocking: Wait until execution finishes and return results (may be interrupted if the process is too long). streaming: Stream response based on SSE ([Server-Sent Events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events)). webhook: Messages from Agent and human customer service will be sent to the webhook URL configured on the API page. |
| messages | JSON Array | Yes | Message content, supports roles user and assistant to construct conversation context. user message: At least one required, latest user message should be last. assistant message: Developers can construct assistant messages as context. |
| conversation\_config | object | No | Temporarily adjust Agent functionalities for special scenarios. short\_term\_memory: Enable or disable short-term memory for this conversation. long\_term\_memory: Enable or disable long-term memory for this conversation. knowledge: Customize knowledge retrieval scope for this conversation. Includes group\_ids and data\_ids. If both provided, retrieval occurs within their union. If both empty, no knowledge retrieval occurs. If omitted, default Agent knowledge configuration applies. group\_ids: Knowledge base ID, which may contain multiple knowledge documents. data\_ids: Knowledge document IDs in the knowledge base. |

*Note:*  
*Agent input and output configuration pages support different recognition schemes for different message types. Supported file types and sizes vary. Adjust API submission data accordingly. Maximum supported message formats:*

* *Text message: string*  
* *Audio message: .mp3, .wav, .acc*  
* *Image message: .jpg, .jpeg, .png, .gif, .webp*  
* *Document message: .pdf, .txt, .docx, .csv, .xlsx, .html, .json, .md, .tex, .ts, .xml, etc.*

## **Response**

### **Response Example**

```
{
    "create_time": 1679587005,
    "conversation_id": "657303a8a764d47094874bbe",
    "message_id": "65a4ccfC7ce58e728d5897e0",
    "output": [
        {
            "from_component_branch": "1",
            "from_component_name": "Component Name",
            "content": {
                "text": "Hi, is there anything I can help you with?",
                "audio": [
                    {
                        "audio": "http://altatech.ai/example.mp3",
                        "transcript": "Transcribed audio content"
                    }
                ]
            }
        }
    ],
    "usage": {
        "tokens": {
            "total_tokens": 29,
            "prompt_tokens": 19,
            "prompt_tokens_details": {
                "audio_tokens": 0,
                "text_tokens": 0
            },
            "completion_tokens": 10,
            "completion_tokens_details": {
                "reasoning_tokens": 0,
                "audio_tokens": 0,
                "text_tokens": 0
            }
        },
        "credits": {
            "total_credits": 0.0,
            "text_input_credits": 0.0,
            "text_output_credits": 0.0,
            "audio_input_credits": 0.0,
            "audio_output_credits": 0.0
        }
    }
}
```

### **Success Response (Blocking)**

| Field | Type | Description |
| :---- | :---- | :---- |
| conversation\_id | string | Conversation identifier for continuing conversation. |
| message\_id | string | Unique identifier for a specific message within a conversation. |
| create\_time | long | Timestamp when the message was generated. |
| output | JSON Array | Agent response content. |
| usage | object | Usage consumption details. |

### **Success Response (Streaming)**

| Field | Type | Description |
| :---- | :---- | :---- |
| code | int | Message type code: 3-Text, 10-FlowAgent output, 0-End, 4-Usage data, 39-Audio message. |
| message | string | Message type: Text, FlowOutput, End. |
| data | object | Response content. |

Text message streaming example:

```
{"code":11,"message":"MessageInfo","data":{"message_id":"6785dba0f06d872bff9ee347"}}
{"code":3,"message":"Text","data":"How "}
{"code":3,"message":"Text","data":"can "}
{"code":3,"message":"Text","data":"I "}
{"code":3,"message":"Text","data":"help "}
{"code":3,"message":"Text","data":"you?"}
{"code":0,"message":"End","data":null}
```

Audio message streaming example:

```
{"code":11,"message":"MessageInfo","data":{"message_id":"67b857b6be1f2906861a5e75"}}
{"code":39,"message":"Audio","data":{"audioAnswer":"","transcript":"Hello"}}
{"code":39,"message":"Audio","data":{"audioAnswer":"EQAUAA0...IA3bi","transcript":""}}
{"code":0,"message":"End","data":null}
```

### **Success Response (Webhook)**

If a webhook URL is configured, AltaBots system sends Agent and human customer service messages to the webhook URL. For details, see Webhook Mode.

### **Error Response**

| Field | Type | Description |
| :---- | :---- | :---- |
| code | int | Error code. |
| message | string | Error details. |

### **Error Codes**

| Code | Message |
| :---- | :---- |
| 40000 | Invalid parameters |
| 40127 | Developer authentication failed |
| 40356 | Conversation does not exist |
| 40358 | Conversation ID mismatch |
| 40364 | Agent does not support image modality |
| 50000 | Internal system error |
| 20040 | Exceeded question length limit |
| 20022 | Insufficient credits |
| 20055 | API usage forbidden, ensure API switch is enabled |
