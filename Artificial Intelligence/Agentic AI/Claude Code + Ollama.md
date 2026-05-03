You can run Anthropic's agentic coding tool (Claude Code) locally by connecting it to open-source models (like Qwen2.5-Coder or Llama 3) via Ollama. 

This setup enables free, private, AI-powered terminal coding by using `ollama` as a backend, replacing the paid Claude API with local model inference.

**Steps to Run Claude Code with Ollama**
1. **Install Ollama & Models:** Install [Ollama](https://ollama.com/), then pull a capable coding model (e.g., `ollama pull qwen2.5-coder`).
2. **Install Claude Code:** Install the tool using npm or the official script:  
    `curl -fsSL https://claude.ai/install.sh | bash`
3. **Configure Environment:** Point Claude Code to your local Ollama instance:
    - **macOS/Linux:** `export ANTHROPIC_BASE_URL=http://localhost:11434`
    - **Windows (PowerShell):** `$env:ANTHROPIC_BASE_URL="http://localhost:11434"`
4. **Run with Model:** Use the `claude` command, specifying the Ollama model:  
    `claude --model ollama/qwen2.5-coder` [](https://docs.ollama.com/integrations/claude-code#:~:text=To%20use%20Claude%20Code%20with%20Ollama%2C%20you,as:%20*%20qwen3.5%20*%20glm%2D5:cloud%20*%20kimi%2Dk2.5:cloud)

**Key Considerations**
- **Best Models:** Use high-performance local coding models such as `qwen2.5-coder` or `deepseek-coder-v2`.
- **Context Length:** Claude Code works best with large context windows. Ensure your model supports at least 32k or 64k tokens.
- **Capabilities:** The local setup allows reading, editing, and executing code within your terminal, similar to the paid version, but relies on your machine's GPU/CPU performance.
- **Auth Token:** Set a dummy token if required: `export ANTHROPIC_AUTH_TOKEN=ollama`.