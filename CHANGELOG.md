# TwitterCldr Changelog

### 6.12.1 (Apr 28th, 2024)
* Fix issue causing sentence segmentation to return incorrect results when a string ends with a suppression directly followed by a single space. (#274, @didier-84)

### 6.12.0 (Aug 28th, 2023)
* Upgrade to CLDR v43, ICU 73.2, and Unicode v15.0.0.

### 6.11.5 (Mar 19th, 2023)
* Fix bug causing locale codes to be converted before language lookup (#263)
  - During `TwitterCldr::Shared::Languages.from_code_for_locale(:nn, :en)`, the `:nn` locale code was converted to `:nb` before lookup, causing "Norwegian Bokmål" to be returned instead of "Norwegian Nynorsk".
  - The conversion process uses the likely subtags data to convert locale codes to ones TwitterCLDR supports.
  - I don't remember why that decision was made, but it's definitely wrong. We shouldn't restrict the locale code -> language names dataset to only supported locales, since there's no danger in allowing access to the whole thing.

### 6.11.4 (Nov 2nd, 2022)
* Fix a bug in the CJK break engine causing an int to be compared to nil (#261, @camertron)
  - The code effectively read past the end of an array because it used the wrong counter variable as an index.

### 6.11.3 (Apr 21st, 2022)
* `Currencies#for_code` returns the name of the currency instead of the entry for the "one" plural form (#254, @ur5us)
  - Older versions of CLDR did not include a `name` field for currency data, so a stop-gap measure was taken.

### 6.11.2 (Feb 17th, 2022)
* Replace `BigDecimal.new()` with `BigDecimal()` for ruby 2.7+ compatibility (#251, @jasonpenny)

### 6.11.1 (Jan 18th, 2022)
* Fix calendars importer.
  - Importer was not correctly looking up aliases that contained back references, i.e. '../'.
* Check `Psych::VERSION` instead of `RUBY_VERSION` when parsing YAML.

### 6.11.0 (Dec 29th, 2021)
* Add support for Ruby 3.1.
  - Fixes issue with loading YAML files containing complex types like `Range`, `Time`, etc. Ruby 3.1 comes with Psych 4 which aliases `load` to `safe_load`. The `safe_load` method disallows parsing non-primitve types.

### 6.10.0 (Dec 18th, 2021)
* Fix calendars importer to better respect aliases that point to ancestor locale data.
* Change calendar default format (see: https://github.com/twitter/twitter-cldr-rb/issues/245).

### 6.9.0 (Nov 30th, 2021)
* Forward arguments and block to TZInfo's `#period_for_local` (#243, @DRBragg)

### 6.8.0 (Nov 11th, 2021)
* Upgrade to CLDR v40, ICU 70.1, and Unicode v14.0.0.
* Remove hacky legacy support for the Thai Buddhist calendar.
  - While this is technically a breaking change, I don't think (or rather, hope) that anyone is actually using it.
* Correctly import calendar data respecting ancestor chain.
  - Remove custom Australian calendar data.

### 6.7.0 (July 17th, 2021)
* Upgrade to CLDR v39 and ICU 69.1.

### 6.6.2 (July 8th, 2021)
* Fix additional bugs causing blank strings when formatting lists.

### 6.6.1 (June 30th, 2021)
* Fix bug causing blank strings when formatting lists (#241)

### 6.6.0 (March 25th, 2021)
* Support BigDecimal in number localization (#240, @opoudjis)

### 6.5.0 (March 22nd, 2021)
* Upgrade to CLDR v38.1 and ICU 68.2.

### 6.4.0 (November 25th, 2020)
* Upgrade to CLDR v38 and ICU 68.1.

### 6.3.0 (October 28th, 2020)
* Add support for Kazakh (#239, @viroulep).

### 6.2.0 (October 1st, 2020)
* Add support for Esperanto (#238, @viroulep).

### 6.1.0 (August 18th, 2020)
*  Use likely subtags data to supplement locale fallback logic (#236, @mkaplan9)
  - `zh-Hant-TW` should fall back to `zh-Hant`, not `zh`.
  - `es-CR` should fall back to `es-419`, not `es`.

### 6.0.2 (July 28th, 2020)
* Fix issue causing incorrect maximization of locales via likely subtags data.
  - Region codes were being incorrectly identified as language codes, eg CH (Switzerland) was being identified as ch (Chamorro), and AF (Afghanistan) was being identified as af (Afrikaans). This led to und_CH ⇒ ch_Latn_GA instead of ⇒ de_Latn_CH.

### 6.0.1 (May 16th, 2020)
* Fix Chinese list formatter (#233).
* Fix `NoMethodError` thrown when attempting to hyphenate German (and other locales) text (#234).

### 6.0.0 (April 25th, 2020)
* Upgrade to CLDR v37, ICU 67.1, and Unicode 13.0.

### 5.4.0 (February 26th, 2020)
* Adds support for the en-001 locale (@SlexAxton).

### 5.3.0 (January 4th, 2020)
* Adds support for Lao, Khmer, and Burmese.
* Adds support for dictionary-based word segmentation.
  - Scripts that don't use spaces as word delimiters have to be segmented using a dictionary.
  - Supported scripts now include Chinese, Japanese, Korean, Lao, Thai, Khmer, and Burmese.
* Adds Ruby 2.7 to the build matrix.

### 5.2.0 (December 1st, 2019)
* Improve performance of the text segmentation algorithm.
  - Break engine now uses state tables from ICU instead of regular expressions.
  - It was... embarassing how slow it was before.
* Adds support for line and grapheme cluster segmentation.

### 5.1.0 (November 21st, 2019)
* Upgrade to CLDR v36, ICU 65.1, and Emoji 12.1.
* Full timezone support in formatted dates and times (eg. "Eastern Standard Time" instead of simply "UTC").
* Full day period support (eg. AM, PM, etc).
* Fix bug causing timezone option to not be passed to the underlying datetime formatter.
* Compose locale data from more accurate ancestor list.
* Move date, time, and datetime tokenizers to static methods so they don't have to be re-created for every formatting operation.
* Add support for en-US (how did it take us 7 years to add this??)

### 5.0.0 (October 15, 2019)
* Upgrade to Unicode v12.0.0, CLDR v35.1, and ICU 64.2.
* Fixes several transliteration bugs causing incorrect transform rules to be applied.
* BREAKING: `LocalizedNumber#to_short_decimal` and `LocalizedNumber#to_long_decimal` have been replaced with `LocalizedNumber#to_decimal#to_s(format: :short)` and `LocalizedNumber#to_decimal#to_s(format: :long)` respectively.
* BREAKING: Telephone code support has been removed since the data are no longer published in the CLDR data set.
* BREAKING: Dropped support for Ruby 1.9.

### 4.4.5 (August 11, 2019)
* Fix infinite recursion bug affecting certain Russian RBNF rule sets (and
  possibly other locales).

### 4.4.4 (April 1, 2019)
* Explicitly set encoding in resource loader to fix encoding bug on Windows.

### 4.4.3 (Feburary 2, 2018)
* Fix warning caused by using the 'u' regex modifier, which is no longer supported.

### 4.4.2 (August 1, 2017)
* Fix list formatter.

### 4.4.1 (June 26, 2017)
* Fix bug in Shared::Caser raising error when titlecasing Japanese text.

### 4.4.0 (April 28, 2017)
* Address several more Ruby 2.4 deprecation warnings.
* Upgrade to RSpec 3, drop rr mocking library.

### 4.3.1 (March 27, 2017)
* Add support for Ruby 2.4.

### 4.3.0 (March 11, 2017)
* Add support for the Slovenian locale (sl).

### 4.2.0 (November 30, 2016)
* Fix parent locale fallbacks (#202).
* Pass along locale when formatting currencies (#203).

### 4.1.0 (November 17, 2016)
* Add support for Tibetan (bo).
* Import a bunch of missing transform rules added in CLDR v27-29.
* Refactor importers, introduce add_locale rake task.

### 4.0.0 (November 1, 2016)
* Upgrade to Unicode v8.0.0, CLDR v29, and ICU 57.1.
* Add support for fields.
* Update plural rules with several bug fixes.
* Add support for hyphenation (uses LibreOffice/Hunspell data).
* Add support for transliteration.

### 3.6.0 (October 20, 2016)
* Override South Korean postal code format, which changed recently.

### 3.5.0 (July 10, 2016)
* Only add JSON as a dependency if running under Ruby < 2 (#191).

### 3.4.0 (July 8, 2016)
* Add units support, eg. "12 degrees Celsius", etc.

### 3.3.0 (March 29, 2016)
* Added `#as_territory` convenience method to LocalizedSymbol (@Anthony-Gaudino).
* Improved documentation for world territories (@Anthony-Gaudino).
* Fixed issues with Unicode regular expressions
  - Unicode properties not recognized correctly
  - Unicode properties not unioned correctly when placed side-by-side
  - Inverted unicode properties not unioned correctly
  - Leading dashes in character classes now treated as literals instead of as
    denoting a range

### 3.2.1 (June 24, 2015)
* Fix units for Gujarati, Kannada, Marathi

### 3.2.0 (June 24, 2015)
* Add Gujarati, Kannada, Marathi

### 3.1.2
* Add 'short' as a valid weekday names form in Shared::Calendar.

### 3.1.1
* Fixed an issue with single quotes appearing in dates formatting.
* Added support for locale codes that have region code in lower case.
* Fixed time zone formatting for DateTime objects.

### 3.1.0

* Updated resources from CLDR v26 (except the collation data).
* Added support for ordinal plurals.
* Added PostalCodes#find_all method.
* Added negative numbers abbreviation.
* Fixed pluralization for abbreviated numbers.
* Fixed pluralization rules by not merging 'en' rules into every locale.

### 3.0.10

* Adding Date back in as a localizable object.

### 3.0.9

* Fixing date and time formatting issue where calling `to_additional_s` on an instance of `LocalizedDate` could raise an error.

### 3.0.8

* Fixing issue causing extraneous single quotes to appear in formatted dates and times.

### 3.0.7

* Territories containment support.

### 3.0.6

* Add en-150 and es-419 locales.

### 3.0.5

* Fixed short numbers formatting for ru and other locales that use patterns
  with literal periods.

### 3.0.4

* Fixed short numbers formatting for ja, ko, af, and a few other locales.
* Added more locales: de-CH, en-AU, en-CA, en-GB, en-IE, en-SG, en-ZA,
  es-CO, es-MX, es-US, fr-BE, fr-CA, fr-CH, it-CH.

### 3.0.3

* Rubinius support.

### 3.0.2

* Adding ability to generate sample postal codes from their regexes.

### 3.0.1

* Fixing abbreviated timespan formats for en-GB (backport from 2.4.3).

### 3.0.0

* Adding maximum_level option to SortKeyBuilder to limit the size of collation sort keys (@jrochkind).
* Significant performance enhancements for normalization via the eprun gem.
* Adding the rule-based number formatters (123 becomes "one hundred twenty-three").
* Major overhaul of most formatters, now using data readers to encapsulate format options and read pattern data.
* Adding support for different numbering systems (eg. arab, latn, etc), number formatter updated accordingly.
* Partial upgrade to CLDR v24 (missing units).
* Support for simple/full/Turkic casefolding. Upper/lowercasing support still needed.
* Support for Unicode regular expressions. Requires oniguruma for use in Ruby 1.8.
* Text segmentation by sentence (word and line support coming soon).
* Executable README.

### 2.4.3

* Fixing abbreviated timespan formats for en-GB.

### 2.4.2

* Fixing non-quoted symbol error in en-GB plural resource file.

### 2.4.1

* Upgrade to CLDR v23.1, ICU4J 51.2.
* Adding en-GB locale (British English).
* Partial support for Ruby 2.0 (yaml no longer breaks, may not dump correctly).

### 2.4.0

* Upgrade to CLDR v23.
* Ability to disable loading of any custom locale resources.
* Long and short decimal formatters now respect the :precision option.

### 2.3.0

* Adding timezone support to date/time formatting.
* Removing the localize method from Date objects.  Call to_date on a LocalizedDateTime or LocalizedTime object instead.

### 2.2.0

* Relaxing JSON dependency to give users more version flexibility. JSON gem now has no version number in twitter-cldr-rb.

### 2.1.1

* Modified AdditionalDateFormatSelector to return the correct format on exact format match.

### 2.1.0

* Significant performance improvements (memoization, resource preloading).
* Number parsing.
* Custom Hebrew units (thanks @yarons!)
* Icelandic and Croatian support.
* Global locale setter and fallbacks.
* Support for territories from CLDR.

### 2.0.2

* Added support for Vietnamese.

### 2.0.1

* Fixed bug for additional date formats that was causing the wrong format to be returned.

### 2.0.0

* Added locales ga, ta, gl, cy, sr, bg, ku, ro, lv, be, sq, sk, and bn.
* Added additional date formats.
* Upgraded to CLDR 22.1.
* Imported currency symbols and formatting rules from CLDR.
* Added support for short/long numbers (eg. 1M for 1,000,000).
* Improved RCov/Simplecov support.
* Added custom Hungarian plurals rule.
* Added support for approximate timespans (relative times).

### 1.9.1

* Locale resources now exported without Unicode escape sequences.

### 1.9.0

* Included Unicode-safe YAML dumping support via an adaptation of the ya2yaml gem.
* Implemented the Unicode Bidirectional Algorithm to help reorder mixed right-to-left and left-to-right text.
* Added list formatting support.

### 1.8.1

* Improved, more accurate Finnish and Chinese collation support.
* Moved JavaScript build environment to twitter-cldr-js.

### 1.8.0

* Added support for language code conversion.
* `#localize` methods (eg. for Hash, String, etc) now dynamically generated, part of the `TwitterCldr::Localized` namespace.
* New convenience method `TwitterCldr::Normalization#normalize`.

### 1.7.0

* Wrote rake tasks to update CLDR and ICU resources.
* All resource files now written with symbolized keys so the gem doesn't have to recursively symbolize them on load.
* Unicode code points now represented internally with integers instead of strings for better performance.
* Added number formatting in JavaScript.
* Added telephone code lookup functionality (per country) and postal code validation.

### 1.6.2

* Collation tries now loaded from marshal dumps, collation running time improved by \~80%.

### 1.6.1

* Added case-first collation element tailoring support for languages like Danish.
* Included a missing development dependency (ruby_parser).

### 1.6.0

* Added locale-aware collation via fractional collation element tailoring.
* Added #sort and #sort! methods to LocalizedArray.
* Added JavaScript relative time functionality, eg. "2 seconds ago".

### 1.5.0

* Added collation (sorting) support via the Unicode Collation Algorithm.
* Added Catalan, Basque, Greek, Afrikaans, Ukrainian, and Czech support along with calendar fixes for existing locales.
* DateTimeTokenizer now falls back on English if the given locale isn't supported.

### 1.4.1

* Added ability to use NFC and NFKC in core_ext/string

### 1.4.0

* Added NFC and NFKC algorithms.
* Refactored Shared::UnicodeData::Attributes into Shared::CodePoint.

### 1.3.6

* Added relative time functionality, eg. "2 seconds ago".

### 1.3.0

* Reorganized locale resources.
* Added explicit specs for examples in the README.
* ArgumentError now raised if a resource can't be found.
* Fixed behavior of the :precision option for number formatting.
* Updated CLDR data to v21 (http://cldr.unicode.org/index/downloads/cldr-21).
* Added support for localized arrays (i.e. arrays of Unicode code points).

### 1.2.0

* Added NFKD normalization algorithm.
* Formatter tokens now cached for better performance.
* Improvements to core extensions (Symbol, Date, etc).
* Added full normalization test from unicode.org.
* Autoload classes to improve performance.

### 1.1.0

* Plural support [@KL-7]
* Unicode data, decomposition [@timothyandrew]

### 1.0.1

* Fixed a US-ASCII bug that caused rake errors. This fix applies to both Ruby 1.8 and 1.9.
* Fixed a regexp error in a test function, as well as a tokenizer bug. All tests now pass.
* Added support for Travis, a distributed build platform.

### 1.0.0

* Look ma, I'm open source!

### 0.1.4

* Added functionality to gracefully fall back on default locale if chosen locale is unsupported.

### 0.1.3

* Added support for Arabic, Hebrew, Farsi, Thai, and Urdu.

### 0.1.2

* Added world language support.

### 0.1.1

* Localized dates, times, and datetimes can now be interchangeably converted to each other.
* Fixed a bug that would not allow lookup of resource data by string (only symbol).
* Added really basic plural support.

### 0.1.0

* Birthday!
