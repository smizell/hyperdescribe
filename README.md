Hyperdescribe
=============

A hypermedia message description format.

**Note**: This is currently a work in progress for developing the [current draft](https://github.com/smizell/hyperdescribe/blob/master/hyperdescribe-draft.md) at version 0.2.0.

This could change drastically before becoming a fully-usable spec. I recommend NOT using this for production purposes at this point.

## Purpose

Currently, building hypermedia APIs is hard, and education about how to do it just starting to spread. The goal of this project is to making hypermedia easier and help improve education about it.

## Description

Hyperdescribe is a standardized way for describing hypermedia messages. It provides a way for libraries to be built that will automatically convert one hypermedia type to another. It also provides a way for libraries to be built that allows for coding to hypermedia elements rather than specific message formats.

![Hypermedia Diagram](https://github.com/smizell/hyperdescribe/raw/master/files/img/hyperdescribe.png)

The question may arise, "Why not just write a converter from one hypermedia type to another?" The reason is that we would end up with tons of converters. We'd have one for HAL to Cj, another for Cj to HAL, another for HAL to Siren, and so on. With this description format, you simply build a parser and builder for each hypermedia format. The parser parses from a hypermedia type to Hyperdescribe, and the builder builds from Hyperdescribe to another media format. 

For this to work, this format must be flexible and must include as many available features as possible from other hypermedia formats. There is a lot of collaboration to make this into a great resource.

## What this is not

Current, this is just a format for describing messages. This project contains no code for converting, and will probably stay that way since it will need to be different for each language. If you do write a library for this, please let me know and I'll add a link to it here.

## Examples

* [Describing an HTML Document](https://github.com/smizell/hyperdescribe/blob/master/examples/html.md)
* [Describing an Uber Message](https://github.com/smizell/hyperdescribe/blob/master/examples/uber.md)
* [Describing a Collection+JSON Document](https://github.com/smizell/hyperdescribe/blob/master/examples/cj.md)
* [Describing a HAL+JSON Document](https://github.com/smizell/hyperdescribe/blob/master/examples/hal.md)
* [Describing a Siren Document](https://github.com/smizell/hyperdescribe/blob/master/examples/siren.md)
