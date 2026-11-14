# Inputs for the audio divergence report

Each `.bin` is one input of a differential decoder target that decodes the same operations with `OpusDecoder` and
libopus 1.6.1 and compares the results after every step.

Format: byte 0 selects the decoder rate (`& 0x0f`, index into 8/12/16/24/48 kHz) and stereo (`& 0x10`). Then
operations follow: `op % 8 == 0` is a loss of `[2.5, 5, 10, 20, 40, 60][(op >> 3) % 6]` ms, `op % 8 == 1` is
`decode_fec` with that duration, and anything else decodes a packet into a 120 ms buffer. A packet is preceded by its
length (one byte, or two bytes when the high bit of the first is set).

`diff-replays-on-main.txt` (on `main`) and `diff-replays-on-work.txt` (on `main` with the open PRs merged) list every
step of every input with both decoders' sample counts, final ranges, the SNR against libopus's output clamped to
[-1, 1], and the peak level.
