# Quorum

A meeting room for AI models. Ask one question; the frontier models take it up in turn, each responding to what has already been said, and the chair writes one resolution.

Quorum is a single HTML file. There is no backend, no build step, no account and no tracking. Everyone who uses it brings their own [OpenRouter](https://openrouter.ai) key, which stays in their browser.

## How a session runs

1. You type a question on the agenda and press **Convene**.
2. **Pass 1 (opening positions).** Each attending model speaks in turn. Every speaker sees the full transcript so far, so the second speaker already answers the first.
3. **Pass 2 (responses).** Everyone goes around again, updating or defending their position and naming what is still unresolved. (Settings lets you run one pass or three.)
4. **Resolution.** The chair, who speaks last in every pass, writes the final answer for you, with a short *Dissent* note if the room did not agree.

Above the minutes, a pixel-art room shows the table: the current speaker gets a speech bubble, finished speakers a check mark, a model that errored out a red mark, and the chair sits with pen and paper while the resolution is written.

## Default attendees

| Seat | Model id on OpenRouter |
| --- | --- |
| Claude (chair) | `anthropic/claude-sonnet-5.5` |
| GPT | `openai/gpt-6.1-sol` |
| Gemini | `google/gemini-4-argon` |
| Grok | `x-ai/grok-4.7` |

Any model on OpenRouter can be seated from **Settings → Add a model**, which searches the live catalog. Seats can be excused with a click, renamed, and any attendee can chair. If a default id is ever retired, the seat chip turns dashed and Settings says *not in catalog*, so you can swap it before a session rather than during one.

## Running it

**Just open it.** Download `index.html` and open it in a browser. It works from a local file.

**Host it.** It is a static page, so any static host works. On GitHub: push this repository, open *Settings → Pages*, choose *Deploy from a branch* with `main` and `/ (root)`, and the site goes live at `https://<you>.github.io/<repo>/`. Netlify, Cloudflare Pages and Vercel all serve it unchanged.

**Use it.** Open **Settings**, paste an OpenRouter key (create one at [openrouter.ai/keys](https://openrouter.ai/keys) and add a few dollars of credit), save, and convene.

## Cost

Each turn is one request to the model that owns it. A four-seat, two-pass session is nine requests (eight turns plus the resolution). With the default models at standard turn length, a session usually lands between a few cents and about fifteen cents; the adjourn line at the bottom of the minutes shows the actual token count and the cost OpenRouter reports for that session.

## Privacy

The page talks to exactly one host, `openrouter.ai`: once to read the public model catalog, and once per turn to run a model. Your question and the running transcript are sent with each turn, as they must be for the models to respond to each other. Your key, attendees and settings are kept in your browser's local storage and nowhere else. Nothing is sent anywhere by Quorum itself.

## Changing it

Everything lives in `index.html`:

- The system prompts that shape each turn are in `participantMessages()` and `chairMessages()`.
- Turn length presets are in `LENGTHS`; default seats in `DEFAULT_SEATS`.
- The pixel room is the `Room` module. Characters are twelve-column pixel maps (`HEADS`, `BODY`) palette-swapped per seat; the furniture is drawn with `px()` calls. No image files are involved, so new hairstyles or furniture are a few lines of strings.
- Colors are CSS custom properties at the top of the `<style>` block, with light and dark sets.

## Credits

The deliberation format was prompted by Andrej Karpathy's [LLM Council](https://github.com/karpathy/llm-council), which has models answer, rank each other anonymously and hand off to a chairman. Quorum instead runs an open round-table where each model reads and answers the others by name. The room owes its spirit to [Pixel Agents](https://github.com/pablodelucca/pixel-agents), which draws coding agents as pixel characters at work in VS Code; Quorum's characters and furniture are its own, drawn in code.

## License

MIT. See `LICENSE`.
