# Bitwise

One 64-bit buffer, many interpretations — flip bits and watch every view update live.

Bitwise is an interactive explorer for how the same 8 bytes read as integers, floating-point numbers, text, variable-width encodings and colors. Every view reads from and writes to a single shared `ArrayBuffer`, so editing any one of them immediately updates all the others.

## Running it

Open `index.html` in a browser. It's a single self-contained file — no build step, dependencies or server.

## Views

- **Integers** — `i32`/`u32` and `i64`/`u64`, with a two's-complement breakdown. Accepts decimal, `0x…`, `0b…` and `0o…`.
- **Floating point** — IEEE 754 `f32` and `f64`, plus the 16-bit formats `f16` (half precision) and `bf16` (bfloat16): sign, exponent and mantissa, the formula worked through step by step, and the exact stored decimal value. Handles zero, subnormals, infinities and quiet/signaling NaNs.
- **Text** — ASCII and UTF-8 side by side, with invalid sequences flagged and non-printable bytes shown as symbols or `\xNN` escapes.
- **Protobuf varint** — a byte-by-byte decode of the 7-bit groups, plus the ZigZag (`sint64`) reading.
- **Color** — two 32-bit RGBA pixels (with RGBA / ARGB / ABGR channel order), one 64-bit pixel (RGBA16 or half-float RGBA16F), and four RGB565 pixels.

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
