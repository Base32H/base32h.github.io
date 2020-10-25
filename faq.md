---
layout: default
title: FAQ
subtitle: A temporarily-hypothetical set of frequently-asked questions
---
* toc
{:toc}

## What is Base32H?

Base32H is a format and digit set for
[base-32](https://en.wikipedia.org/wiki/Base32) / duotrigesimal
numbers.  It's similar to -- and inspired by -- a [design by Douglas
Crockford](https://www.crockford.com/base32.html), but with some minor
tweaks to allow any (English) word to decode to a number and re-encode
back into something mostly resembling itself.

## What are some of Base32H's use cases?

Base32H [lends itself well](/usecases) to unique identifiers,
coordinates/addresses, and other things that humans might frequently
be reading/writing or dictating/hearing.

## What's the Base32H "Alphabet" / digit set?

In short: all the digits and all the uppercase letters on a US
keyboard, except for I, O, S, and U (which are aliases for 1, 0, 5,
and V, respectively); lowercase letters are aliases for their
uppercase equivalents.  All other characters are ignored when decoding
(and not emitted when encoding).

Unlike some other base-32 systems, Base32H does not have a pad
character; instead, it's assumed that any such padding is "implicit"
in the left-most digit (that is: after decoding a Base32H number, it
is up to the application to pad any unused more-significant bits to
whatever boundary it sees fit).  Base32H also does not have any
explicit support for check digits; however, since a Base32H decoder is
expected to ignore characters it does not recognize as digits, it is
straightforward for an application to define such a check digit for
itself (a period -- `.` -- being a reasonable choice given its
relative safety in URLs), and it's also of course possible to
incorporate any other sort of [error correction
code](https://en.wikipedia.org/wiki/Error_correction_code) just like
any ordinary number (since ultimately Base32H numbers are indeed
ordinary numbers).

The full digit set can be found in the [Base32H
specification](/specification#digits).

## Why does Base32H keep `L`/`l` as its own digit?  Won't people mix up `l` with `1` and `I`?

The Base32H encoding specification requires encoders to emit uppercase
letters exclusively, which should substantially mitigate the visual
confusion (since `L` is much more distinct than `l`).  The only
situation where this assumption might fail is if someone writes or
types a Base32H number in lowercase and then has to read it again; the
best guard against this is to hand-write Base32H numbers in uppercase
exclusively (after all, your pen would be the "encoder" in this case),
and to type Base32H numbers into a decoder and save the re-encoded
output.

A very early version of Base32H separated `L` from `l` (making the
latter an alias of `1` alongside `I`/`i`), but that seemed more
confusing than helpful.

## Are there any implementations of Base32H encoders/decoders?

Yes!  This site includes a [list of all known
implementations](/implementations), be they "official" or
community-maintained.  All official implementations can also be found
on the [Base32H GitHub organization](https://github.com/Base32H), and
are all licensed under the ISC License.

More implementation libraries for different languages, as well as some
applications/demos showcasing Base32H in action, are in the works.  One such
demo is included on this site's [homepage](/).

## Can I use Base32H to encode binary data?

Yes, and the [specification](/specification#binary) describes a
recommended mechanism to do so (splitting on 8-digit / 40-bit / 5-byte
segments, which applications are free to interpret as they see fit --
perhaps a 32-bit integer with an 8-bit checksum/ECC, or a single
40-bit integer, or simply an array of bytes, whatever makes sense).
An application can, of course, entirely ignore that and use some other
scheme; the scheme described in the specification is just the default
for "conformant" encoder/decoder libraries.

Not all implementations support binary encoding/decoding (yet), but the
"official" ones which do support it implement the scheme described in the
specification.

## I wrote a Base32H implementation and I'd like it included in that implementation list; how do I go about doing that?

Great!  The list (along with the rest of this site) is [hosted on
Github Pages](https://github.com/Base32H/base32h.github.io), so you're
encouraged to submit a pull request with your changes to that repo's
[`implementations.yml`
file](https://github.com/Base32H/base32h.github.io/blob/master/_data/implementations.yml),
or submit an Issue describing your implementation and someone will get
it added.  There are no specific requirements for inclusion in the
"Implementations" page; if it implements Base32H, then it's good to
go.

If you'd like to go a step further and make your implementation an
"official" Base32H reference implementation (i.e. part of the Base32H
Github organization, as well as yourself being a member of said Github
organization), there are some requirements:

* Your implementation must pass the [Base32H test
  suite](https://github.com/Base32H/base32h-tests) or otherwise
  demonstrably adhere to the numeric encoding/decoding specification
  as published on this site

* Your implementation must be licensed under the ISC License

* Your implementation is encouraged (but not strictly required) to
  implement the binary encoding/decoding scheme described in the
  specification

## Does Base32H have a license?

The idea that a number system would be considered "intellectual
property" seems nonsensical and absurd.  Consider Base32H itself to be
public domain.

All "official" implementations (i.e. implementations listed as
"official" on this site and for which exist repositories within the
[Base32H Github organization](https://github.com/Base32H), as well as
this site itself, are free software per the terms of the ISC License.
The Base32H test suite, likewise, is free documentation per the terms
of the Zero-Clause BSD License (a.k.a. 0BSD).  If you're in some weird
jurisdiction that doesn't recognize the concept of "public domain",
then either of those licenses would be applicable to Base32H itself as
a fallback (since a license to use an implementation logically implies
a license to use the number system itself, as does a license to use
the test files describing and demonstrating the number system).
