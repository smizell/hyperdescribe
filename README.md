Hyperdescribe
=============

A hypermedia message description format.

**Note**: This is currently a work in progress for developing the [first draft](https://github.com/smizell/hyperdescribe/blob/master/hyperdescribe-draft.md).

This could change drastically before becoming a fully-usable spec.

## Purpose

Currently, building hypermedia APIs is hard, and education about how to do it just starting to spread. The goal of this project is to making hypermedia easier and help improve education about it.

## Description

Hyperdescribe is a standardized way for describing hypermedia messages. It provides a way for libraries to be built that will automatically convert one hypermedia type to another. 

The question may arise, "Why not just write a converter from one hypermedia type to another?" The reason is that we would end up with tons of converters. We'd have one for HAL to Cj, another for Cj to HAL, another for HAL to Siren, and so on. With this description format, you simply build a parser and builder for each hypermedia format. The parser parses from a hypermedia type to Hyperdescribe, and the builder builds from Hyperdescribe to another media format. 

## What are some uses?

Below are some ways in which this could be used, although I'm sure there are quite a few other uses or this.

### Build in HTML only, convert to others on server

Currently, in most frameworks, a person must build separate representations for each format they want to serve from their API. This means creating an HTML and then serializing to a JSON format. 

With Hyperdescribe, a person can build just the HTML representation, use a library to convert from HTML to Hyperdescribe, and then from Hyperdescribe to any other supported format. Build once, serve in many different ways.

### Build in Hyperdescribe, convert to others on server

Instead of building to HTML like just mentioned, you could describe your hypermedia with Hyperdescribe and from there convert to other formats.

### Serve HTML from server only, convert to others in browser

Say you have an API that serves HTML+RDFa but your client-side code understands HAL. A Javascript library could be built that was able to make other media formats understandable by adding this Hyperdescribe layer onto the client. 

## How does this make it simpler? And how does this educate?

Building one hypermedia format and getting others for free would make things a lot simpler by itself. The big benefit is that developers could build their entire API in HTML, which brings with it many benefits as I have outlined in [my post on using HTML](http://smizell.com/weblog/2014/html-hypermedia-api-decoupled-ui.html).

As far as helping to educate, developers mostly understand the hypermedia aspects of HTML. Ask a developer to remove the links for forms from their HTML pages, and they will see the lost benefits for not including hypermedia.

## What are the cons?

No hypermedia format is equal, and some formats address different hypermedia factors than others address. For instance, if your original hypermedia format is in HAL and you want to convert to HTML through Hyperdescribe, you will not get any forms (actions). This means this is only as strong as the weakest hypermedia format you use. That means it will probably be best to use the most expressive hypermedia format and convert to others from there.

## What this is not

Current, this is just a format for describing messages. This project contains no code for converting, and will probably stay that way since it will need to be different for each language. If you do write a library for this, please let me know and I'll add a link to it here.

## TODO

This is currently a work in progress, so there is still a lot of work to be done.

1. Should this support all of the different form fields that HTML5 supports, such as selects and textareas?
2. How should URI templates be handled?
