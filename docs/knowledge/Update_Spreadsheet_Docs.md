_Last updated at: 2025-10-20_

# **Update Spreadsheet Docs (file)**

Batch update documents using a file. The system will sequentially perform chunking/slicing, embedding/vectorization, and finally replace the old document content with the new document content, while keeping the document ID unchanged.  
Note: The embedding model used is the default model and cannot be defined within the API.  
Note: Only return the upload result, do not return the final embedding result. You can obtain the final result through the "Query Document Status" API.

## **Request Method**

`PUT`

## **Request URL**

`https://altatech.ai/v1/bot/doc/spreadsheet/update`

## **Request Authentication**

See Overview for authentication details.

## **Request**

### **Request Example**

```bash
curl --location --request PUT 'https://altatech.ai/v1/bot/doc/spreadsheet/update' \
--header 'Authorization: Bearer YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data '{
    "knowledge_base_id": "67457fea6f658672d6482542",
    "chunk_token": 700,
    "header_row": 5,
    "files": [
        {
            "file_url": "https://www.AltaBots.ai/doc/spreadsheet.xlsx",
            "source_url": "https://www.AltaBots.ai/doc/spreadsheet.xlsx",
            "file_name": "spreadsheet_1.xlsx"
        }
    ]
}'
```

### **Request Header**

| Field | Type | Description |
| ----- | ----- | ----- |
| Authorization | Bearer {token} | Use `Authorization: Bearer {token}` for authentication. Please obtain the key as token from the API key page. |
| Content-Type | application/json | Data type, with the value being `application/json`. |

### **Request Body**

| Field | Type | Required | Description |
| ----- | ----- | ----- | ----- |
| files | Array<Object> | Yes | List of documents to be updated. Supports updating up to 20 documents simultaneously. |
| doc_id | String | Yes | The ID of the document to be updated. |
| file_url | String | No | The URL of the document being added.Supported formats: csv/xls/xlsx. Maximum size per document is 10MB. Note: Either URL or base64 must be provided. If both are provided, base64 takes priority. |
| file_base64 | String | No | The base64 of the document being added. Supported formats: csv/xls/xlsx. Maximum size per document is 10MB. Note: Either URL or base64 must be provided. If both are provided, base64 takes priority. |
| source_url | String | No | The source URL of the document being updated. Must comply with URL format specifications. If empty, the system will not update this value. To set this value to empty, please enter `NULL`. |
| chunk_token | Integer | No | The maximum number of tokens per knowledge chunk when chunking. Default value is 600. Range: 1-1000. |
| header_row | Integer | No | The maximum number of header rows. Table documents are chunked based on "header + data rows". Default value is 1. Range: 1-5. |

## **Response**

### **Response Example**

```json
{
    "doc": [
        {
            "doc_id": "xxxxxx",
            "doc_name": "test_1.csv"
        },
        {
            "doc_id": "xxxxxx",
            "doc_name": "test_2.xlsx"
        }
    ],
    "failed": [
        "xxxxxx",
        "xxxxxx"
    ]
}
```

### **Success Response**

| Field | Type | Description |
| :---: | :---: | :---: |
| doc | Array<Object> | List of documents to be updated. |
| doc_id | String | ID of the document to be updated. |
| doc_name | String | Name of the document to be updated. |
| failed | Array<Object> | List of failed document IDs to be updated. |

### **Failure Response**

| Field | Type | Description |
| :---- | :---- | :---- |
| code | Int | Error code. |
| message | String | Error details. |
