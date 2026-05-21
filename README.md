
# 🚀 Multimodal WhatsApp Orchestrator Agent 

An advanced, production-ready **n8n automation workflow** that transforms WhatsApp into a powerful command center. By leveraging **LangChain, OpenAI (GPT-4o-mini), and a multi-agent orchestration architecture**, this system processes multimodal inputs (**Text, Voice, and Images**) to execute complex tasks across external platforms like Google Calendar, Gmail, and the web.

---

## 🧠 System Architecture Overview

The project uses a **Hub-and-Spoke Orchestration Model**. Instead of a single rigid script, a central "Parent Agent" acts as a router, handling the conversation logic and delegating tasks to dedicated sub-agent tools.

```
                  [ WhatsApp Trigger ]
                           │
                    [ Router/Switch ]
             ┌─────────────┼─────────────┐
          [ Text ]      [ Voice ]     [ Image ]
             │             │             │
             │       (Whisper API)  (GPT-4o-mini)
             └─────────────┬─────────────┘
                           ▼
                [ Orchestrator Agent ]
             ┌─────────────┼─────────────┐
             ▼             ▼             ▼
       [ Gmail Agent ] [ Calendar ] [ Web Search ]
             │             │             │
             └─────────────┬─────────────┘
                           ▼
                  [ Dynamic Response ]
             ┌─────────────┴─────────────┐
             ▼                           ▼
      (Text Message)              (Audio/Voice Reply)
```

---

## ✨ Key Features

### 1. Multimodal Ingestion Pipeline
* **Text Processing:** Extracts raw textual data directly for immediate analysis.
* **Voice/Audio Processing:** Downloads incoming WhatsApp voice notes, sends them to OpenAI's Whisper API for high-accuracy speech-to-text transcription, and passes the context downstream.
* **Image Analysis:** Captures incoming images, uses **GPT-4o-mini** to visually analyze the content, pairs it with user captions, and prepares it for agent delegation.

### 2. Intelligent Parent Orchestration
Driven by an LangChain Agent using a `toolThink` node, the central orchestrator evaluates user intent and selects the optimal path.
* Maintains the conversational context.
* Maintains a friendly, casual tone ideal for a messaging application interface.
* Guarantees structured outcomes from messy, conversational messaging payloads.

### 3. Sub-Agent Network (Tools)
* 📧 **Gmail Sub-Agent:** Fully capable of checking inbox status (`Get many e-mails`), sorting (`Mark as read/unread`), drafting/sending communications (`Send a message`), or instantly cleaning clutter (`Delete an e-mail`).
* 📅 **Google Calendar Sub-Agent:** Manages schedules natively via explicit operations (`Create`, `Update`, `Get`, `Get Many`, and `Delete` events) with built-in scheduling conflict awareness.
* 🌐 **Web Search Sub-Agent:** Connects the system to live information streams using **SerpApi (Google Search)** for current trends, **Wikipedia** for factual entries, and a custom **Hacker News** scraper for engineering updates.
* 🐦 **Social Media Integration (X/Twitter):** Drafts and queues micro-blogging content directly from a casual text loop or voice command.

### 4. Smart Bi-Directional Output
The system evaluates how the user communicated with it to formulate the response type:
* If the user sent a text or image, it sends a structured textual response.
* If the user sent a voice memo, the agent processes the response text through an OpenAI Text-to-Speech (TTS) engine (`shimmer` voice profile), converting the output back into an `.opus` audio file delivered straight to their chat thread.

---

## 🛠️ Technology Stack

* **Workflow Engine:** n8n (Advanced AI & LangChain integration layers)
* **AI Framework:** LangChain (Agents, Tools, Think Nodes)
* **LLM Providers:** OpenAI (GPT-4o-mini for Vision & Core Logic)
* **Audio Engines:** OpenAI Whisper (Speech-to-Text) & OpenAI TTS (Text-to-Speech)
* **Third-Party APIs:** WhatsApp Business API, Google Calendar API, Gmail API, SerpApi (Google Search)

---

## 📋 Example Use Cases

* **Voice-Driven Scheduling:** *User sends a voice note: "Hey, book a project review with Sarah tomorrow at 3 PM."* ➡️ **System Response:** Processes audio, validates tomorrow's date context, hits Google Calendar API, and drops a text confirmation back.
* **Visual Documentation:** *User sends a snapshot of a receipt or business card with the caption "Email this to accounting."* ➡️ **System Response:** Visual agent extracts textual data from the image, writes a structured breakdown, and forwards it through Gmail.
