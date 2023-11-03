<h1 align="center">Extending JSON</h1>

<p align="center">Human-writeable JSON-like data formats for GameMaker LTS 2022.</p>

<p align="center"><i>You may also be interested in <a href="https://github.com/JujuAdams/SNAP">SNAP</a>, a standard data format and struct/array utility toolkit.</i></p>

&nbsp;

This library focuses on a couple JSON-like formats that are designed to be maximally human-writeable. JSON is a great format, I love it, but it is a format that is more easily written and read by a computer than a human. This makes JSON unhelpful for situations where a human is likely to want to modify the data.

&nbsp;

## Loose JSON

Loose JSON is as it sounds - a looser form of JSON. It stands somewhere between JSON] and YAML and attempts to combine the familiarity of the former with the writing convenience of the latter.

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

Config JSON is an extension to the previously described Loose JSON but is even less strict. It allows for data to be defined and overwritten, something that is usually forbidden in standard JSON (including "loose JSON" above). In the case of integers and strings, values are overwritten. Structs and arrays, however, are merged together where possible.

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
