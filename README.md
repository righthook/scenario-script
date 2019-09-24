# ScenarioScript

## DRAFT NOTICE

ScenarioScript is currently in a Draft phase.  That means that it may change before version 1.0.  During this phase we actively encourage you(!) to provide feedback, requests, comments, and/or contributions.

## Overview

ScenarioScript is a portable, open format for describing driving situations.  Our goal is to create a format that is descriptive enough for a simulator, but empirical enough to be created from real-world, recorded data.  Additionally, since adopters will inevitably need customization, we have designed for extendability.

## What is in this Repository?

This repository contains
* [a slidedeck overviewing the format](docs/ScenarioScript_Introduction.pdf)
* [a JSON schema describing the scenario format](schema.json)
* [an example of the "base," or unextended, scenario](examples/base/scenario.json) and [data structures used in it](examples/base/)
* [examples for a format extended by an imaginary company, RefCo](examples/ref_co)

## Motivation, Philosophy, and Design

To get a feel for why we created the format, what we think a good format should include, and how we attempt to deliver on those goals, take a look at our [Introductory Slidedeck](docs/ScenarioScript_Introduction.pdf).

## The Specification

Ok really though, did you look at the [slidedeck](docs/ScenarioScript_Introduction.pdf)?  Because we promise it makes the rest of this explanation a lot easier.

The specification takes the form of a [JSON schema v4](https://json-schema.org/specification.html), found [here](schema.json).  **A document that is in compliance with ScenarioScript is one that validates against the schema.**

A list of projects related to JSON schema (validators, schema editors, etc.) can be found [here](https://json-schema.org/implementations.html).

## Extending the Schema

The schema is designed to be extended through what we call "abstract elements."  These are places where we acknowledge that adopters will want to represent data differently depending on their use cases.

In the [base example](examples/base/scenario.json), these objects appear as `null`.

An extended object must contain a `type` field, which is used to tell different implementations/subclasses apart.

As an example, consider the `weather` object.  Imaginary company RefCo may want to represent rain as a float in [0, 1], while imaginary company OtherCo wants to represent rain as `true` or `false`.  Each company is able to extend the schema to represent this object the way it likes.  RefCo's `weather` might be
```
"weather": {
	"type": "ref_co_weather",
	"rain": 0.2
}
```
while OtherCo's `weather` might be
```
"weather": {
	"type": "other_co_weather",
	"rain": true
}
```

For more examples, see the [RefCo example scenario](examples/ref_co/scenario.json) and [data structures used in it](examples/ref_co/).  Compare the fields that are `null` in the base example scenario to those same fields in the RefCo example scenario.

## "Hello World"

To give the specification a try, go to the JSON validator at [jsonschemavalidator.net](https://www.jsonschemavalidator.net/).  Paste the contents of [schema.json](schema.json) into the left side and the contents of the [example base scenario](examples/base/scenario.json) into the right side.  To get hands-on, give these experiments a try.  (note: the tool is not great for syntax errors so if you think you have one, [jsonlint](https://jsonlint.com) will find it)

* **Try messing with the "building blocks":** deleting the `"u": 25.0` key-value pair from the vehicle's initial velocity (line 14) invalidates the scenario.
* **Try messing with the structure:** deleting the `"version": "v0.1"` key-value pair from the scenario (line 2) invalidates it.
* **Try messing with the abstract elements:** putting an empty object `{ }` for the `place` key (line 5) invalidates the scenario.
* **Implement an abstract element:** putting the object `{ "type": "my_place" }` for the `place` key (line 5) makes the scenario valid.
* **Try an empty object for the scenario:** delete the entire right side and put `{ }`.  The validator will tell you what fields are required.

Now, paste the contents of the [RefCo example scenario](examples/ref_co/scenario.json) into the right side.

* **Add fields to abstract objects:** add any fields you like to RefCo's `place` object (lines 5-8).  The only way to invalidate the scenario in the `place` object is to delete the `type` field.

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests to us.

## Authors

* **RightHook** - *Initial work* - [RightHook](https://www.righthook.io)

See also the list of [contributors](https://github.com/righthook/scenario-script/contributors) who participated in this project.

## License

This project is licensed under the Apache 2.0 License - see the [LICENSE](LICENSE) file for details
