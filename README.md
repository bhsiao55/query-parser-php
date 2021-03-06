QueryParser Handler
===================

Php library that converts search queries into terms, phrases, hashtags, mentions, etc.

## Query Syntax

The query parser supports a fairly universal search query standard. It is
meant to support the most common search combinations while be fairly simple to
use by non-technical users.

It supports the following features:
* Keywords are split on whitespace, ex. `word1 word2` contains two keywords.
* Keywords can be grouped using quotation marks, ex. `"word1 word2"` only contains one keyword.
* Keywords can be combined using boolean and, ex. `word1 AND word2`. This is also the default combination, so `word1 word2` is the same as `word1 AND word2`.
* Keywords can be combined using boolean or, ex. `word1 OR word2`.
* Keywords can be exclude or include using "-" or "+", ex. `-excludedWord +mandatoryWord`.
* Combinational logic can be specified using parentheses, ex. `word1 OR (word2 AND word3)`.
* A keyword can be explicitly marked as belonging to a certain domain, ex. `people:Michael`.

## Basic Usage

``` php
<?php

use Gdbots\QueryParser\Parser\QueryParser;
use Gdbots\QueryParser\Visitor\QueryItemPrinter;

$parser = new QueryParser();
$parser->readString('+mandatoryWord AND -excludedWord fieldName:"value"');
$query = $parser->parse();

$printer = new QueryItemPrinter();
$query->accept($printer);
```

**Result:**

```
And
> IncludeTerm
>> Word: mandatoryWord
> Or
>> ExcludeTerm
>>> Word: excludedWord
>> Term: fieldName - value
```

You can also ignore operator by using `$parser->readString('search query here', true)`, which will remove all AND and brackets.
For the above example the result would be:

```
Or
> IncludeTerm
>> Word: mandatoryWord
> ExcludeTerm
>> Word: excludedWord
```
