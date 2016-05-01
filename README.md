Swagger is a great way of describing an API in YAML or JSON. From that description one can generate client or server bindings for a huge number of different languages.

Described here is an alternate structure for defining a Swagger API which splits the definition into separate files which are combined by a NodeJS script prior to processing by the Swagger Generator.

## The Problem

There are two problems I ran into when defining an API in Swagger:

1. The API must be defined in a single monolithic file
2. All paths must be defined seperately from the data-type definitions

## Split YAML Files

Split the YAML definition to a directory of files which can be combined.

### Original Structure

``` yaml
definitions:
  foo:
    type: object
    ...
  bar:
    type: object
    ...
```

### Split

#### foo.yaml
```yaml
definitions:
  foo:
    type: object
    properties:
      fooValue:
        type: string
```

#### bar.yaml
```yaml
definitions:
  bar:
    type: object
    properties:
      barValue:
        type: string
```

YAML is a simple key-value tree structure like JSON, so multiple trees can be combined quite easily with existing libraries.


## Full Solution

NodeJS package.json. Install the dependencies with `npm install`

<%= render_code("package.json", "json") %>

Script to combine all yaml files in an src directory

<%= render_code("generate.js", "javascript") %>

Create an `src` directory containing the yaml sources

<%= render_code("src/base.yaml", "javascript", {:range => [1,2]}) %>

Run the script

``` bash
node .
```

A combined Swagger definition will be written to `target/swagger.yaml`

### Example 'src' (From petstore_simple)

<%= render_code("src/base.yaml", "javascript", {:range => [1,5]}) %>

<%= render_code("src/defs.yaml", "javascript", {:range => [1,5]}) %>

<%= render_code("src/solo_pet.yaml", "javascript", {:range => [1,5]}) %>

<%= render_code("src/pets.yaml", "javascript", {:range => [1,5]}) %>
