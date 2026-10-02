# Bitwise

One 64-bit buffer, many interpretations — flip bits and watch every view update live.

Bitwise is an interactive explorer for how the same 8 bytes read as integers, floating-point numbers, text, variable-width encodings and colors. Every view reads from and writes to a single shared `ArrayBuffer`, so editing any one of them immediately updates all the others.

## Running it

Open `index.html` in a browser. It's a single self-contained file — no build step, dependencies or server.

## Views

- **Integers** — `i32`/`u32` and `i64`/`u64`, with a two's-complement breakdown. Accepts decimal, `0x…`, `0b…` and `0o…`.
- **Floating point** — IEEE 754 `f32` and `f64`, plus the 16-bit formats `f16` (half precision) and `bf16` (bfloat16): sign, exponent and mantissa, the formula worked through step by step, and the exact stored decimal value. Handles zero, subnormals, infinities and quiet/signaling NaNs.
- **Fixed point** — Q16.16 (games and graphics) and Q1.15 (audio/DSP): an integer with an implied binary point, split into sign, integer and fraction bits.
- **Text** — ASCII, UTF-8 and UTF-16 (with surrogate pairs), with invalid sequences flagged and non-printable bytes shown as symbols or escapes, plus Base64 with the 8-bit → 6-bit regrouping laid out.
- **Protobuf varint** — a byte-by-byte decode of the 7-bit groups, plus the ZigZag (`sint64`) reading.
- **Time** — Unix timestamps: `i32` seconds (with the Year 2038 rollover) and `i64` milliseconds as used by JavaScript `Date`, plus the same `i64` read as seconds, microseconds and nanoseconds.
- **Network** — an IPv4 address (with its address class, and what happens if you read it as a native integer without `ntohl`) and a 48-bit MAC address (vendor prefix plus the multicast and locally-administered flag bits).
- **Bit fields** — a Unix file mode (`st_mode`): file type, setuid/setgid/sticky and owner/group/other `rwx`, shown as `ls -l` output and `chmod` octal, with clickable permission bits; a 32-bit FAT / ZIP (MS-DOS) date and time; a RISC-V (RV32IM) instruction you can disassemble or assemble, with each field of its R/I/S/B/U/J format broken out; and a 64-bit Snowflake ID (as used by Twitter / X and Discord) split into timestamp, machine and sequence fields.
- **Color** — two 32-bit RGBA pixels (with RGBA / ARGB / ABGR channel order), one 64-bit pixel (RGBA16 or half-float RGBA16F), and four RGB565 pixels.
- **Bit stats & tricks** — popcount, leading/trailing zeros, highest set bit and power-of-two checks for `u32` and `u64`, plus classic tricks (`x & (x − 1)`, `x & −x`, rotates, bit reverse, byte swap) you can apply to the buffer.

## Using it

- **Bit grid** — click a bit to toggle it, or press and drag to paint several. The tabs choose which view colors the bits (sign/exponent/mantissa, channels, and so on).
- **Byte order** — view the bytes big-endian (most significant byte first, as in network byte order) or little-endian.
- **Hover** a field such as "Exponent" or a color channel to isolate its bits in the grid.
- **Focused / All** — show only the cards for the current view, or everything.
- **Toolbar** — edit the raw bytes as hex, apply operations (NOT, shifts, ±1, byte swap, random) or load a preset such as `INT64_MIN`, π, a signaling NaN, a truncated varint or canvas pixel bytes.

## How it works

The page creates one 8-byte `ArrayBuffer` and layers typed-array views over it (`Uint8Array`, `Int32Array`, `Float64Array`, `BigInt64Array` and others). Typed arrays use the machine's native byte order — little-endian on practically all hardware — which is why the byte-order switch matters. 64-bit integers use `BigInt`; the varint, UTF-8 and 16-bit float (f16/bf16) codecs are implemented by hand.

## License

[MIT](LICENSE)
