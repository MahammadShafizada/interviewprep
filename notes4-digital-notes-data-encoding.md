# VERSION 1 — Digital Notes (fuller)
**Session: Data Encoding — ASCII, Unicode, UTF**

Splits into two topics: legacy encodings (ASCII + ISO-8859) and Unicode/UTF.

---

# TOPIC A — ASCII & Legacy Encodings

## 1. Big Picture
- Everything is stored as numbers → need an agreed map from number ↔ character.
- ASCII was that first agreement, but only for English.

## 2. Core Concepts
- **Encoding (vs representation)** — the *agreed mapping* number → meaning; representation is just "it's bits in memory," encoding is what those bits mean.
- **ASCII** — American Standard Code for Information Interchange, 1963, **7 bits → 128 codes**; covers English letters, digits, punctuation, control chars. Remember the **A = American** — that's the whole limitation in one word.
- **ASCII is ordered** — `A`=65 (0x41), `Z`=90 (0x5A), `a`=97 (0x61), `0`=48 (0x30); so you can derive neighbours instead of memorising the table.
- **Char → byte in practice** — "TryHackMe" on disk = `54 72 79 48 61 63 6b 4d 65 0a`; last `0a` = newline (`\n`) from pressing Enter.
- **Hex as the display format** — we read bytes as hex not binary/decimal; same bits, just readable (4 bits = 1 hex digit).
- **8th bit / extended ASCII** — adds 128 more slots, still nowhere near enough for all European letters.
- **ISO/IEC 8859 series** — regional 8-bit standards: **8859-1 (Latin-1)** = Western European (ß, ü, é, ç, ñ, ð); **8859-2 (Latin-2)** = Central/Eastern European (ł, ń, č, ř, ő, ș).
- **Mojibake / encoding mismatch** — save in one encoding, open in another → wrong characters; e.g. `Ø` in Latin-1 shows as `Ř` in Latin-2. Bytes never changed, the lookup table did.

## 3. Terms That Get Confused
- **Representation vs Encoding** — "it's bits" vs "what the bits *mean*."
- **Character set vs Encoding** — the list of characters + numbers vs how those numbers become bytes.
- **ASCII vs Extended ASCII** — 7-bit/128 (universal) vs 8-bit/256 (regional, inconsistent).
- **ISO-8859-1 vs 8859-2** — Western vs Central/Eastern Europe. 1 = France/Spain/Germany, 2 = Poland/Czech/Hungary.
- **Corrupted file vs wrong encoding** — gibberish ≠ damaged; usually the right bytes read with the wrong table.
- **`0` the character vs 0 the number** — character `0` = decimal 48 (0x30), not value 0.
- **Control character vs printable** — e.g. `0a` newline, `7F` DEL do things; letters display.

## 4. Interview Drills
**Q: What is ASCII?**
It's an encoding standard from the sixties that maps English characters to numbers, using seven bits so 128 possible characters. That covers letters, digits, punctuation and some control codes. The catch is the "A" stands for American — there's no room in it for anything outside English.

**Q: Why do you sometimes see garbled text in a file or subtitle?**
Usually it's an encoding mismatch. The file got saved with one encoding and opened with a different one, so the same bytes get looked up in the wrong table and you see the wrong characters. The file itself isn't damaged — it's being misread.

**Q (pushback): So could you recover that text?**
Usually yes, if you can work out the original encoding, since the bytes are intact. Most editors let you reopen a file with a specific encoding, so it's often just a case of trying the likely one for that language.

**Q: What's the difference between representation and encoding?**
Representation is just the fact that data sits in memory as bits and numbers. Encoding is the agreement about what those numbers actually mean — like the number 65 meaning capital A. Without that shared agreement, the bits are meaningless.

**Q: Why weren't the ISO-8859 standards a good long-term fix?**
They each only handled a region, so you had Latin-1 for Western Europe, Latin-2 for Central and Eastern, and so on. You couldn't mix languages in one document, and if the reader picked the wrong variant you got mojibake. It solved the symptom rather than the problem.

## 5. Memory Anchors
- Encoding → the shared dictionary.
- ASCII → **A for American** = English only.
- ASCII order → A=65, a=97, 0=48. Letters run in sequence.
- Hex view → bytes in human-readable clothing.
- Extended ASCII → 128 extra seats, still not enough.
- ISO-8859 → a different phrasebook per region.
- Mojibake → right bytes, wrong dictionary.

## 6. Topic Tag
`IT Fundamentals → Data Encoding → ASCII & Legacy 8-bit Encodings`

---

# TOPIC B — Unicode & UTF

## 1. Big Picture
- One universal table for every language, script, symbol and emoji.
- Kills the "which encoding did you use?" problem.

## 2. Core Concepts
- **Why ASCII broke** — Arabic 250+ chars, Japanese 6,879 (JIS X 0208), Chinese 87,887+ (GB 18030-2022); 256 slots was never going to work.
- **Unicode** — universal *character set*: one unique number (code point) per character, all modern + historical scripts. One table everyone shares → no encoding negotiation.
- **Code point** — written `U+hex`, e.g. `U+0041` A, `U+03A9` Ω, `U+3042` あ, `U+9F8D` 龍, `U+265E` ♞; the identity of a character, separate from how it's stored.
- **Unicode ≠ storage format** — Unicode says *which number*; UTF says *how to write that number in bytes*. Key distinction.
- **UTF-8** — variable, **1–4 bytes**; ASCII range (U+0000–U+007F) = 1 byte identical to ASCII → backward compatible; Ω = 2 bytes, 🔥 = 4. Dominant on the web.
- **UTF-16** — 2 or 4 bytes; common scripts in 2, rare/emoji need a surrogate pair (🔥 = U+D83D U+DD25).
- **UTF-32** — always 4 bytes; simplest to index, most wasteful (A = U+00000041).
- **Emoji are just code points** — 😊 = U+0001F60A, nothing special; Unicode 17.0 ≈ 157k chars, ~4k emoji sequences.

## 3. Terms That Get Confused
- **Unicode vs UTF** — the *catalogue* vs the *packing method*. Unicode = number, UTF = bytes.
- **Character set vs encoding** — same split as above; Unicode is the set, UTF-8 the encoding.
- **UTF-8 vs UTF-16 vs UTF-32** — 1–4 / 2 or 4 / always 4 bytes. Efficient → simple.
- **UTF-8 vs ASCII** — UTF-8 *contains* ASCII; plain English text is byte-identical in both.
- **Code point vs byte** — U+03A9 is one code point but two bytes in UTF-8. Never assume 1 char = 1 byte.
- **UTF-8 vs Latin-1** — universal vs Western-Europe-only; both 1 byte for plain English, diverge after.
- **Glyph vs character** — the drawn shape (font's job) vs the code point. Missing glyph = tofu box `□`, not a broken file.

## 4. Interview Drills
**Q: What's Unicode and why does it exist?**
Unicode is one universal table that gives every character in every language its own unique number. It exists because the old regional encodings couldn't cover everything and didn't play well together. Now you can mix Arabic, Japanese and emoji in one file without picking a special encoding.

**Q: What's the difference between Unicode and UTF-8?**
Unicode's the character set — it says capital A is code point U+0041. UTF-8 is an encoding, so it decides how you actually write that number out as bytes. One's the catalogue number, the other's the packaging.

**Q: Why is UTF-8 so dominant on the web?**
Mainly because it's variable width, so plain English text still takes one byte per character and nothing's wasted. On top of that it's backward compatible with ASCII, so old ASCII files are already valid UTF-8. You only spend extra bytes when you actually need them.

**Q (pushback): If UTF-8 is the efficient one, why does UTF-32 exist at all?**
Because it's predictable — every character is exactly four bytes, so jumping to the tenth character is simple arithmetic. That's handy for internal processing, but it roughly quadruples the size of English text, so you wouldn't send it over a network.

**Q: How does an emoji get stored?**
It's just a Unicode code point like any letter. The fire emoji is U+1F525, which happens to sit high enough in the range that UTF-8 needs four bytes for it. To the computer there's nothing special about it — the font is what draws the picture.

**Q (pushback): So if I see empty boxes instead of emoji, is that an encoding problem?**
Usually not — that's normally a font problem. The code point came through fine, the font just doesn't have a glyph to draw, so you get the placeholder box.

## 5. Memory Anchors
- Why ASCII broke → 256 seats, thousands of guests.
- Unicode → one global phonebook for characters.
- Code point → the character's passport number (U+...).
- Unicode vs UTF → catalogue number vs packaging.
- UTF-8 → elastic, 1–4 bytes, ASCII-compatible.
- UTF-16 → 2 or 4, emoji travel in pairs.
- UTF-32 → always 4, simple but bloated.
- Emoji → just another code point.

## 6. Topic Tag
`IT Fundamentals → Data Encoding → Unicode & UTF-8/16/32`
