<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>OmniChat — All your AI chats</title>
  <style>
    :root {
      color-scheme: dark;
      --bg: #212121;
      --sidebar: #171717;
      --panel: #2f2f2f;
      --panel-hover: #3a3a3a;
      --text: #ececec;
      --muted: #a0a0a0;
      --border: #3b3b3b;
      --accent: #10a37f;
    }

    * { box-sizing: border-box; }

    body {
      margin: 0;
      height: 100vh;
      overflow: hidden;
      background: var(--bg);
      color: var(--text);
      font: 15px/1.5 Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    }

    button, textarea { font: inherit; }
    button { color: inherit; cursor: pointer; }

    .app { display: flex; height: 100vh; }

    .sidebar {
      width: 260px;
      flex: 0 0 260px;
      display: flex;
      flex-direction: column;
      gap: 16px;
      padding: 16px 12px;
      background: var(--sidebar);
      border-right: 1px solid #292929;
    }

    .brand-row, .side-action, .profile, .topbar, .composer-tools {
      display: flex;
      align-items: center;
    }

    .brand-row { justify-content: space-between; padding: 0 5px; }

    .brand {
      display: flex;
      align-items: center;
      gap: 9px;
      font-size: 17px;
      font-weight: 700;
      letter-spacing: -.3px;
    }

    .brand-mark {
      display: grid;
      width: 29px;
      height: 29px;
      place-items: center;
      border-radius: 9px;
      background: var(--accent);
      color: white;
      font-size: 16px;
    }

    .icon-btn, .side-action, .chat-link, .suggestion, .model-button, .send-btn {
      border: 0;
      background: transparent;
    }

    .icon-btn {
      width: 34px;
      height: 34px;
      border-radius: 9px;
      color: var(--muted);
      font-size: 19px;
    }

    .icon-btn:hover, .side-action:hover, .chat-link:hover { background: #292929; }

    .side-action {
      gap: 11px;
      width: 100%;
      padding: 10px;
      border-radius: 9px;
      text-align: left;
      color: var(--text);
    }

    .side-action .plus { color: var(--muted); font-size: 20px; }
    .history-label {
      padding: 8px 10px 0;
      color: #858585;
      font-size: 12px;
      font-weight: 600;
    }

    .history { flex: 1; overflow-y: auto; }

    .chat-link {
      display: block;
      width: 100%;
      overflow: hidden;
      padding: 9px 10px;
      border-radius: 8px;
      color: #c7c7c7;
      text-align: left;
      text-overflow: ellipsis;
      white-space: nowrap;
      font-size: 13px;
    }

    .profile {
      gap: 10px;
      padding: 11px 8px 3px;
      border-top: 1px solid #303030;
    }

    .avatar {
      display: grid;
      width: 32px;
      height: 32px;
      flex: 0 0 32px;
      place-items: center;
      border-radius: 50%;
      background: #6e5be8;
      font-size: 13px;
      font-weight: 700;
    }

    .profile-name { font-size: 13px; font-weight: 600; }
    .profile-note { color: var(--muted); font-size: 11px; }

    .main { position: relative; display: flex; min-width: 0; flex: 1; flex-direction: column; }

    .topbar {
      justify-content: space-between;
      height: 58px;
      padding: 0 22px;
    }

    .model-wrap { position: relative; }

    .model-button {
      display: flex;
      align-items: center;
      gap: 8px;
      padding: 8px 10px;
      border-radius: 9px;
      color: var(--text);
      font-size: 15px;
      font-weight: 600;
    }

    .model-button:hover { background: #2a2a2a; }
    .model-button .chevron { color: var(--muted); font-size: 12px; }

    .dropdown {
      position: absolute;
      z-index: 5;
      top: 44px;
      left: 0;
      display: none;
      width: 290px;
      max-height: 72vh;
      overflow-y: auto;
      padding: 8px;
      border: 1px solid var(--border);
      border-radius: 13px;
      background: #252525;
      box-shadow: 0 16px 45px #0008;
    }

    .dropdown.open { display: block; }
    .dropdown-label {
      padding: 8px 9px 5px;
      color: var(--muted);
      font-size: 11px;
      font-weight: 700;
      letter-spacing: .08em;
      text-transform: uppercase;
    }

    .model-option {
      display: flex;
      width: 100%;
      flex-direction: column;
      gap: 2px;
      padding: 9px;
      border: 0;
      border-radius: 8px;
      background: transparent;
      text-align: left;
    }

    .model-option:hover { background: #363636; }
    .model-option strong { font-size: 13px; font-weight: 600; }
    .model-option span { color: var(--muted); font-size: 11px; }

    .top-actions { display: flex; gap: 5px; }
    .top-actions button {
      padding: 7px 10px;
      border: 1px solid var(--border);
      border-radius: 9px;
      background: transparent;
      color: #d1d1d1;
      font-size: 12px;
    }
    .top-actions button:hover { background: #303030; }

    .conversation {
      display: flex;
      min-height: 0;
      flex: 1;
      flex-direction: column;
      align-items: center;
      overflow-y: auto;
      padding: 20px 20px 190px;
    }

    .welcome {
      width: min(720px, 100%);
      margin: auto 0;
      padding: 30px 0;
      text-align: center;
    }

    .welcome h1 {
      margin: 0 0 9px;
      font-size: clamp(27px, 4vw, 36px);
      font-weight: 600;
      letter-spacing: -1.1px;
    }

    .welcome p { margin: 0; color: var(--muted); }

    .suggestions {
      display: grid;
      grid-template-columns: repeat(2, minmax(0, 1fr));
      gap: 10px;
      margin: 27px auto 0;
      text-align: left;
    }

    .suggestion {
      padding: 13px 14px;
      border: 1px solid var(--border);
      border-radius: 12px;
      background: #262626;
      text-align: left;
    }

    .suggestion:hover { background: var(--panel-hover); }
    .suggestion strong { display: block; margin-bottom: 3px; font-size: 13px; }
    .suggestion span { color: var(--muted); font-size: 12px; }

    .messages { width: min(760px, 100%); }
    .message { display: flex; gap: 13px; margin: 20px 0 28px; }
    .message .avatar { width: 30px; height: 30px; flex-basis: 30px; }
    .message-body { min-width: 0; flex: 1; padding-top: 2px; white-space: pre-wrap; overflow-wrap: anywhere; }
    .message-role { margin-bottom: 5px; font-size: 13px; font-weight: 650; }
    .message-text { color: #e2e2e2; }
    .message.assistant .avatar { background: var(--accent); }

    .composer-area {
      position: absolute;
      right: 0;
      bottom: 0;
      left: 0;
      padding: 30px 20px 14px;
      background: linear-gradient(transparent, var(--bg) 28%);
    }

    .composer {
      width: min(760px, 100%);
      margin: 0 auto;
      padding: 12px 12px 8px 16px;
      border: 1px solid #4b4b4b;
      border-radius: 22px;
      background: #2f2f2f;
      box-shadow: 0 4px 25px #0002;
    }

    .composer textarea {
      display: block;
      width: 100%;
      min-height: 31px;
      max-height: 180px;
      resize: none;
      border: 0;
      outline: 0;
      background: transparent;
      color: var(--text);
      line-height: 1.5;
    }

    .composer textarea::placeholder { color: #a0a0a0; }

    .composer-tools { justify-content: space-between; margin-top: 7px; }
    .tool-left { display: flex; gap: 7px; }

    .tool-btn {
      padding: 5px 8px;
      border: 1px solid #484848;
      border-radius: 8px;
      background: transparent;
      color: #c5c5c5;
      font-size: 12px;
    }

    .tool-btn:hover { background: #424242; }
    .send-btn {
      display: grid;
      width: 33px;
      height: 33px;
      place-items: center;
      border-radius: 50%;
      background: #f2f2f2;
      color: #202020;
      font-size: 18px;
      font-weight: 700;
    }

    .send-btn:disabled { opacity: .45; cursor: default; }
    .disclaimer { margin-top: 10px; color: #888; text-align: center; font-size: 11px; }

    @media (max-width: 700px) {
      .sidebar { display: none; }
      .topbar { padding: 0 12px; }
      .top-actions button { padding: 7px; }
      .conversation { padding-right: 14px; padding-left: 14px; }
      .composer-area { padding-right: 12px; padding-left: 12px; }
      .suggestions { grid-template-columns: 1fr; }
      .welcome { margin: auto 0; }
    }
  </style>
</head>
<body>
  <div class="app">
    <aside class="sidebar">
      <div class="brand-row">
        <div class="brand"><span class="brand-mark">✳</span> OmniChat</div>
        <button class="icon-btn" aria-label="Toggle sidebar">☰</button>
      </div>

      <button class="side-action" id="newChat"><span class="plus">＋</span> New chat</button>

      <div class="history-label">Recent</div>
      <div class="history" id="history">
        <button class="chat-link">How to plan a weekend trip</button>
        <button class="chat-link">Explain neural networks simply</button>
        <button class="chat-link">Ideas for a healthy dinner</button>
      </div>

      <div class="profile">
        <div class="avatar">I</div>
        <div><div class="profile-name">Your workspace</div><div class="profile-note">All models, one place</div></div>
      </div>
    </aside>

    <main class="main">
      <header class="topbar">
        <div class="model-wrap">
          <button class="model-button" id="modelButton" aria-expanded="false">
            <span id="selectedModel">OmniChat</span><span class="chevron">▼</span>
          </button>
          <div class="dropdown" id="modelDropdown" role="menu"></div>
        </div>
        <div class="top-actions">
          <button id="compareButton" title="Demo comparison mode">Compare models</button>
          <button id="shareButton">Share</button>
        </div>
      </header>

      <section class="conversation" id="conversation">
        <div class="welcome" id="welcome">
          <h1>What can I help with?</h1>
          <p>Choose an AI model, or start with OmniChat.</p>
          <div class="suggestions">
            <button class="suggestion" data-prompt="Help me brainstorm a creative project"><strong>Brainstorm ideas</strong><span>Find a fresh direction for a project</span></button>
            <button class="suggestion" data-prompt="Explain a difficult topic in simple terms"><strong>Learn something</strong><span>Make a complicated topic easier</span></button>
            <button class="suggestion" data-prompt="Help me write a clear, friendly email"><strong>Write an email</strong><span>Draft a message with the right tone</span></button>
            <button class="suggestion" data-prompt="Write a small JavaScript function that sorts names alphabetically"><strong>Write code</strong><span>Get help building or fixing something</span></button>
          </div>
        </div>
        <div class="messages" id="messages"></div>
      </section>

      <div class="composer-area">
        <form class="composer" id="chatForm">
          <textarea id="prompt" rows="1" placeholder="Message OmniChat" aria-label="Message"></textarea>
          <div class="composer-tools">
            <div class="tool-left">
              <button class="tool-btn" type="button" id="attachButton">＋ Attach</button>
              <button class="tool-btn" type="button" id="webButton">◎ Web search</button>
            </div>
            <button class="send-btn" id="sendButton" type="submit" aria-label="Send" disabled>↑</button>
          </div>
        </form>
        <div class="disclaimer">AI can make mistakes. Check important information.</div>
      </div>
    </main>
  </div>

  <input id="fileInput" type="file" hidden multiple>

  <script>
    const models = [
      { group: "All-in-one", name: "OmniChat", detail: "Choose the best available model" },
      { group: "OpenAI", name: "GPT-4o", detail: "OpenAI · General purpose" },
      { group: "Anthropic", name: "Claude 3.7 Sonnet", detail: "Anthropic · Writing and reasoning" },
      { group: "Google", name: "Gemini 2.5 Pro", detail: "Google · Multimodal and research" },
      { group: "DeepSeek", name: "DeepSeek R1", detail: "DeepSeek · Reasoning" },
      { group: "xAI", name: "Grok", detail: "xAI · Conversational assistant" },
      { group: "Meta", name: "Llama", detail: "Meta · Open model family" },
      { group: "Mistral", name: "Mistral Large", detail: "Mistral · Multilingual assistant" }
    ];

    const dropdown = document.getElementById("modelDropdown");
    const modelButton = document.getElementById("modelButton");
    const selectedModel = document.getElementById("selectedModel");
    const promptInput = document.getElementById("prompt");
    const sendButton = document.getElementById("sendButton");
    const messages = document.getElementById("messages");
    const welcome = document.getElementById("welcome");
    const conversation = document.getElementById("conversation");
    let currentModel = "OmniChat";
    let compareMode = false;

    let currentGroup = "";
    models.forEach((model) => {
      if (model.group !== currentGroup) {
        currentGroup = model.group;
        const label = document.createElement("div");
        label.className = "dropdown-label";
        label.textContent = currentGroup;
        dropdown.appendChild(label);
      }
      const option = document.createElement("button");
      option.className = "model-option";
      option.type = "button";
      option.innerHTML = `<strong>${model.name}</strong><span>${model.detail}</span>`;
      option.addEventListener("click", () => {
        currentModel = model.name;
        selectedModel.textContent = model.name;
        promptInput.placeholder = `Message ${model.name}`;
        dropdown.classList.remove("open");
        modelButton.setAttribute("aria-expanded", "false");
      });
      dropdown.appendChild(option);
    });

    modelButton.addEventListener("click", () => {
      const open = dropdown.classList.toggle("open");
      modelButton.setAttribute("aria-expanded", String(open));
    });

    document.addEventListener("click", (event) => {
      if (!event.target.closest(".model-wrap")) {
        dropdown.classList.remove("open");
        modelButton.setAttribute("aria-expanded", "false");
      }
    });

    promptInput.addEventListener("input", () => {
      sendButton.disabled = !promptInput.value.trim();
      promptInput.style.height = "auto";
      promptInput.style.height = Math.min(promptInput.scrollHeight, 180) + "px";
    });

    promptInput.addEventListener("keydown", (event) => {
      if (event.key === "Enter" && !event.shiftKey) {
        event.preventDefault();
        if (promptInput.value.trim()) document.getElementById("chatForm").requestSubmit();
      }
    });

    function addMessage(role, text, modelName = "") {
      const row = document.createElement("div");
      row.className = `message ${role}`;
      const avatar = document.createElement("div");
      avatar.className = "avatar";
      avatar.textContent = role === "user" ? "I" : "✳";
      const body = document.createElement("div");
      body.className = "message-body";
      const label = document.createElement("div");
      label.className = "message-role";
      label.textContent = role === "user" ? "You" : (modelName || currentModel);
      const content = document.createElement("div");
      content.className = "message-text";
      content.textContent = text;
      body.append(label, content);
      row.append(avatar, body);
      messages.appendChild(row);
      conversation.scrollTop = conversation.scrollHeight;
    }

    document.getElementById("chatForm").addEventListener("submit", (event) => {
      event.preventDefault();
      const text = promptInput.value.trim();
      if (!text) return;

      welcome.style.display = "none";
      addMessage("user", text);
      promptInput.value = "";
      promptInput.style.height = "auto";
      sendButton.disabled = true;

      const answer = compareMode
        ? "Comparison mode is a UI demo. Connect your server to the providers you want to compare and show their responses here."
        : `This is a front-end demo for ${currentModel}. Connect a secure backend API to receive real AI responses. Your message was: “${text}”`;

      window.setTimeout(() => addMessage("assistant", answer), 350);

      const historyItem = document.createElement("button");
      historyItem.className = "chat-link";
      historyItem.textContent = text.length > 30 ? text.slice(0, 30) + "…" : text;
      document.getElementById("history").prepend(historyItem);
    });

    document.querySelectorAll(".suggestion").forEach((button) => {
      button.addEventListener("click", () => {
        promptInput.value = button.dataset.prompt;
        promptInput.dispatchEvent(new Event("input"));
        promptInput.focus();
      });
    });

    document.getElementById("newChat").addEventListener("click", () => {
      messages.replaceChildren();
      welcome.style.display = "";
      promptInput.value = "";
      promptInput.dispatchEvent(new Event("input"));
      promptInput.focus();
    });

    document.getElementById("compareButton").addEventListener("click", (event) => {
      compareMode = !compareMode;
      event.currentTarget.textContent = compareMode ? "Compare: on" : "Compare models";
      event.currentTarget.style.borderColor = compareMode ? "var(--accent)" : "";
    });

    document.getElementById("shareButton").addEventListener("click", async () => {
      try {
        await navigator.clipboard.writeText(location.href);
        alert("Page link copied.");
      } catch {
        alert("Copy the page URL from your browser to share it.");
      }
    });

    document.getElementById("attachButton").addEventListener("click", () => {
      document.getElementById("fileInput").click();
    });

    document.getElementById("webButton").addEventListener("click", (event) => {
      event.currentTarget.classList.toggle("active");
      event.currentTarget.style.borderColor =
        event.currentTarget.classList.contains("active") ? "var(--accent)" : "";
    });
  </script>
</body>
</html>
