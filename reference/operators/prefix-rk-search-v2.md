---
title: "&^~ operator"
upper_level: ../
---

# `&^~` operator

Since 1.2.1.

## Summary

`&^~>` operator for `text[]` is deprecated since 1.2.1. Use `&^~` operator instead.

`&^~` operator performs [prefix RK search][groonga-prefix-rk-search]. R is for [Romaji][wikipedia-romaji]. K is for [Katakana][wikipedia-katakana].

Prefix RK search is useful for Japanese.

Prefix RK search is useful for implementing input completion.

## Syntax

```sql
column &^~ prefix
```

`column` is a column to be searched. It's `text` type or `text[]` type.

`prefix` is a prefix to be found. It's `text` type.

`column` values must be in Katakana. `prefix` must be in Romaji, Hiragana or Katakana.

The operator returns `true` when the `column` value starts with `prefix`.

## Operator classes

You need to specify one of the following operator classes to use this operator:

  * `pgroonga_text_term_search_ops_v2`: For `text`

  * `pgroonga_text_array_term_search_ops_v2`: For `text[]`

  * `pgroonga_varchar_term_search_ops_v2`: For `varchar`

## Usage

Here are sample schema and data for examples:

```sql
CREATE TABLE tag_readings (
  name text,
  katakana text,
  PRIMARY KEY (name, katakana)
);

CREATE INDEX pgroonga_tag_reading_katakana_index ON tag_readings
  USING pgroonga (katakana pgroonga_text_term_search_ops_v2);
```

```sql
INSERT INTO tag_readings VALUES ('PostgreSQL', 'ポストグレスキューエル');
INSERT INTO tag_readings VALUES ('PostgreSQL', 'ポスグレ');
INSERT INTO tag_readings VALUES ('Groonga',    'グルンガ');
INSERT INTO tag_readings VALUES ('PGroonga',   'ピージールンガ');
INSERT INTO tag_readings VALUES ('pglogical',  'ピージーロジカル');
```

You can perform prefix RK search with prefix in Romaji by `&^~` operator:

```sql
SELECT * FROM tag_readings WHERE katakana &^~ 'pi-ji-';
--    name    |     katakana     
-- -----------+------------------
--  PGroonga  | ピージールンガ
--  pglogical | ピージーロジカル
-- (2 rows)
```

You can also use Hiragana for prefix:

```sql
SELECT * FROM tag_readings WHERE katakana &^~ 'ぴーじー';
--    name    |     katakana     
-- -----------+------------------
--  PGroonga  | ピージールンガ
--  pglogical | ピージーロジカル
-- (2 rows)
```

You can also use Katakana for prefix:

```sql
SELECT * FROM tag_readings WHERE katakana &^~ 'ピージー';
--    name    |     katakana     
-- -----------+------------------
--  PGroonga  | ピージールンガ
--  pglogical | ピージーロジカル
-- (2 rows)
```

## See also

  * [`&^` operator][prefix-search-v2]: Prefix search

  * [`&^|` operator][prefix-search-in-v2]: Prefix search by an array of prefixes

  * [`!&^|` operator][not-prefix-search-in-v2]: NOT prefix search by an array of prefixes

  * [`&^~|` operator][prefix-rk-search-in-v2]: Prefix RK search by an array of prefixes

[groonga-prefix-rk-search]:http://groonga.org/docs/reference/operations/prefix_rk_search.html

[wikipedia-romaji]:https://en.wikipedia.org/wiki/Romanization_of_Japanese

[wikipedia-katakana]:https://en.wikipedia.org/wiki/Katakana

[prefix-search-v2]:prefix-search-v2.html

[prefix-search-in-v2]:prefix-search-in-v2.html

[not-prefix-search-in-v2]:not-prefix-search-in-v2.html

[prefix-rk-search-in-v2]:prefix-rk-search-in-v2.html
