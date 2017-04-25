# Wiry

Wiry is an unopinionated dependency injection container for your node or typescript appllication.

## Design Goals

**Your code should not be aware of Wiry**.

You should have one file, or one layer that deals with configuring the container, but every other class or function should have no knowledge of a container. Classes will not have magic annotations that get processed, or assumptions of how the class will be used. 

## Example Usage

...
