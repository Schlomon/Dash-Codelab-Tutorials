# Hosting

## Getting started

How to get started with codelabs itself can be found in the [official codelabs tools repo](https://github.com/googlecodelabs/tools).  
Especially under <https://github.com/googlecodelabs/tools/blob/master/site/README.md>,  
and <https://medium.com/@zarinlo/publish-technical-tutorials-in-google-codelab-format-b07ef76972cd>.

## Host Codelab Yourself

### Download hostable site

You can find the static hostable website in the [releases](https://github.com/Schlomon/Dash-Codelab-Tutorials/releases).
Download the `hostable-website-v*.tar.xz` file, extract and host it's content with any webserver. (e.g. `python3 -m http.server`)

### Generate hostable site from source
To quickly generate and host the codelab site in this repository yourselfe, follow these steps:
After cloning:

1. Change directory and get dependencies:

    ```sh
    cd site/
    npm install
    ```

2. Rebuild the site and host codelab:

    ```sh
    cd codelabs/ && claat export *.md && cd .. && gulp serve --codelabs-dir=codelabs
    ```

`gulp` can be replaced by any other webserver, as long as it serves the contents of the `build/` directory.
