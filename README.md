# unicode-collation

[![GitHub
CI](https://github.com/jgm/unicode-collation/workflows/CI%20tests/badge.svg)](https://github.com/jgm/unicode-collation/actions)
[![Hackage](https://img.shields.io/hackage/v/unicode-collation.svg?logo=haskell)](https://hackage.haskell.org/package/unicode-collation)
[![BSD-2-Clause license](https://img.shields.io/badge/license-BSD--2--Clause-blue.svg)](LICENSE)

Haskell implementation of [unicode collation algorithm].

[unicode collation algorithm]:  https://www.unicode.org/reports/tr10

## Motivation

Previously there was no way to do correct unicode collation
(sorting) in Haskell without depending on the C library `icu`
and the barely maintained Haskell wrapper `text-icu`.  This
library offers a pure Haskell solution.

## Conformance

The library passes all UCA conformance tests.

Localized collations have not been tested as extensively.

## Performance

Performance is generally about 2.8 times slower than `text-icu`:

```
  sort a list of 10000 random Texts (en):
    5.8 ms ± 540 μs,  21 MB allocated, 899 KB copied
  sort same list with text-icu (en):
    2.1 ms ± 124 μs, 7.1 MB allocated, 149 KB copied
```

It doesn't seem to matter much what collation is used:

```
  sort a list of 10000 random Texts (zh):
    5.9 ms ± 436 μs,  21 MB allocated, 900 KB copied
  sort same list with text-icu (zh):
    2.3 ms ± 214 μs, 7.1 MB allocated, 147 KB copied
```

Normalization accounts for about 15% of the run time,
as we can see by turning it off:

```
  sort a list of 10000 random Texts (en-u-kk-false = no normalize):
    4.9 ms ± 226 μs,  17 MB allocated, 883 KB copied
```

Performance is much worse in relation to `text-icu` when we
need to sort strings that have a long common initial segment:

```
  sort a list of 10000 random Texts that agree in first 32 chars (en):
    102 ms ± 7.1 ms, 402 MB allocated, 703 KB copied
  sort same list with text-icu (en):
    3.0 ms ± 240 μs, 8.8 MB allocated, 251 KB copied
```

## Localized collations

The following localized collations are available.
For languages not listed here, the root collation is
used.

```
af
ar
as
az
be
bn
ca
cs
cu
cy
da
de-AT-u-co-phonebk
de-u-co-phonebk
dsb
ee
eo
es
es-u-co-trad
et
fa
fi
fi-u-co-phonebk
fil
fo
fr-CA
gu
ha
haw
he
hi
hr
hu
hy
ig
is
ja
kk
kl
kn
ko
kok
lkt
ln
lt
lv
mk
ml
mr
mt
nb
nn
nso
om
or
pa
pl
ro
sa
se
si
si-u-co-dict
sk
sl
sq
sr
sv
sv-u-co-reformed
ta
te
th
tn
to
tr
ug-Cyrl
uk
ur
vi
vo
wae
wo
yo
zh
zh-u-co-big5han
zh-u-co-gb2312
zh-u-co-pinyin
zh-u-co-stroke
zh-u-co-zhuyin
```

Collation reordering (e.g. `[reorder Latn Kana Hani]`)
is not suported

## Data files

Version 13.0.0 of the Unicode data is used:
<http://www.unicode.org/Public/UCA/13.0.0/>

Locale-specific tailorings are derived from the Perl
module Unicode::Collate:
https://cpan.metacpan.org/authors/id/S/SA/SADAHIRO/Unicode-Collate-1.29.tar.gz

## Executable

The package includes an executable component, `unicode-collate`,
which may be used for testing and for collating in scripts.
To build it, enable the `executable` flag.
For usage instructions, `unicode-collate --help`.

## References

- Unicode Technical Standard #35:
  Unicode Locale Data Markup Language (LDML):
  <http://www.unicode.org/reports/tr35/>
- Unicode Technical Standard #10:
  Unicode Collation Algorithm:
  <https://www.unicode.org/reports/tr10>
- Unicode Technical Standard #215:
  Unicode Normalization Forms:
  <https://unicode.org/reports/tr15/>

