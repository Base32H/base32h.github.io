---
layout: default
title: Comparisons
subtitle: How does Base32H compare to other number systems?
---
* toc
{:toc}

## Hexadecimal (16)

Base-16 numbers are convenient for their readability (no case-sensitivity,
visual ambiguity, or punctuation), and are widely used, universally supported,
and well-understood.  They also map cleanly to binary data that's aligned on
8-bit bytes (which would be nearly all binary data in modern times).

However, base-16 has poor data density compared to base-32 systems like Base32H.
Further, relatively few English words can serve as base-16 numbers (`DEAD`,
`BEEF`, `CAFE`, and `BABE` being the typical examples), whereas practically any
English word can be a Base32H number (some people prefer their beef to be
*live*, after all!).

## Duotrigesimal (32)

There are quite a few other base-32 number systems out there, Base32H being the
newest on the block.  All base-32 systems represent 5 bits of data per digit;
the whole number of bits per digit is convenient for encoding binary data, and
chunking into 8-digit / 40-bit segments bodes well for such a system when
representing byte-aligned data.

### RFC 4648

[RFC 4648's base-32](https://tools.ietf.org/html/rfc4648) is the most common
base-32 number system in modern use (at least until Base32H catches on 😉), and
(as one might guess from the name) is an IETF protocol.  It's simple and
includes all letters on a US keyboard; however, it excludes digits 0, 1, 8, and
9, and is somewhat out-of-order relative to, say, octal or hexadecimal as a
result (for example: the RFC 4648 digit `3` has a value of 27, whereas Base32H's
`3` is simply 3).  RFC 4648 also uses `=` as a pad digit, in contrast with
Base32H which does not define a pad digit.

### z-base-32

Zooko Wilcox-O'Hearn's
[z-base-32](http://philzimmermann.com/docs/human-oriented-base-32-encoding.txt)
is used in both his [Tahoe-LAFS](https://tahoe-lafs.org/trac/tahoe-lafs) and
Phil Zimmermann's ZRTP (a.k.a. [RFC 6189](https://tools.ietf.org/html/rfc6189)).
Like Base32H, it's explicitly designed for easy human use, and it lacks pad or
check digits.  Unlike Base32H (and like RFC 4648), it's unable to accept certain
alphanumeric characters as digits (specifically: `l`, `v`, and `2`).  z-base-32
is unique in both its "jumbled" mapping between values and digits (the point
being to make easier characters more frequent) and using lowercase digits
(instead of uppercase-only like RFC 4648, or uppercase-by-default like
Crockford's and Base32H).

### Crockford's Base32

[Douglas Crockford's Base32](https://www.crockford.com/base32.html) is
remarkably similar to Base32H, and Base32H does indeed take heavy inspiration
from it.  Like Base32H, Crockford's is much more permissive about input
characters, aliasing `O`/`o` to `0` and `I`/`i` to `1`, and generally permitting
lowercase or uppercase letters.

There are several key differences between Crockford's and Base32H, however:

* Crockford's excludes the letter `U`/`u` from the regular digit set, as an
  attempt to reduce the likelihood of accidental generation of profanity;
  Base32H instead aliases this to `V` (thus achieving the same protection while
  still allowing words containing the letter U to be decoded)

* Crockford's aliases the letter `L`/`l` to `1`, for the same reason that it and
  Base32H both do so for `I`/`i`; Base32H, in contrast, maintains `L`/`l` as its
  own digit (old versions of Base32H's reference implementation and spec treated
  `L` and `l` separately, but this was more confusing than helpful, so Base32H's
  current assumption is that encoders will always output the uppercase canonical
  character, thus reducing if not outright eliminating the likelihood of a human
  encountering `l`)

* Crockford's maintains `S`/`s` as its own digit, while Base32H aliases it to
  `5` (both to make room for `L`/`l` and to address potential `5` v. `S`
  ambiguities)

* Crockford's includes check digits (`*`, `~`, `$`, `=`, `U`/`u`); Base32H does
  not, but an application can capitalize on the assumption that non-alphanumeric
  digits are "ignored" and therefore use Crockford's digits (using some other
  URL-safe character instead of `U`, e.g. `.`) or -- better yet -- can
  incorporate an error correction code (e.g. Hamming or Reed-Solomon) in the
  number itself (Base32H numbers are just numbers, after all)

### "Double hex"

Christian Lanctot proposed a base-32 system called "double hex" in [his letter
to *Dr. Dobb's* magazine](https://www.drdobbs.com/letters/184410894) in March
1999 as a Y2K solution; an identical system later found its way into [RFC
2938](https://tools.ietf.org/html/rfc2938#section-3.1.2).  Being a "pure"
extension of hexadecimal from 16 to 32 digits, the numbers 0-9 map directly to
their values, just like with Base32H and Crockford's Base32.  However, this
comes at a cost of visual conflicts between `0` v. `O`, `1` v. `I`, and `5`
v. `S`, and further makes the letters U-Z inaccessible for decoding purposes.

## Hexatrigesimal (36)

Base-36 is nearly always implemented as the combination of all Arabic numerals
0-9 and Latin letters A-Z.  Like most base-32 systems (including Base32H),
base-36 systems are case-insensitive and punctuation-free.  However, the
inclusion of all numerals and letters means that visually-ambiguous digits
exist, hampering readability.

## Octoquinquagesimal (58)

[Bitcoin](https://en.bitcoin.it/wiki/Technical_background_of_version_1_Bitcoin_addresses),
[IPFS](https://github.com/multiformats/cid), and
[Flickr](https://www.flickr.com/services/api/misc.urls.html#short) use variants
of a base-58 digit set (usually simply called "Base58").  Like most base-32
systems, all known base-58 systems exclude some visually-ambiguous characters
(namely `0`, `O`, `I`, and `l`) and punctuation.  However, they are still
case-sensitive, making them poorly suited for filenames or speech.

## Tetrasexagesimal (64)

In general, base-64 number systems offer better information density than base-32
systems.  They're also far more widely supported than any base-32 encoding
(Base32H included).  However, this typically comes at the cost of readability
and audibility.

### Base64 (RFC 989 and descendants)

What most people think of when they hear or read "Base64" has a long history
with many different subtle variations in various IETF RFCs:

* [RFC 989](https://tools.ietf.org/html/rfc989)
* [RFC 1421](https://tools.ietf.org/html/rfc1421)
* [RFC 1642](https://tools.ietf.org/html/rfc1642) ("Set B")
* [RFC 2045](https://tools.ietf.org/html/rfc2045)
* [RFC 2152](https://tools.ietf.org/html/rfc2152) ("Set B")
* [RFC 3548](https://tools.ietf.org/html/rfc3548)
* [RFC 4648](https://tools.ietf.org/html/rfc4648)
* [RFC 4880](https://tools.ietf.org/html/rfc4880) ("Radix-64")

All of them share the same digit set, and primarily differ on things like
padding characters and line lengths.  Having the same digit set, they all have
three key disadvantages relative to e.g. Base32H:

1. Case sensitivity, which makes them impractical for reading aloud, e.g. over
   the phone ("Was that an uppercase J?"), as well as unsuitable for filenames
   on case-insensitive filesystems (e.g. FAT, NTFS, HPFS by default)

2. Ambiguous-looking characters ("Is this an O or a 0?")

3. The use of `+`, `/`, and `=`, which is a pain point for systems which treat
   these as special characters

### Bash base-64 literals, crypt, bcrypt, xxencode, GEDCOM 5.5

All of these (and other similar base-64 encoding schemes) only mildly differ
from Base64, and have all of Base64's downsides as described above.

### uuencode

[`uuencode`](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/uuencode.html)
is somewhat unique among base-64 systems in that it excludes lowercase letters
by default, eliminating the need to have to differentiate between lowercase and
uppercase when reading a uuencoded number aloud.  However, it compensates for
this by using a considerable number of punctuation characters, including space,
making it unsuitable for filenames/URLs and making it somewhat clunky to read
out loud.  It also includes ambiguous-looking characters.

### BinHex

[BinHex 4](https://files.stairways.com/other/binhex-40-specs-info.txt), like
Base64, uses a mix of uppercase and lowercase letters, and like `uuencode` it
includes a hefty amount of punctuation.  However, it does exclude some
visually-ambiguous characters (namely `7`, `O`, `g`, and `o`), giving it an
advantage against other base-64 encodings on that front.

## Pentoctogesimal (85) and beyond

There are a couple base-85 systems, like
[Ascii85](http://www.gelato.unsw.edu.au/archives/git/0605/19975.html) and
[Z85](https://rfc.zeromq.org/spec/32/).  All of these, by their nature, are more
data-dense than even base-64 (let alone base-32, and therefore Base32H).
However, they also have the same usual problems with case-sensitivity,
visually-ambiguous characters, and egregious use of punctuation characters.

[basE91](http://base91.sourceforge.net), a base-91 system, has the same
tradeoffs.
