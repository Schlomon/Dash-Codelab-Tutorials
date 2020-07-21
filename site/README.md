# About

## Getting started

How to get started with codelabs can be found in the [official codelabs tools repo](https://github.com/googlecodelabs/tools).
Especially under <https://github.com/googlecodelabs/tools/blob/master/site/README.md>,  
and <https://medium.com/@zarinlo/publish-technical-tutorials-in-google-codelab-format-b07ef76972cd>.

## Host Codelab Yourself

To quickly host the codelabs in this repository follow these steps:

Change directory and get dependencies:

```sh
cd site/
npm install
```

Then rebuild the site and host codelab:

```sh
cd codelabs/ && claat export Using-DashJS-with-TypeScript-in-VSCode.md && cd .. && gulp serve --codelabs-dir=codelabs
```

Gulp can be replaced by any other webserver, as long as it serves the contents of the [build](build/) directory.
