This stems from the preliminary research that I did in [VS Code AI agents with Ollama models](VS%20Code%20AI%20agents%20with%20Ollama%20models.md)

After a dicussion with GPT 4o, I walked away with this summary on the session
## 🧠 Summary: Enhanced Ollama + Continue Setup

### 🧱 1. **Docker Environment & Runtime Tweaks**

You originally had:
```
OLLAMA_CONTEXT_WINDOW_SIZE=128         # (invalid or deprecated)
OLLAMA_MEMORY_POOL_SIZE=8GB            # (too low for long context)
```

We replaced with:
```
OLLAMA_NUM_CTX=131072                  # Enables 128K token context
OLLAMA_MEMORY_POOL_SIZE=16GB           # Maximize memory pool
OLLAMA_LOW_VRAM=true                   # Offload to system RAM
OLLAMA_NUM_GPU=1                       # Set GPU usage (or 0 for CPU)
```

It also suggested that I change to models that can handle the context window 
I found gemma3:4b to be a good match, however, this model does not have the option to use the agent feature or the planning feature

It also suggested that I change some things in the continue configs, But I got decent enough results without these:
```json
"models": {
  "default": {
    "provider": "ollama",
    "model": "yi-200k",
    "apiBase": "http://172.17.0.2:11434",
    "contextLength": 131072
  }
}
"contextProviders": [
  {
    "name": "repo",
    "params": {
      "embeddingProvider": {
        "provider": "nomic",
        "model": "nomic-embed-text-v1.5"
      },
      "maxTokens": 96000,
      "topK": 15
    }
  }
]

```

One suggestion that I should consider in the #future is to edit the context ignore configs:

```
node_modules/
*.min.js
*.json
dist/
``` 

more things to do in the #future include:
- getting speech to text to work
- adding mcp servers? https://dev.to/debs_obrien/building-your-first-mcp-server-a-beginners-tutorial-5fag#:~:text=What%20is%20an%20MCP%20Server,information%20for%20any%20city%20worldwide.
-  getting web search to work
