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

## 9 Nov - step 2 title "[electronic resource]"

- Removed "[electronic resource]" from Title with `value.replace(/\s*\[electronic resource\]/, "")`. 10529 cells changed.
- Checked with a text filter on "electronic": 1 left, a typo "[electronic resouirce]" (Antoinette : a tale of the ancien régime).
  Fixed that one by hand.

## 9 Nov - checking Imprint

- 12 empty. All the others have a place and a year, but only 70% have a publisher (30% are just `London, 1869.`).
- Year is messy: [1873.] (1664 rows), ranges like 1884-93 (205), two years like 1860 [1859] (399).
- London is 71%. Same place written differently (`Edinburgh & London` / `Edinburgh and London`).
- Some odd ones that will break a simple split: `Whiting & Co.: London, 1888.`, "printed by ... sold by ...".

## 10 Nov - step 3 title brackets

- Some titles are fully wrapped in [ ] (= title made up by the cataloguer). Removed the outer brackets with
  `value.replace(/^\[(.*)\]$/, "$1")` in a new column, renamed old Title to `OG_Title` so the original stays. 893 changed.
- Greedy regex broke 5 titles that start and end with different brackets, e.g. `[A novel.] By ... [J. B. Harwood].`
  Fixed those back by hand, plus some typos in the original (Hogan `.)]`, Hungerford `]`, Ned Locksley `”]`, Mary Hollis, Elegy).
- Hand edits are not in the JSON (OpenRefine doesn't extract single cell edits).

## 10 Nov - step 4 drop DDC

- Removed DDC (only 9 of 10532 filled). Agreed with Bert.

## 12 Nov - step 5 title fix + drop OG_Title

- `[Manual of Geology ... Revised edition.].` was still bracketed: it ends with `].` so the step 3 regex (needs `]` at the very end) skipped it. Removed the brackets (3 rows have this title).
- Dropped `OG_Title`.

## 12 Nov - step 6 author name

- New column `Personal author date` = last comma part if it has a 3+ digit number, dots removed (`1850-1912`, `d 1880`, `fl 1850`). `\d{3}` so `1st Bart.` doesn't count. 1891 rows got a date.
- Removed that date part from Personal author: `value.replace(/,\s*[^,]*\d{3}[^,]*$/, "")`.
- Surname to Titlecase, but only if it was ALL CAPS.

## 12 Nov - step 7 author casing fixes

- toTitlecase messed up some surnames, fixed 73 cells: `O'byrne` -> `O'Byrne`, `Hahn-hahn` -> `Hahn-Hahn`,
  `Auget De Saint-sylvain` -> `Auget de Saint-Sylvain`.

## 12 Nov - step 8 Language (041)

- Took out the MARC codes: `$aeng$afre$adut` -> `eng, fre, dut`. 19 cells.

## 13 Nov - step 9 split Imprint

- Imprint -> 3 new columns, same names and order as Bert: `Publisher/printer`, `Place of publishing`, `Year of publication`. Imprint removed after.
- Year: `value.find(/1[4-9]\d\d/)[0]` = first year, plain number.
- Place: `value.split(":")[0]`, then cut the date `value.replace(/[,;]?[^,;]*1[4-9]\d\d.*$/, "")`, removed brackets, trim,
  then drop printing places `value.replace(/\s*[,;].*printed,?$/, "")`.
- Publisher: `value.partition(":")[2]` (keeps text after a 2nd colon), same date cut, brackets out + trim.
## 14 Nov - step 10 author titles

- Removed titles/jobs/degrees from the Personal author