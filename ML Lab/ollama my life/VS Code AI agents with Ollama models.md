# Using VS Code AI agents with local Ollama models (no GitHub Copilot)
Written by gpt4o with prompt:
>Find the best way to use AI agents in VScode that are driven by Ollama models. I want to go completely without github copilot

>my input will be in these note blocks
## Why run a local model?

Running an AI assistant locally avoids sending code to a cloud service, eliminates subscription costs and can be customized.  
>also I would like to strike a balance in my AI use

Ollama is a light‑weight runtime that downloads and runs quantized large‑language models (LLMs) on your hardware. It exposes a REST API that is compatible with the OpenAI Chat Completions API[ollama.com](https://ollama.com/blog/openai-compatibility#:~:text=To%20invoke%20Ollama%E2%80%99s%20OpenAI%20compatible,http%3A%2F%2Flocalhost%3A11434), which allows many tools to treat a local model like a remote OpenAI endpoint.

Depending on your needs there are several ways to integrate Ollama‑powered models into Visual Studio Code without using GitHub Copilot. The table below summarises the main options; detailed guidance follows.

| Option                                          | Highlights                                                                                                                                                                                                                                                                                                                                     | Key setup points                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ----------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Continue extension**                          | Free, open‑source assistant offering chat, tab completion, inline editing and RAG‑style context retrieval.                                                                                                                                                                                                                                     | Install **Continue** from the VS Code marketplace and Ollama on your machine. Run `ollama pull` to download models such as **StarCoder2** or **Llama 3**. Set `OLLAMA_HOST=0.0.0.0` if Ollama runs as a service. Edit Continue’s `config.json` to register each local model and select one for autocomplete[pedroalonso.net](https://www.pedroalonso.net/blog/local-ai-assitance-with-continue-ollama-vscode/#:~:text=Configuring%20Continue%20with%20Ollama). Optional: use `nomic‑embed‑text` embeddings for the `@codebase` provider[ollama.com](https://ollama.com/blog/continue-code-assistant#:~:text=Use%20%60nomic,codebase). |
| **Cline extension**                             | Autonomous coding agent capable of creating/editing files, running terminal commands and browsing with your permission. Works with local models via Ollama.                                                                                                                                                                                    | Install **Cline**. In VS Code, open Cline’s settings, choose **Ollama** as the API provider and set the base URL to `http://localhost:11434/`. Select your downloaded model from the list[docs.cline.bot](https://docs.cline.bot/running-models-locally/ollama#:~:text=3). Keep Ollama running in the background[docs.cline.bot](https://docs.cline.bot/running-models-locally/ollama#:~:text=,download%20may%20take%20several%20minutes).                                                                                                                                                                                            |
| **AI Toolkit (model playground/agent builder)** | Official VS Code toolkit for experimenting with models and building agents using the **Model Context Protocol**. Lets you add local models through a GUI.                                                                                                                                                                                      | Requires AI Toolkit v0.6.2+ and Ollama v0.4.1. In the AI Toolkit view, click **Add model → Add Ollama model**, select the downloaded model and click OK[code.visualstudio.com](https://code.visualstudio.com/docs/intelligentapps/models#:~:text=Add%20Ollama%20models). Models are available in the playground and agent builder; attachments are not yet supported[code.visualstudio.com](https://code.visualstudio.com/docs/intelligentapps/models#:~:text=AI%20Toolkit%20only%20shows%20models,refer%20to%20the%20Ollama%20documentation).                                                                                        |
| **Cody with local completion**                  | Sourcegraph’s Cody extension supports experimental local code completion via Ollama.                                                                                                                                                                                                                                                           | After installing Cody, set VS Code’s `cody.autocomplete.advanced.provider` to `experimental-ollama` and configure `cody.autocomplete.experimental.ollamaOptions` with your local URL and model (e.g., `url: "http://localhost:11434"`, `model: "codellama"`). This routes autocomplete requests to the local model[sourcegraph.com](https://sourcegraph.com/blog/local-code-completion-with-ollama-and-cody#:~:text=installed%2C%20update%20your%20VS%20Code,to%20use%20Ollama%20with%20Cody). Chat and other features still use Sourcegraph’s remote service.                                                                        |
| **OpenAI‑compatible endpoints**                 | Many VS Code extensions and frameworks that accept an OpenAI API URL can work with Ollama. The OpenAI compatibility layer in Ollama exposes an endpoint at `http://localhost:11434/v1` [ollama.com](https://ollama.com/blog/openai-compatibility#:~:text=To%20invoke%20Ollama%E2%80%99s%20OpenAI%20compatible,http%3A%2F%2Flocalhost%3A11434). | Any VS Code tool expecting an OpenAI‑style endpoint can be pointed at this URL with an arbitrary API key (ignored by Ollama).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
## 1. Using the Continue extension

1. **Install Continue and Ollama**.
    
    - Install the Continue extension from the VS Code marketplace.
        
    - Install Ollama from [ollama.com](https://ollama.com) for your OS. On macOS or Linux, set `OLLAMA_HOST="0.0.0.0"` so that Continue can connect[pedroalonso.net](https://www.pedroalonso.net/blog/local-ai-assitance-with-continue-ollama-vscode/#:~:text=,Integration).
        
2. **Download models**. Use `ollama pull <model-name>` to download a model. _StarCoder2_ and _DeepSeek Coder_ provide fast code completions, while _Llama 3.1 8b_ or _Gemma 2_ handle chat and general programming questions[pedroalonso.net](https://www.pedroalonso.net/blog/local-ai-assitance-with-continue-ollama-vscode/#:~:text=Code,7b). Quantized models require less memory but may trade accuracy.
    
3. **Configure Continue**. Open the command palette and run **“Continue: Open Config”**. In the `config.json` file, register each model with a descriptive title and specify `provider: "ollama"` and the exact model name[pedroalonso.net](https://www.pedroalonso.net/blog/local-ai-assitance-with-continue-ollama-vscode/#:~:text=Configuring%20Continue%20with%20Ollama). Example:
```json
{
  "models": [
    { "title": "Llama 3.1 8b", "provider": "ollama", "model": "llama3:8b" },
    { "title": "StarCoder2 3b", "provider": "ollama", "model": "starcoder2:3b" }
  ],
  "tabAutocompleteModel": { "title": "StarCoder2 3b", "provider": "ollama", "model": "starcoder2:3b" },
  "embeddingsProvider": { "title": "Nomic Embed Text", "provider": "ollama", "model": "nomic-embed-text" }
}
```

The `tabAutocompleteModel` field decides which model is used for completions and the `embeddingsProvider` enables context‑based retrieval with `@codebase`[pedroalonso.net](https://www.pedroalonso.net/blog/local-ai-assitance-with-continue-ollama-vscode/#:~:text=Configuring%20Continue%20with%20Ollama).
    
4. **Use Continue features**. _Chat_ (`Ctrl + L`) allows natural‑language queries with optional code context; _Autocomplete_ (`Tab`) inserts suggestions as you type; _Edit_ (`Ctrl + I`) rewrites selected code based on an instruction[pedroalonso.net](https://www.pedroalonso.net/blog/local-ai-assitance-with-continue-ollama-vscode/#:~:text=Leveraging%20Continue%E2%80%99s%20Features). You can experiment with multiple models or run embeddings locally for private code‑search[ollama.com](https://ollama.com/blog/continue-code-assistant#:~:text=Use%20%60nomic,codebase). Continue is open‑source and can be self‑hosted, but its AI quality varies and inline edits sometimes require manual adjustment[dev.to](https://dev.to/maximsaplin/continuedev-the-swiss-army-knife-that-sometimes-fails-to-cut-4gg3#:~:text=Yet%20as%20time%20passed%2C%20sharp,More%20on%20that%20later).
    

**Pros**: Free, open source, supports multiple models, allows local embeddings and retrieval, integrates seamlessly with VS Code.  
**Cons**: Requires manual configuration, GPU memory and CPU constraints; some features can be unstable or less accurate compared to commercial assistants[dev.to](https://dev.to/maximsaplin/continuedev-the-swiss-army-knife-that-sometimes-fails-to-cut-4gg3#:~:text=Yet%20as%20time%20passed%2C%20sharp,More%20on%20that%20later).
## 2. Using the Cline extension
> Not really interested in agentic coding. Vibe coding is bad and I dont like the code it produces
> What I want is really good RAG. I want the model to understand my codebase and be helpful in interpreting error messages and pulling documentation

Cline is an autonomous coding agent that performs multi‑step tasks in your project—similar to an agent—while asking for confirmation at each step. It can create or edit files, run commands and even control a browser in an embedded view[github.com](https://github.com/cline/cline#:~:text=Use%20the%C2%A0,changes%20your%20workspace%20more%20clearly).

1. **Install Cline** from the VS Code marketplace.
    
2. **Download a model via Ollama** (see above).
    
3. **Configure Cline**. Open the Cline settings (gear icon in the Cline panel), choose **“Ollama”** as the API provider and set the base URL to `http://localhost:11434/`. Then select the model you previously downloaded[docs.cline.bot](https://docs.cline.bot/running-models-locally/ollama#:~:text=3). Keep Ollama running in the background; otherwise Cline cannot connect[docs.cline.bot](https://docs.cline.bot/running-models-locally/ollama#:~:text=,download%20may%20take%20several%20minutes).
    
4. **Use Chat/Agent mode**. In chat mode you can interact with the model; in agent mode you can issue high‑level tasks. Cline will plan the steps, show the commands it wants to run and ask for approval. It uses the Model Context Protocol (MCP) so you can extend it with custom tools or remote capabilities[github.com](https://github.com/cline/cline#:~:text=).
    

**Pros**: Autonomous agent with ability to execute commands, browse, create files and integrate custom tools. The open‑source project allows local operation.  
**Cons**: Still maturing; some models lack tool‑calling support and tasks might fail if the model cannot use functions. Requires careful review of actions.

## 3. Using AI Toolkit (Visual Studio Code’s model playground and agent builder)

>seems a little too manual and not feature rich enough

Microsoft’s **AI Toolkit** provides a graphical interface to experiment with models, build simple agents and test prompts. Since version 0.6.2 it supports adding **Ollama** models.

1. **Install AI Toolkit** from the VS Code marketplace (requires version 0.6.2+).
    
2. **Ensure Ollama is installed**. Only models already downloaded in Ollama are shown[code.visualstudio.com](https://code.visualstudio.com/docs/intelligentapps/models#:~:text=AI%20Toolkit%20only%20shows%20models,to%20the%20%20289%20Ollama).
    
3. **Add a local model**. From the AI Toolkit model catalog, click **+ Add model → Add Ollama model**, choose **Select models from Ollama library**, pick the model you downloaded and click **OK**[code.visualstudio.com](https://code.visualstudio.com/docs/intelligentapps/models#:~:text=Add%20Ollama%20models). Models are listed under **My Models**.
    
4. **Use the playground or agent builder**. You can test prompts in the playground or build simple agents that execute step‑by‑step instructions. Note that attachments (e.g., files) are not supported when using the Ollama connector because the OpenAI compatibility layer lacks this functionality[code.visualstudio.com](https://code.visualstudio.com/docs/intelligentapps/models#:~:text=AI%20Toolkit%20only%20shows%20models,refer%20to%20the%20Ollama%20documentation).
    

**Pros**: Integrated with VS Code; no need to edit JSON; supports building basic agents via the Model Context Protocol.  
**Cons**: Limited features compared with Copilot or third‑party extensions; attachments not supported; still in active development.

## 4. Using Sourcegraph Cody with local completion

Cody provides AI coding assistance and has an experimental feature that lets you use a local model for code completions while leaving chat features remote. After installing Cody:

1. Add the following to your VS Code `settings.json`:
```json
"cody.autocomplete.advanced.provider": "experimental-ollama",
"cody.autocomplete.experimental.ollamaOptions": {
  "url": "http://localhost:11434",
  "model": "codellama"
}

```

1. This tells Cody to send autocomplete requests to your local model[sourcegraph.com](https://sourcegraph.com/blog/local-code-completion-with-ollama-and-cody#:~:text=installed%2C%20update%20your%20VS%20Code,to%20use%20Ollama%20with%20Cody). Replace `codellama` with the model you downloaded.
    
2. Restart VS Code. The Output panel will show the prompts and responses being sent to Ollama[sourcegraph.com](https://sourcegraph.com/blog/local-code-completion-with-ollama-and-cody#:~:text=for%20your%20VS%20Code%20files,when%20a%20completion%20is%20generated).
    

**Pros**: Familiar UI and high‑quality context retrieval for completions; easy to configure.  
**Cons**: Only completions are local; chat still uses Sourcegraph’s remote service (requires a login). Feature is marked experimental[sourcegraph.com](https://sourcegraph.com/blog/local-code-completion-with-ollama-and-cody#:~:text=installed%2C%20update%20your%20VS%20Code,to%20use%20Ollama%20with%20Cody).
> Deal breaker
## Choosing models

Different tasks benefit from different models. For tab completion and inline edits where speed matters, small code‑specialized models such as **StarCoder2 3b** or **DeepSeek Coder 6.7b** provide rapid suggestions[pedroalonso.net](https://www.pedroalonso.net/blog/local-ai-assitance-with-continue-ollama-vscode/#:~:text=Code,7b). For chat and general reasoning, general‑purpose LLMs such as **Llama 3.1 8b** or **Gemma 2 9b** offer broader knowledge[pedroalonso.net](https://www.pedroalonso.net/blog/local-ai-assitance-with-continue-ollama-vscode/#:~:text=General,and%20Gemma%202%209b). Ollama can run multiple models concurrently if your hardware has sufficient RAM/VRAM; you can assign one for chat and another for completions by adjusting the configuration[ollama.com](https://ollama.com/blog/continue-code-assistant#:~:text=Use%20DeepSeek%20Coder%206,Llama%203%208B%20for%20chat).
> gemma 3 for me 
## Hardware considerations and best practices

- **VRAM/CPU**: Larger models (e.g., 22 B parameter Codestral) require significant GPU memory and may only run on high‑end GPUs[ollama.com](https://ollama.com/blog/continue-code-assistant#:~:text=Try%20out%20Mistral%20AI%E2%80%99s%20Codestral,model%20for%20autocomplete%20and%20chat). Choose models appropriate for your hardware.
    
- **Running the server**: Start the model with `ollama run <model>` before using it in VS Code. On macOS use `launchctl setenv OLLAMA_HOST "0.0.0.0"` to allow connections[pedroalonso.net](https://www.pedroalonso.net/blog/local-ai-assitance-with-continue-ollama-vscode/#:~:text=1,launchctl).
    
- **Security**: Running a local LLM protects your code from cloud services, but some extensions may still phone home. Consider firewall rules to restrict network access.
    
- **Model quality**: Local models are improving rapidly but may still lag behind GPT‑4 or Claude in reasoning. Evaluate different models and quantization levels to find the best trade‑off between quality and performance.
    

## Summary

You can work with AI agents in VS Code without relying on GitHub Copilot by using local models via **Ollama**. Extensions like **Continue** and **Cline** provide open‑source chat/agent experiences, while **AI Toolkit** allows adding local models through a GUI and **Cody** offers experimental local completions. Each option requires running Ollama locally and pointing the extension to `http://localhost:11434`. By selecting suitable models and adjusting configuration files, you can achieve private, cost‑effective AI assistance tailored to your workflow.