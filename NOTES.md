# Notes

## 9 Nov - importing

- Sheets looked like they don't add up to Prose, but they do: OpenRefine counts the header row, and
  History-Sub is just a part of Story.
- Prose sheet already has all the books, subgenre is in `Type`. So only import the Prose sheet.

## 9 Nov - exploring

- Genre is Prose everywhere.
- Type: story 4768, novel 3352, tale 1577, romance 630, satire 185, parod 20 (should be parody).
- Country code is basically empty: `|||` (= no info) 10438, enk 90, stk 2, xxu 1, "fr " 1 (trailing space).
  So place has to come from Imprint.
- Language (008): und 6583, eng 3795, fre 67, pol 40, mul 17, ita 11, spa 10, dut/ger/swe 2, dan/gre/rus 1.
  Most "und" books are probably English, the field is just not filled in.
- Imprint has 5973 different values, Personal author 5614. Too many to check by hand, will need clustering.
- Almost empty columns (filled / 10532):
  Corporate author 90, Subjects 109, DDC 9, Series 20, Language code (041) 19, Other corporate authors 31.
  Probably drop these, TODO: check with Bert.


## 9 Nov - step 1 whitespace

- Trim + collapse spaces on all columns. Only 13 cells changed: Personal author 8, Other personal authors 4, Country code 1 ("fr " → "fr").
