# 🌍 Trip Craft — AI Travel Planner

Trip Craft is an AI-powered day-trip planner that generates a personalized itinerary based on a destination city and your interests, then automatically attaches a clickable Google Maps link and a short "why it's famous" description to every stop.

Built with **Python, LangGraph-style state, LangChain, Groq (Llama), and Gradio**.

---

## ✨ Features

- Generates a full day-trip itinerary (breakfast, sightseeing, lunch, more activities, dinner) tailored to your interests.
- Every stop includes:
  - ⏰ Time and activity
  - ℹ️ A short description of why the place is notable
  - 📍 A clickable **Google Maps** link that opens in a new tab
- Structured JSON output from the LLM (not fragile text-parsing) for reliable, consistent formatting.
- Simple, clean Gradio web UI with example destinations built in.

---

## 🖼️ Example Output

```
Day 1

08:00 AM
Breakfast at Mylari Hotel
ℹ️ A legendary Mysore breakfast spot famous for its soft, ghee-roasted dosas.
📍 Open in Google Maps

10:00 AM
Visit Mysore Palace
ℹ️ A former royal residence known for its Indo-Saracenic architecture and
   nightly illumination of nearly 100,000 lights during Dasara.
📍 Open in Google Maps

12:30 PM
Lunch at RRR Restaurant
ℹ️ A popular local restaurant known for authentic Andhra-style thali meals.
📍 Open in Google Maps
```

---

## 🧰 Tech Stack

| Component      | Purpose                                      |
|----------------|-----------------------------------------------|
| Python         | Core language                                 |
| LangChain      | Prompt templating and LLM orchestration       |
| Groq (Llama)   | LLM inference (fast itinerary generation)     |
| Gradio         | Web UI                                        |
| Google Maps    | Location links via Maps Search URL API        |

---

## 📦 Requirements

```
gradio
langchain-core
langchain-groq
```

Install with:

```bash
pip install gradio langchain-core langchain-groq
```

---

## 🔑 Setup

1. Get a free API key from [Groq Console](https://console.groq.com).
2. Set it as an environment variable:

   **macOS / Linux**
   ```bash
   export GROQ_API_KEY="your_api_key_here"
   ```

   **Windows (PowerShell)**
   ```powershell
   $env:GROQ_API_KEY="your_api_key_here"
   ```

3. Run the app:

   ```bash
   python app.py
   ```

4. Open the local URL Gradio prints in your terminal (typically `http://127.0.0.1:7860`).

---

## 🕹️ Usage

1. Enter a **city** (e.g., `Mysore`, `Paris`, `Tokyo`).
2. Enter your **interests**, comma-separated (e.g., `history, food, architecture`).
3. Click **Generate Itinerary**.
4. Click any **📍 Open in Google Maps** link to open that location directly in a new browser tab.

---

## 🗂️ Project Structure

```
.
├── app.py          # Main application (planner logic + Gradio UI)
└── README.md
```

### Key functions in `app.py`

| Function                     | Responsibility                                                        |
|-------------------------------|------------------------------------------------------------------------|
| `input_city` / `input_interests` | Capture and store user inputs in planner state                    |
| `create_itinerary`            | Calls the LLM and produces the final rendered itinerary               |
| `parse_structured_itinerary`  | Safely parses the LLM's JSON response into a Python list               |
| `generate_maps_link`          | Builds a Google Maps Search URL for a given place (+ city disambiguation) |
| `format_itinerary_html`       | Renders stops as HTML with time, description, and Maps link            |
| `travel_planner`               | Top-level function wired to the Gradio submit button                  |

---

## ⚠️ Known Notes

- **Model deprecation:** `llama-3.3-70b-versatile` was deprecated by Groq (announced June 17, 2026). If you hit a `model_decommissioned` error, switch `model_name` in `app.py` to `openai/gpt-oss-120b` or `qwen/qwen3.6-27b`.
- The itinerary output is rendered via `gr.HTML` (not `gr.Textbox`), which is required for the Google Maps links to be clickable.
- If the LLM ever returns malformed JSON, the app falls back to a friendly "could not parse itinerary" message instead of crashing — just regenerate.

---

## 🚀 Possible Future Improvements

- Multi-day trip support with day-by-day tabs.
- Let users regenerate or edit individual stops.
- Add estimated travel time between consecutive stops.
- Cache/store generated itineraries for later retrieval.
