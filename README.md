<h1 align="center">Extending JSON</h1>

<p align="center">Human-writeable JSON-like data formats for GameMaker LTS 2022</p>

&nbsp;

## Loose JSON

"Loose JSON" is a custom JSON-like format developed for use in configuration files. Loose JSON stands somewhere between [JSON](json) and [YAML](yaml) and attempts to combine the familiarity of the former with the writing convenience of the latter.

Loose JSON focuses on being easy to write. It dispenses with commas and double quotes where they are not needed. Loose JSON can also contain single-line and multi-line comments. Any standard JSON is automatically parseable as Loose JSON, but Loose JSON isn't typically parseable as standard JSON.

Consider the following JSON:

```json
{
	"graphics": {
		"width": 1920,
		"height": 1080,
		"fullscreen": true
	},
	"names": ["john and yoko", "paul", "george", "ringo"]
}
```

This could be written as the following loose JSON:

```
{
	graphics: {
		width: 1920
		height: 1080
		fullscreen: true
	}
	names: [john and yoko, paul, george, ringo]
}
```

In Loose JSON, the keywords `true` and `false` (without quotes) are transformed into booleans as one would expect. The keyword `null` (again, without quotes) is converted into GameMaker's native `undefined` datatype.

A string must be delimited using double quotes in the following situations:
1. If a string needs to contain any reserved symbols (`:` `,` `\n` `\r` `{` `}` `[` `]`)
2. If a string needs to be exactly `"true"` `"false"` or `"null"`
3. If a string must contain whitespace at the start or end

Strings can further contain escaped characters.

&nbsp;

## Config JSON

"Config JSON" is custom JSON-like loose format, and is even less strict than "Loose JSON".

Config JSON focuses on being easy to write. It dispenses with commas and double quotes where they are not needed. Config JSON can also contain single-line and multi-line comments (they are ignored on load).
It allows to use duplicated keys (data either is just replaced in case of integers and string, or merged in case of arrays and structs).

```
{
  // display info
  height: 720,
  width: 960,
  width: 1280,
  /* names */
  names: [john and yoko],
  names: ["paul", george, "ringo"],
}
```

This would result in GML struct:

```
{}
 |- height: 720
 |- names:[]
 |         |- "john and yoko"
 |         |- "paul"
 |         |- "george"
 |         \- "ringo"
 \- width: 1280
```
