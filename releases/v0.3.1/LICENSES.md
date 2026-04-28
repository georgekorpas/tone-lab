# Tone Lab — Third-Party Licenses & Attribution

This document credits the upstream open-source projects whose designs
informed parts of Tone Lab. All Tone Lab code is original JavaScript.
Where an engine's algorithm or parameter mapping was studied from an
upstream open-source project, the upstream project is acknowledged
below and in an in-file attribution comment at the top of the engine's
JavaScript implementation.

---

## Mutable Instruments — Plaits

**Project:** Plaits (Eurorack macro-oscillator)
**Author:** Émilie Gillet
**Source:** https://github.com/pichenettes/eurorack/tree/master/plaits
**License:** MIT

The Tone Lab **PL** device is inspired by Plaits and uses Plaits as its
design reference for parameter ergonomics (MODEL / HARMONICS / TIMBRE /
MORPH macro layout, low-pass-gate-style envelope, model selector). The
JavaScript implementations of each PL model are original — not a literal
port of the Plaits C++ source — but the algorithmic intent and parameter
behavior is intentionally faithful so that PL feels familiar to Plaits
users.

The Plaits MIT license notice, reproduced verbatim as required by the
license:

```
Copyright 2016 Emilie Gillet.

Author: Emilie Gillet (emilie.o.gillet@gmail.com)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.

See http://creativecommons.org/licenses/MIT/ for more information.
```

PL models in Tone Lab carry an in-file header pointing back to the
specific Plaits engine that informed the design.

---
