# jetbrains-ai-proxy

### get your jetbrains ai jwt token
![img.png](img.png)

### docker build 
```bash
docker build -t zouyq/jetbrains-ai-proxy ./
```

### Command Line Arguments and Environment Variables

##### Command Line Arguments

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `-p` | int | 8080 | Server listening port |
| `-h` | string | "0.0.0.0" | Server listening address |
| `-c` | string | "" | JWT Token value (JWT_TOKEN) |
| `-k` | string | "" | Bearer Token value (BEARER_TOKEN) |

###### Environment Variables

| Variable | Type | Required | Description |
|----------|------|----------|-------------|
| `JWT_TOKEN` | string | Yes | Jetbrains AI Token |
| `BEARER_TOKEN` | string | Yes | Bearer Token for authentication |

##### Usage Examples

```bash
# Using command line arguments
./jetbrains-ai-proxy -p 8080 -c "cookie" -k "token"

# Using environment variables
export JWT_TOKEN="your_jwt_token"
export BEARER_TOKEN="your_bearer_token"
./jetbrains-ai-proxy

# Mixed usage (command line overrides environment variables)
export JWT_TOKEN="env_jwt_token"
./jetbrains-ai-proxy -p 9090 -k "cli_bearer_token"

### run docker use environment or command args
docker run -it -p 8080:8080 zouyq/jetbrains-ai-proxy -k $auth_token -c $jwt_token
```

### /v1/models
```bash
curl 'http://localhost:8080/v1/models' \
                            -H 'Authorization: Bearer 2'
{
    "object": "list",
    "data": [
        {
            "id": "gemini-flash-2.0",
            "object": "model",
            "owned_by": "google",
            "profile": "google-chat-gemini-flash-2.0"
        }
        ……
    ]
}
```
### /v1/chat/completions
```bash
curl 'http://localhost:8080/v1/chat/completions' \
                              -H 'Authorization: Bearer 2' \
                              -H 'Content-Type: application/json' \
                              -d '{
                            "model": "claude-4-sonnet",
                            "messages": [
                              {
                                "role": "user",
                                "content": "你是谁"
                              }
                            ],
                            "stream": true
                          }'

data: {"id":"chatcmpl-1751441945","object":"chat.completion.chunk","created":1751441945,"model":"claude-4-sonnet","choices":[{"index":0,"delta":{"role":"assistant"},"finish_reason":null,"content_filter_results":{"hate":{"filtered":false},"self_harm":{"filtered":false},"sexual":{"filtered":false},"violence":{"filtered":false},"jailbreak":{"filtered":false,"detected":false},"profanity":{"filtered":false,"detected":false}}}],"system_fingerprint":"J32IAp9LKm"}

……

data: [DONE]
```
### support model list

| No. | Model ID            | Object | Profile                          | Owned By  |
|-----|---------------------|--------|----------------------------------|-----------|
| 1   | claude-3.5-sonnet   | model  | anthropic-claude-3.5-sonnet      | anthropic |
| 2   | o3-mini             | model  | openai-o3-mini                   | openai    |
| 3   | gpt-4o              | model  | openai-gpt-4o                    | openai    |
| 4   | gpt4.1              | model  | openai-gpt4.1                    | openai    |
| 5   | gpt4.1-mini         | model  | openai-gpt4.1-mini               | openai    |
| 6   | claude-3.7-sonnet   | model  | anthropic-claude-3.7-sonnet      | anthropic |
| 7   | claude-4-sonnet     | model  | anthropic-claude-4-sonnet        | anthropic |
| 8   | o1                  | model  | openai-o1                        | openai    |
| 9   | gemini-pro-2.5      | model  | google-chat-gemini-pro-2.5       | google    |
| 10  | gemini-flash-2.0    | model  | google-chat-gemini-flash-2.0     | google    |
| 11  | gemini-flash-2.5    | model  | google-chat-gemini-flash-2.5     | google    |
| 12  | claude-3.5-haiku    | model  | anthropic-claude-3.5-haiku       | anthropic |
| 13  | o3                  | model  | openai-o3                        | openai    |
| 14  | o4-mini             | model  | openai-o4-mini                   | openai    |
| 15  | gpt4.1-nano         | model  | openai-gpt4.1-nano               | openai    |

- **OpenAI**: 8 models (o3-mini, gpt-4o, gpt4.1, gpt4.1-mini, o1, o3, o4-mini, gpt4.1-nano)
- **Anthropic**: 4 models (claude-3.5-sonnet, claude-3.7-sonnet, claude-4-sonnet, claude-3.5-haiku)
- **Google**: 3 models (gemini-pro-2.5, gemini-flash-2.0, gemini-flash-2.5)
