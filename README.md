# Draft.php

A validation library which build on top of the draft-php library.

[![Latest Version on Packagist](https://img.shields.io/packagist/v/aight8/draft-php-validator.svg?style=flat-square)](https://packagist.org/packages/aight8/draft-php-validator)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE)
[![Build Status](https://img.shields.io/travis/aight8/draft-php-validator.svg?style=flat-square)](https://travis-ci.org/aight8/draft-php-validator)
[![Total Downloads](https://img.shields.io/packagist/dt/aight8/draft-php-validator.svg?style=flat-square)](https://packagist.org/packages/aight8/draft-php-validator)

This library validates the content state by the given configuration.

Optional: Additionally it can Autofix invalid content states if possible.

## Features

##### Validate the block depth
  - **Is possible on target block type** (Autofix: Set to 0)
  - **Depth raises in incremental steps** (Autofix: Set to last valid depth)
  - **Maximal depth** (Autofix: Set to maximum depth)

##### Allows only specific...
  - **Content block types** (Autofix: Set to default block type)
  - **Entity types** (Autofix: Remove from ContentBlock and EntityMap)
  - **Inline styles** (Autofix: Remove from ContentBlock)

##### Check for limits when set...
  - **Character count** (no Autofix)
  - **Word count** (no Autofix)
  - **Line count** (no Autofix)

##### ContentBlock
  - **No newline character** - called soft newlines in draft.js (Autofix: Split block)

##### CharacterMetadata
  - **Entity must reference to an existing entity in the entity map** (Autofix: Remove from ContentBlock)

##### EntityMap
  - **Not referenced entities** (Autofix: Remove from EntityMap)
  - **Validat entity data** (Autofix: filter the entity data)

## More Features

- Unicode Characters are handled correctly
- draft.js Defaults are included (Block Types, Inline Styles, List Block Types)

## Usage

The default configuration of the ValidatorConfig is lazy and contains only
the very basics.

### ValidatorConfig class

```
public function __construct(array $config = null)
```

The passed configuration array can contains following options:

| Configuration key | Type | Description | Default value |
| --- | --- | --- | --- |
| content_block_max_depth | ... | int | 4
| content_block_types | ... | string[]? | Defaults::BLOCK_TYPES
| inline_styles | ... | string[]? | Defaults::INLINE_STYLES
| entity_types | ... | string[]? | []
| entity_validator | ... | callback(DraftEntity):DraftEntity? | null
| block_types_with_depth | ... | string[]? | Defaults::LIST_BLOCK_TYPES
| incremental_depth_steps | ... | bool | true
| max_character_count | ... | int? | null
| max_word_count | ... | int? | null
| max_line_count | ... | int? | null

```content_block_types```, ```inline_styles```, ```entity_types```, ```block_types_with_depth``` have
all a default value (the default constants in draft.js).
- If one of those values is set to ```null``` - then everything is allowed.
- If one of those values is set to ```[]``` then nothing is allowed.

### Validator class

The relevant validation function is this one:
```PHP
public function validate(ContentState $contentState, $validatorConfig = null, $tryAutoFix = null)
```

If the third parameter ```tryAutoFix``` is true, it autofixes all invalid catches.
**Excepted for character/word/line limitations!** This will always throw an exception.

If ```tryAutoFix``` is false then it always throws an exception on the first invalid catch.

By default autofix is enabled because this is the most common use case.

### Future

- Optional Autofix for max char/word/line count by shrinking when it overlaps
But this is not default since data loss should be never the default. However
this limitation checks should be implemented on the client side first! This is only
a safety check.




