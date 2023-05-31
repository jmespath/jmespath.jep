# Title of the feature

- JEP: (leave blank)
- Author: tmccombs
- Created: 2023-05-30

## Abstract
[abstract]: #abstract

Add two new functions which allow parsing a string into a JSON value, and serializing a value to a JSON string.

## Motivation
[motivation]: #motivation

JSON values sometimes contain strings, which are themselves JSON. As a specific example, some AWS APIs
return responses that include values which are strings containing JSON strings, such as IAM policy
documents.

Previously, it was not possible to extract data from such a string directly with JMESPath. At least without
having to use one pass to extract the string value, then use a different expression the result of parsing the
JSON from that string.

Conversely, if you need to serialize data into a string field of the result, then if your input is not a string
you can currently use `to_string`, but that function won't correctly serialize an existing string as a JSON string.
If you don't know the actual type of the value, and it could be a string, you can't safely serialize as a JSON string.

## Specification
[specification]: #specification

This JEP defines two new functions `from_json` and `to_json`.

### `from_json`

The `from_json` function has the following signature:

```
any from_json(string $json_data)
```

This function takes a string, and will parse it as a JSON document and return the resulting value. The resulting
value may be any JSON type.

If the input string is not a valid JSON document, an `invalid-value` error is raised.

### `to_json`

The `to_json` function has the following signature:

```
string to_json(any $value)
```

This function performs the inverse of `from_json`. It takes any JSON value as input, and serialize the result
as a JSON document, and then returns a string containing that serialized result. The resulting JSON should not
contain any unneeded whitespace, including newlines, spaces, or tabs.

## Rationale
[rationale]: #rationale

The names `from_json` and `to_json` were chosen because the names are symmetric with each other and are similar to
the names of the corresponding functions in [`jq`](https://jqlang.github.io/jq/manual/#Convertto/fromJSON).

However, other names could also be accptable, such as:
* `parse_json`/`stringify_json`
* `json_parse`/`json_stringify`
* `deserialize`/`serialize`

Another alternative is to add the `from_json` function but not the `to_json` function, since the existing
`to_string` function can already serialize most values as json. However, the fact that it can't serialize a string as to
JSON is an unfortunate gap, and it seems odd not to have symmetric functions going in both directions. If only a deserizalization
function is added, `from_string` may also be an appropriate name.

This specification only supports serializing as minimal JSON without whitespace. This is conjectured to be what is desired in most
cases where the `to_json` function is used, in order to keep the size of the resulting data small. This is also consistent
with the existing `to_string` function. However, a future extension could
provide functionality to output JSON in a "pretty" format including newlines and indentation, either as an additional function, or with
an optional argument to the `to_json` function. However, this was intentionally left out of scope for this proposal to keep the scope small.

For handlinge invalid JSON input, another possibility is that `from_json` could return some kind of value. However, it would be difficult to distinguish
between parsing a legitimate value, and inability to parse the string. And in most use cases, invalid JSON is probably an unexpected scenario that should
be treated as an error.

## Testcases
[testcases]: #testcases

```yaml
given:
  string: "abc"
  empty_list: []
  empty_hash: {}
  object:
    foo: "bar"
    bar: "baz"
    'null': null
  json: |
      {
          "str": "string",
          "num": 1.25e4,
          "arr": [1,2, "a"],
          "nothing": null
      }
cases:
  - expression: from_json(json)
    result:
      str: "string"
      num: 1.25e4
      arr: [1,2, "a"]
      nothing: null
  - expression: from_json(json).str
    result: "string"
  - expression: "from_json('true')"
    result: true
  - expression: "from_json('false')"
    result: false
  - expression: "from_json('123')"
    result: 123
  - expression: "from_json('1.5')"
    result: 1.5
  - expression: "from_json('1e8')"
    result: 1e8
  - expression: "from_json('5e-3')"
    result: .005
  - expression: "from_json('\"abc\"')"
    result: "abc"
  - expression: "from_json('[]'"
    result: []
  - expression: "from_json('{}')"
    result: {}
  - expression: "from_json('null')"
    result: null
  - expression: "from_json('abc')"
    error: invalid-value
  - expression: to_json(object)
    result: '{"foo":"bar","bar":"baz"}'
  - expression: to_json(string)
    result: '"abc"'
  - expression: to_json(empty_list)
    result: []
  - expression: to_json(empty_hash)
    result: {}
```
