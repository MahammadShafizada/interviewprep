# VERSION 1 — Digital Notes (fuller)
**Session: Number systems + color representation**

Two separate topics in this material → split below.

---

# TOPIC A — Colors in Computers

## 1. Big Picture
- Screens only have R/G/B lights + computers only store 0/1 → need a scheme to turn "purple" into numbers.
- Answer: 3 lights × intensity numbers, written in hex so humans can type it.

## 2. Core Concepts
- **Bit** — single 0 or 1 (short for *binary digit*); smallest unit, everything else is just piles of these.
- **Additive RGB mixing** — red + green + blue *lights* stacked; matters because computer color = 3 knobs, not paint mixing.
- **8-color palette (3 bits)** — 1 bit per light, on/off only → 2×2×2 = 8 colors; shows the "bits = states" idea at tiny scale. (000 black, 111 white, 100 red, 011 cyan…)
- **Byte / octet** — 8 bits = 256 possible values (0–255); the standard chunk size, one per color channel.
- **24-bit / true color** — 3 bytes (R,G,B), 256×256×256 = 16,777,216 colors; "16 million colors" = just this math.
- **Hex color code** — 6 hex digits = 3 bytes, e.g. `A3EA2A`; exists purely because `10100011 11101010 00101010` is unusable by hand.
- **4 bits ↔ 1 hex digit** — so 1 byte = exactly 2 hex digits; the reason hex lines up so neatly with binary.

## 3. Terms That Get Confused
- **Bit vs Byte** — bit = 1 digit, byte = 8 of them. "b" small = bit, "B" big = Byte.
- **8 colors vs 16 million** — 3 bits (on/off per light) vs 24 bits (256 levels per light). Same idea, more bits.
- **256 vs 255** — 256 *values*, counted 0–255. Zero takes a slot.
- **Additive (RGB) vs Subtractive (CMYK)** — light adds up to white; ink absorbs, piles up to black. Screens vs printers.
- **Hex digit vs byte** — 1 hex digit = 4 bits (a nibble), 2 hex digits = 1 byte. Never confuse `A3` (one byte) with `A` (half).
- **`#RRGGBB` order** — first pair = red, last = blue. Left-to-right = R,G,B, always.

## 4. Interview Drills
**Q: How does a computer store a color?**
So a color on screen is just three lights — red, green, and blue — and the computer stores how bright each one is. Each gets one byte, so 256 levels per light, which works out to about 16.7 million combinations. That's why colors get written as six hex digits, two per channel.

**Q: Why hexadecimal instead of just binary?**
Honestly it's for us, not the machine. One hex digit maps exactly to four bits, so a byte is always two hex digits and the conversion is mechanical. Writing `A3EA2A` is a lot easier than twenty-four ones and zeros.

**Q (pushback): If the computer only understands binary, isn't hex just fake?**
Yeah, pretty much — it's a display format. The hardware is still storing bits either way. Hex is a shorthand we use because it groups those bits into readable chunks without changing anything underneath.

**Q: Why exactly eight colors if each light is on or off?**
Each light has two states, and there are three of them, so it's 2 times 2 times 2, which is 8. That's the same reasoning behind 24-bit color — you just swap the two states for 256 levels each.

**Q (pushback): So more bits always means better color?**
It means more distinct values, which usually means smoother gradients and less banding. But past a point your eyes stop noticing, so 24-bit became the practical standard rather than the maximum possible.

## 5. Memory Anchors
- Bit → a light switch.
- RGB mixing → three dimmer knobs.
- 8-color palette → 3 switches, all-off = black, all-on = white.
- Byte → a carton of 8 eggs, 256 combos.
- 24-bit color → three knobs × 256 notches = 16 million.
- Hex color → the postcode for a color.
- 4 bits = 1 hex digit → hex is binary in bulk packaging.

## 6. Topic Tag
`IT Fundamentals → Data Representation → Color Encoding (RGB / hex)`

---

# TOPIC B — Number Systems (Binary, Hex, Octal)

## 1. Big Picture
- Humans count in 10s (ten fingers), hardware only has 2 states (voltage low/high).
- Need a way to write the same number in different bases, and convert between them.

## 2. Core Concepts
- **Base / radix** — how many digits a system uses before rolling over; decides everything else about the system.
- **Positional notation (place value)** — each position = digit × base^position, counting from 0 on the right; the single rule behind *all* base conversions. e.g. 213 = 2×10² + 1×10¹ + 3×10⁰.
- **Decimal (base-10)** — digits 0–9, powers of 10; our default, only a habit from finger-counting.
- **Binary (base-2)** — digits 0 and 1, powers of 2 (…8,4,2,1); matches physical hardware — transistor on/off, magnetic N/S, light present/absent.
- **Binary → decimal** — add up the place values where there's a 1. `1001` = 8+0+0+1 = 9. `1111` = 8+4+2+1 = 15.
- **Hexadecimal (base-16)** — digits 0–9 then A–F (A=10 … F=15); one digit = 4 bits, so it compresses binary cleanly. `9BDF` = 9×4096 + 11×256 + 13×16 + 15 = 39,903.
- **Octal (base-8)** — digits 0–7, one digit = 3 bits; rarer now, still shows up in Linux file permissions. `357` = 3×64 + 5×8 + 7 = 239.
- **Bit-grouping rule** — 3 bits → octal digit, 4 bits → hex digit; why those two bases exist at all rather than, say, base-12.

## 3. Terms That Get Confused
- **Base-2 vs base-16 vs base-8** — digits available: 2 / 16 / 8. Grouping: 1 / 4 / 3 bits.
- **`10` in which base?** — 10 binary = 2, 10 octal = 8, 10 hex = 16. "10" always means "one whole base."
- **Hex `A` vs letter A** — in hex it's a *number*, value 10, not text.
- **F vs 15 vs 1111** — same value, three notations. Max single hex digit.
- **Highest digit vs the base** — base-16 tops out at F (15), base-8 at 7. Digits stop one below the base.
- **Nibble vs byte** — 4 bits vs 8 bits; 1 hex digit vs 2.
- **Binary number vs binary file/data** — number = a value; "binary" casually = raw non-text data.

## 4. Interview Drills
**Q: Why do computers use binary?**
Because the hardware only really has two reliable states — a transistor either passes current or it doesn't, low voltage or high. Trying to squeeze ten distinct levels into that would be noisy and error-prone, so two states it is, and we call them 0 and 1.

**Q: Walk me through converting 1101 to decimal.**
Sure — you go right to left, and each position is a power of two: 1, 2, 4, 8. So there's a 1 in the 8s, a 1 in the 4s, nothing in the 2s, and a 1 in the 1s. Add those up and you get 13.

**Q (pushback): Do you have to do that math every time?**
In practice no — you end up memorising the common patterns, especially the 4-bit ones since they map straight to hex digits. But I keep the place-value method in my head because it works for any base, not just binary.

**Q: What's the relationship between binary and hexadecimal?**
One hex digit is exactly four bits, so conversion is just chunking rather than real arithmetic. That means a byte is always two hex digits, which is why hex shows up everywhere — memory addresses, colors, hashes, MAC addresses.

**Q: Where would you actually run into octal?**
Mostly Linux file permissions — something like `chmod 755`, where each digit is three bits for read, write, and execute. Outside of that it's fairly rare compared to hex.

**Q (pushback): Why does any of this matter for a security role?**
Because a lot of tooling shows you raw data in hex — packet captures, hex editors, memory dumps, hashes. If I can't read that, I'm just staring at noise instead of spotting what's actually in the bytes.

## 5. Memory Anchors
- Base → how many digits before you roll over.
- Positional notation → digit × base^position, count from 0 on the right.
- Decimal → ten fingers.
- Binary → light switch; hardware's native tongue.
- Binary→decimal → 8-4-2-1, add where there's a 1.
- Hex → binary in packs of 4; A–F are just 10–15 wearing letters.
- Octal → packs of 3; lives in `chmod`.

## 6. Topic Tag
`IT Fundamentals → Data Representation → Number Systems (base-2 / 8 / 10 / 16)`
