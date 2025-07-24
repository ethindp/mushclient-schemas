# mushclient-schemas

## Introduction

This repository contains RelaxNG schemas for the MUSHclient XML document format. This includes any worlds, plugins, etc., and any other XML documents that the client may be able to parse or generate in the future. The repository also includes XML schema definitions (XSDs) which can be used for XML schema data binding.

## Schemas and modification policy

The RelaxNG schema format, particularly the compact representation, is the required file in which the official schema is defined. Alterations to the XSD version will not be accepted in pull requests. This is because the XSD file is generated and in the repository solely so that people can download the file and consume it with XML data binding tools, like CodeSynthesis xsd, without needing to manually generate it themselves. Thus, as the schema is an output artifact, it is not meant to be modified by developers of this repository.

To modify or change the schema, make changes to the `.rnc` file. Wherever possible, use the compact format, as this is easiest to read and work with by humans.

Once you have made modifications and are ready to contribute them, send a PR. Please don't regenerate the XSD as I'm hoping that we can get GH actions to do this for us automatically.

## Generating and validating

If you do want to generate the XSD yourself, and possibly even validate it, follow these steps:

1. Download [trang and jing](https://github.com/relaxng/jing-trang). `trang` is the RelaxNG schema translation utility, and can convert between RelaxNG compact, RelaxNG full, and XSD formats. `jing` is the validator tool.
2. Download and install a JRE environment. A JDK environment will also suffice although this is a bit overkill.
3. Run `java -jar /path/to/trang.jar -I rnc -O xsd mushclient.rnc mushclient.xsd`, where `/path/to/trang.jar` is the path to the `trang.jar` java archive.
4. If you wish to validate the generated XSD against an existing MUSHclient file, such as a world (`.mcl`) or plugin (`.xml`), run `java -jar /path/to/jing.jar mushclient.xsd /path/to/mcl_or_xml.{mcl|xml}`.

It is worth noting that validation may not succeed. This schema is still in it's alpha stages and may not be complete or correct, so expect validation to fail with errors.

### A note on booleans

Previously, this schema defined a `McBoolean` type to encompass the fact that MUSHclient treats booleans peculiarly. Specifically, when reading and writing XML files, MUSHclient will read and write booleans using `"y"` and `"n"` instead of what is explicitly specified in the XSD specifications for booleans. Although this caused Jing validation to succeed, it also unnecessarily complicated the schema. Thus, this change was reverted. Here, we use the atomic boolean type, even if validation doesn't succeed at the moment, because these values are, in fact, booleans, and allowing a union type places extra complexity on schema generators.
