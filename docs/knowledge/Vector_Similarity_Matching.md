_Last updated at: 2025-10-20_

# **Vector Similarity Matching**

Convert keywords to vectors and match to document IDs, perform vector retrieval and return top K results ranked by keyword similarity.

## **Request Method**

`POST`

## **Request URL**

`https://altatech.ai/v1/vector/match`

## **Request Authentication**

See Overview for authentication details.

## **Request**

### **Request Example**

```bash
curl -X POST https://altatech.ai/v1/vector/match \
   -H 'Authorization: Bearer your_apikey' \
   -H 'Content-Type: application/json' \
   -d '{
        "embedding_rate": "1",
        "prompt": "Please introduce Aurora.",
        "data_ids": [
            "1234567890",
            "1230987654"
        ],
        "top_k": "5"
    }'
```

### **Request Headers**

| Field | Type | Description |
| :---- | :---- | :---- |
| Authorization | Bearer {token} | Use Authorization: Bearer {token} for authentication. Get the key from the API Keys page as token. |
| Content-Type | application/json | Data type, set to application/json. |
### **Request Body**

| Field | Type | Required | Description |
| :---- | :---- | :---- | :---- |
| embedding_rate | float | No | Knowledge vector retrieval, vector retrieval proportion, default 1, value range: [0,1] |
| prompt | string | Yes | Keywords for vector similarity matching against documents in the bot. |
| data_ids | array | No | Document IDs to match the keyword vectors against. Can specify multiple knowledge document IDs from agents. Defaults to all docs if empty. |
| top_k | int | Yes | Number of top similar results to return after matching keywords to document IDs. Only 1-10 allowed. |

## **Response**

### **Response Example**

```json
{
    "total": 2,
    "list": [
        {
            "content": "Test data",
            "data_id": "aS1CNvPK4XCckDKQNj7azC9a",
            "score": 0.75
        },
        {
            "content": "Test data",
            "data_id": "aS1CNvPK4XCckDKQNj7azC9a",
            "score": 0.75
        }
    ]
}
```

### **Success Response**

| Field | Type | Description |
| :---- | :---- | :---- |
| total | int | The total number of returned fragments. |
| list | JSON Array | Fragment list. |
| content | string | Snippet content. |
| data_id | string | Source document ID. |
| score | float | Similarity score. |

### **Failure Response**

| Field | Type | Description |
| :---- | :---- | :---- |
| code | int | Error code. |
| message | string | Error details. |

### **Error Codes**

| Code | Message |
| :---- | :---- |
| 40000 | Invalid parameter |
| 40127 | Developer authentication failed |
| 20059 | Agent deleted |
| 40332 | Documents queried cannot exceed 10 |
