# Bitwise

One 64-bit buffer, many interpretations. Flip bits and watch every view update live.

Bitwise shows how the same 8 bytes read as integers, floats, text, timestamps, addresses, machine code and colors. Every view reads from one shared `ArrayBuffer`. When you change any view, all the others update at once.

**Try it:** https://dfings.github.io/bitwise/

## Running it

Open `index.html` in a browser. There is nothing to install or build, and no server.

## Views

- **Integers.** Signed and unsigned, 32 and 64 bits, with the two's-complement math shown. Type values in decimal, `0x…`, `0b…` or `0o…`.
- **Floating point.** `f64`, `f32`, `f16` and `bf16`. Each one is split into sign, exponent and mantissa, with the formula worked out step by step and the exact stored value. Zero, subnormals, infinity and both kinds of NaN are labeled.
- **Fixed point.** Q16.16 (old 3D games) and Q1.15 (audio). An integer with an implied binary point.
- **Text.** ASCII, UTF-8, UTF-16 and Base64. Invalid bytes and unpaired surrogates are flagged.
- **Protobuf varint.** The 7-bit groups decoded byte by byte, plus the ZigZag reading.
- **Time.** Unix seconds as `i32`, which runs out in 2038, and milliseconds as `i64`, as in JavaScript `Date`.
- **Network.** An IPv4 address and a MAC address. Shows what goes wrong if you forget `ntohl`.
- **Bit fields.** A Unix file mode, a FAT/ZIP timestamp, a RISC-V instruction and a Snowflake ID. Each field has its own color. You can type RISC-V assembly and see the machine code.
- **Color.** RGBA, RGBA16 and RGB565 pixels.
- **Tricks.** Popcount, leading and trailing zeros, and classic bit hacks you can apply to the buffer.

## Using it

**Flip a bit**
1. Click a bit in the grid.
   *Result:* the bit flips, and every view updates.
2. To flip several bits, press and drag across them.

**Pick a view**
1. Click a tab, such as **f32** or **Fields**.
   *Result:* the grid colors the bits by that view's fields, and the page scrolls to its cards.
2. Hover over a field, such as **Exponent**.
   *Result:* the grid fades every bit outside that field.

**Change the byte order**
1. Click **Big-endian** or **Little-endian**.
   *Result:* the grid and hex row show the bytes in that order. Big-endian is network byte order.

**Edit the bytes directly**
1. Type hex into the **Bytes** field, or use a toolbar button such as **NOT**, **≪ 1** or **Byte swap**.
   *Result:* every view updates.

**Show one view at a time**
1. Click **Focused**. Click **All** to see everything again.
   Phones start in Focused.

**Load an example**
1. Open **Presets** and pick one, such as `INT64_MIN`, a signaling NaN, the Y2038 rollover or `ret`.

## How it works

The page makes one 8-byte `ArrayBuffer` and reads it through typed arrays (`Uint8Array`, `Float64Array`, `BigInt64Array` and others). Typed arrays use your machine's byte order, which is little-endian on almost all hardware. That is why the byte-order switch matters.

The varint, UTF-8, UTF-16, Base64, 16-bit float and RISC-V codecs are written by hand.

## License

[MIT](LICENSE)
