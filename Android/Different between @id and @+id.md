# @id vs @+id

The at-symbol (`@`) at the beginning of the string indicates that the XML parser should parse and expand the rest of the ID string and identify it as an ID resource. The plus-symbol (`+`) means that this is a new resource name that must be created and added to our resources (in the `R.java` file). In other words, the `+` symbol tells Android build tools that you are declaring a new resource, `@id/` you are referring to an existing resource (predefined by `@+id/` and already exists in `R.java`.
