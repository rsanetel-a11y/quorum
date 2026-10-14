# Quorum

A meeting room for AI models. Ask one question; the frontier models take it up in turn, each responding to what has already been said, and the chair writes one resolution.

Quorum is a single HTML file. There is no backend, no build step, no API keys for the default seats, and no per-token bill from Quorum: the frontier models run through [Puter](https://puter.com), where each person's own free monthly allowance covers their sessions. Other free tiers (Google Gemini, Groq, Mistral, a local Ollama) can be seated alongside them.

## How a session runs

1. You type a question on the agenda and press **Convene**. The first time, Puter asks you to sign in (or create a free account) in a popup.
2. **Pass 1 (opening positions).** Each attending model speaks in turn. Every speaker sees the full transcript so far, so the second speaker already answers the first.
3. **Pass 2 (responses).** Everyone goes around again, updating or defending their position and naming what is still unresolved. (Settings lets you run one pass or three.)
4. **Resolution.** The chair, who speaks last in every pass, writes the final answer for you, with a short *Dissent* note if the room did not agree.

Above the minutes, a pixel-art room shows the table: the current speaker gets a speech bubble, finished speakers a check mark, a model that errored out a red mark, and the chair sits with pen and paper while the resolution is written.

## Default attendees

| Seat | Model | Runs on |
| --- | --- | --- |
| Claude (chair) | `anthropic/claude-sonnet-5-5` | Puter |
| GPT | `openai/gpt-6.1-sol` | Puter |
| Gemini | `google/gemini-4-argon` | Puter |
| Grok | `x-ai/grok-4.7` | Puter |

Any of Puter's thousand-odd models can be seated from **Add seat**, which searches Puter's live catalog. Seats can be excused with a click, renamed, and any attendee can chair. If a default id is ever retired, the seat chip turns dashed and Settings says *not in catalog*, so you can swap it before a session rather than during one.

## Why Puter, and the free tiers

Anthropic and xAI publish no free API tier, and OpenAI's only free usage requires a paid account. Puter's *user-pays* model is the one route that gives Claude, GPT, Gemini and Grok to a browser app with no keys: the site pays nothing, and each visitor's Puter account carries a free monthly allowance (Puter prompts them to upgrade if they use it up). Quorum never sees anyone's credentials; the Puter script handles sign-in itself.

Seats can also run directly on services that have a real free tier with no card:

| Runs on | Free tier | Example seats |
| --- | --- | --- |
| Google Gemini | Free tier from [AI Studio](https://aistudio.google.com/apikey); Google may use free-tier traffic to improve its models | `gemini-3.8-flash`, `gemini-2.5-pro` |
| Groq | About 1,000 requests a day, [console.groq.com/keys](https://console.groq.com/keys) | `openai/gpt-oss-120b` (OpenAI's open-weight model), `qwen/qwen3.8-27b` |
| Mistral | Free mode, [console.mistral.ai](https://console.mistral.ai/api-keys) | `mistral-large-latest` |
| Ollama | Models on your own computer | whatever `ollama list` shows |
| Custom | Any OpenAI-compatible `/chat/completions` endpoint | — |

Keys for these go in **Settings** and stay in the browser. A seat from Google, Groq or Mistral needs its key; Puter seats need only the sign-in. GitHub Models' free tier is not offered because its API refuses browser requests.

## Running it

**Just open it.** Download `index.html` and open it in a browser. It works from a local file.

**Host it.** It is a static page, so any static host works. On GitHub: push this repository, open *Settings → Pages*, choose *Deploy from a branch* with `main` and `/ (root)`, and the site goes live at `https://<you>.github.io/<repo>/`. Netlify, Cloudflare Pages and Vercel all serve it unchanged.

For Ollama seats, start Ollama with the site's address allowed as an origin, for example `OLLAMA_ORIGINS=https://<you>.github.io ollama serve`, so the browser may reach `localhost:11434`.

## Privacy

The page talks to the services that run the seats you convene: `puter.com` for Puter seats, and Google, Groq, Mistral or your own endpoint for seats you add from those. Your question and the running transcript are sent with each turn, as they must be for the models to respond to each other. Keys and settings are kept in your browser's local storage and nowhere else. Nothing is sent anywhere by Quorum itself.

## Changing it

Everything lives in `index.html`:

- The services a seat can run on are in `PROVIDERS`; adding another OpenAI-compatible host is one entry.
- The system prompts that shape each turn are in `participantMessages()` and `chairMessages()`.
- Turn length presets are in `LENGTHS`; default seats in `DEFAULT_SEATS`.
- Streaming adapters are `streamPuter()`, `streamGoogle()` and `streamOpenAI()`.
- The pixel room is the `Room` module. Characters are twelve-column pixel maps (`HEADS`, `BODY`) palette-swapped per seat; the furniture is drawn with `px()` calls. No image files are involved, so new hairstyles or furniture are a few lines of strings.
- Colors are CSS custom properties at the top of the `<style>` block, with light and dark sets.

## Credits

The deliberation format was prompted by Andrej Karpathy's [LLM Council](https://github.com/karpathy/llm-council), which has models answer, rank each other anonymously and hand off to a chairman. Quorum instead runs an open round-table where each model reads and answers the others by name. The room owes its spirit to [Pixel Agents](https://github.com/pablodelucca/pixel-agents), which draws coding agents as pixel characters at work in VS Code; Quorum's characters and furniture are its own, drawn in code.

## License

MIT. See `LICENSE`.
