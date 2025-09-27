# API Conference New York 2025: Exercise 3 (OpenAPI)

The goal of this exercise is to start with an abstract API design document (such as API stories) and to turn these into a more concrete and technical API design in OpenAPI.

This still is a description only (i.e., no code has been written at this stage other than the JSON/YAML of the OpenAPI description), but it's in a well-defined machine-readable format that can then be used by tools.

There are several ways to approach this exercise, depending on your preference and level of comfort with working with description formats in JSON/YAML.

1. You can start with the structure of OpenAPI and directly translate it into an OpenAPI description. Using a tool like the [Swagger Editor](https://editor-next.swagger.io/) you can directly write OpenAPI code, but at the same time get feedback in terms of problems with the OpenAPI description, and you'll also see a rendering of the description as an easier-to-read version of what you're writing.

2. You can "vibe-code" the OpenAPI description by either chatting with an LLM, by direct copy/paste of design documents, or by using a combination of these approaches. If you go this route, make sure to copy/paste the result into a tool like [Swagger Editor](https://editor-next.swagger.io/) which will give you immediate feedback about any errors in the OpenAPI description.

The goal at the end of this exercise is to have a relatively well-formed OpenAPI description. At the same time, the goal is to play around with OpenAPI concepts and OpenAPI tooling, and to get a bit of experience with handling OpenAPI

As a backup, here are the well-known "Pet Store" examples often used in OpenAPI examples and training materials as OpenAPI descriptions:

- [Petstore OpenAPI in YAML](petstore.openapi.yaml)
- [Petstore OpenAPI in JSON](petstore.openapi.json)
