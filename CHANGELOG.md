# Changelog

## 0.1.0 - 2026-06-28

Initial release.

- `fence` and `fence_lines` to wrap untrusted text in anti-spoof markers.
- `strip_fence_markers` to remove smuggled fence markers (case and
  whitespace tolerant).
- `strip_urls` for defense-in-depth output scrubbing.
- `UNTRUSTED_DATA_RULES` system-prompt rule block, plus the
  `UNTRUSTED_OPEN` / `UNTRUSTED_CLOSE` marker constants.
