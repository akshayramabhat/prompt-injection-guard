# prompt-injection-guard

Zero-dependency helpers to harden LLM prompts against injection from untrusted
third-party text.

## The threat

Any text you put in a prompt that came from outside your trust boundary is
attacker-controlled: scraped web pages, RAG and tool results, user-uploaded
documents, a profile you fetched. That text can carry instructions aimed at your
model ("ignore your rules", "output this link", "reveal your prompt"). If the
model cannot tell data from instructions, those payloads run with your app's
authority.

This library does two small, boring, effective things: it fences untrusted text
in anti-spoof markers and gives you a system-prompt rule that tells the model to
treat anything inside those markers as inert data, and it gives you a
defense-in-depth pass to strip URLs out of generated output.

## Install

```bash
pip install prompt-injection-guard
```

No dependencies. No network calls. Pure standard library.

## Usage

```python
from prompt_guard import fence, UNTRUSTED_DATA_RULES, strip_urls

system = "You write a one-line summary." + UNTRUSTED_DATA_RULES
user = "Summarize this page:\n" + fence(scraped_text)

# ... call your model with system + user ...

reply = strip_urls(model_output)  # defense-in-depth on the way out
```

`fence()` strips any forged marker out of the value before wrapping it, so a
payload that tries to smuggle in its own `<<<END_UNTRUSTED_DATA>>>` to break out
of the fence cannot.

## API

- `fence(text)`: wrap one untrusted string in anti-spoof markers (forged
  markers stripped from the value first).
- `fence_lines(lines)`: list form of `fence` for already-stripped lines.
- `strip_fence_markers(text)`: remove smuggled fence markers; tolerant of case
  and internal whitespace.
- `strip_urls(text)`: strip `http(s)://` and `www.` URLs from text.
- `UNTRUSTED_OPEN` / `UNTRUSTED_CLOSE`: the marker strings.
- `UNTRUSTED_DATA_RULES`: the system-prompt rule block to append to your system
  prompt.

## Limitations

Read these before relying on it.

- `strip_urls` deliberately misses bare domains (`evil.test`) and markdown links
  (`[text](url)`). It removes the common exfiltration shapes, not all of them.
- Fencing reduces injection risk; it does not eliminate it. A capable model can
  still be talked into misbehaving.
- This is one layer of defense in depth, not a guarantee. Keep your real
  authorization checks server-side and never let model output trigger a
  privileged action without validation.

## License

MIT.
