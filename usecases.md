---
layout: default
title: Use Cases
subtitle: How might I use Base32H in my application?
---
Base32H has quite a few interesting use-cases that highlight its strengths in
real-world applications, especially in comparison to other number systems.

* toc
{:toc}

## Usernames

Because Base32H can readily convert between integers and words, it makes it
trivial to map, say, usernames to numeric IDs.  For example, the username
`YellowApple` decodes to 34292256792632974, which encodes back to
`YELL0WAPPLE` - convenient, eh?  An 8-character username handily maps to a
40-bit / 5-byte unsigned integer without any wasted padding bits, and a
32-character username (the usual limit for modern Unix-like operating systems)
fills 160 bits / 20 bytes; in either case (or other cases where username lengths
are a multiple of 8), a Base32H username bodes well for byte-aligned storage.

Importantly, unlike other base-32 number systems, *any* sequence of English
numerals and letters can represent a number, meaning that any number or letter
one might type on a US keyboard can be entered on login.  Better yet, since
compliant Base32H decoders will filter out any punctuation or other special
characters, it'll inherently sanitize username inputs (some
[XKCD-reading](https://xkcd.com/327/) trickster entering `Robert'); DROP TABLE
Students;--` would simply be known in your system as `R0BERTDR0PTABLE5TVDENT5`).

## Asset tags

Base32H was designed specifically with asset tags in mind; they're often subject
to fading and scratches and smudges and grime, making it hard to differentiate
between, say, `5`/`S` or `1`/`I` or `0`/`O`, and thus making it hard for users
to provide them over the phone when calling, say, a support technician.  Since
Base32H provides aliases for these lookalike characters, even if you read such a
number "wrong" it'll still decode just fine.

4 digits of Base32H are enough to represent 1,048,576 assets, and 8 digits would
be enough for 1,099,511,627,776.  Evidently, a Base32H-based asset ID will
readily scale from small startups to large enterprises without a hitch.

Since Base32H can decode arbitrary words, it's possible to encode an asset's
type or category in the identifier as well, making a "prefixed" identifier of
sorts.  For example, a workstation's tag might be `PC-123456`, which decodes to
`803194638502`, and encodes right back into `PC123456` (obviously it'll be up to
you to add the dash back in, since Base32H doesn't know what a "dash" is, and
nor would a Base32H decoder or encoder).

## Cheat codes

Once upon a time, back in the ancient times before it was widely possible to
save a game, video games would present codes for players to enter and skip to a
specific level.  While modern games don't have this concern, thanks to data
storage being much cheaper now, anyone going for a retro gameplay style would
surely appreciate being able to generate such codes, even if it's just for the
fun of it.  Plus, adventurous programmers writing homebrew for antique systems
might even have a legitimate need beyond just nostalgic game mechanics.

## Cryptography

While Base32H is obviously not itself an encryption scheme, it can certainly be
used in place of traditional "ASCII hardening" to encode e.g. cryptographic keys
and signatures in a human-readable and ASCII-safe format.  This is handy for
"web of trust" systems like PGP; a Base32H-encoded public key fingerprint is
shorter than the conventional hexadecimal encoding, making it faster to read and
write without sacrificing legibility/audibility like a base-64 encoding would.

Other convenient cryptographic applications include license/activation keys,
hashes, and anything else that might entail someone writing or speaking a
potentially-long number.

## Geographic coordinates / addresses

Being both human-readable and more compact than the decimal system, Base32H
lends itself well to alternative coordinate systems, in the same vein as
Mapcode, Open Location Code, or Natural Area Code.  A Base32H-based coordinate
system would be able to offer potentially-shorter coordinates than these systems
without compromising human-friendliness.
