---
layout: default
title: Specification
subtitle: Version 1.0-DRAFT
---
* toc
{:toc}

## Digits

A conformant Base32H implementation must use the following digit set.

|Value|Canonical|Aliases|
|-----------------------|
|0    |0        |O o    |
|1    |1        |I i    |
|2    |2        |       |
|3    |3        |       |
|4    |4        |       |
|5    |5        |S s    |
|6    |6        |       |
|7    |7        |       |
|8    |8        |       |
|9    |9        |       |
|10   |A        |a      |
|11   |B        |b      |
|12   |C        |c      |
|13   |D        |d      |
|14   |E        |e      |
|15   |F        |f      |
|16   |G        |g      |
|17   |H        |h      |
|18   |J        |j      |
|19   |K        |k      |
|20   |L        |l      |
|21   |M        |m      |
|22   |N        |n      |
|23   |P        |p      |
|24   |Q        |q      |
|25   |R        |r      |
|26   |T        |t      |
|27   |V        |v U u  |
|28   |W        |w      |
|29   |X        |x      |
|30   |Y        |y      |
|31   |Z        |z      |
|-----------------------|

## Numeric

A conformant Base32H implementation must implement both a numeric encoder and a
numeric decoder as described below.

### Encoding

A numeric encoder must be able to encode any integer between 0 and
1,099,511,627,775 (inclusive) to its Base32H representation (0 to ZZZZ-ZZZZ,
inclusive).  The encoder may support encoding higher values if it is possible to
do so.

The encoder must emit digits in their canonical form.  The encoder must not emit
any other characters.

### Decoding

A numeric decoder must be able to decode any Base32H integer between 0 and
ZZZZ-ZZZZ (inclusive) to its integer value (0 - 1,099,511,627,775, inclusive).
The decoder may support decoding higher values if it is possible to do so.

The decoder must consume digits in their canonical or aliased forms.  The
decoder must ignore any other characters.

## Binary

A conformant Base32H implementation may implement a binary encoder and binary
decoder as described below.

### Encoding

A binary encoder must divide its input into 40-bit / 5-byte segments and
numerically encode each segment into 8 Base32H digits; for example:

    Binary:  FE ED CA FE BA | BE DE AD BE EF | 01 23 45 67 89 | AB CD EF ED CB
    Decimal: 1094911196858  | 819779714799   | 4886718345     | 737894460875
    Base32H: ZVNW-MZMT      | PVFA-VFPF      | 04HL-ARW9      | MF6Y-ZVEB

If the input data is not evenly divisible into 40-bit segments, the encoder must
pad the beginning of the input data with enough zeroes/nulls to fit the 40-bit
segment boundary; for example:

    Binary:  DE AD BE EF CA FE BA BE
    Padded:  00 00 DE AD BE | EF CA FE BA BE
    Decimal: 14593470       | 1029902875326
    Base32H: 000D-XBDY      | XZ5F-XEMY

The encoder must assume a big-endian byte order by default.

The encoder must emit digits in their canonical form.  The encoder must not emit
any other characters.

### Decoding

A binary decoder must divide its input into 8-digit segments and numerically
decode each segment; for example:

    Base32H: ZVNW-MZMT      | PVFA-VFPF      | 04HL-ARW9      | MF6Y-ZVEB
    Decimal: 1094911196858  | 819779714799   | 4886718345     | 737894460875
    Binary:  FE ED CA FE BA | BE DE AD BE EF | 01 23 45 67 89 | AB CD EF ED CB

If the input data is not evenly divisible into 8-digit segments, the decoder
must pad the beginning of the input data with enough zeroes to fit the 8-digit
segment boundary; for example:

    Base32H: D-XBDY XZ5F-XEMY
    Padded:  000D-XBDY      | XZ5F-XEMY
    Decimal: 14593470       | 1029902875326
    Binary:  00 00 DE AD BE | EF CA FE BA BE

The decoder must assume a big-endian byte order by default.

The decoder must consume digits in their canonical or aliased forms.  The
decoder must ignore any other characters.
