I want to generate an `@OutputFiles` property for a task programmatically.

The output directory is specified as a property with an `@OutputDirectory` annotation, and that is used by the task being run. That process generates a number of outputs (simplified down to one here) whose names are based on the `libVersion` and `libName` properties, and which are created in the `@OutputDirectory`.

To populate the `@OutputFiles` property I need to update the `ConfigurableFileCollection` with the file paths, which I can generate  by calling `outputDir.get().file()` during task execution. I can't update the property then though: I get an error telling me that `The value for this file collection is final and cannot be changed.`.

If I try and do this during task initialization by mapping `outputDir`, to keep it lazy, with code like: 

```groovy
abstract final ConfigurableFileCollection libraryFiles = project.objects.fileCollection().from(project.provider {
    outputDir.map { it.files("${libName.get()}-${libVersion.get()}-eg.txt") }
} as Provider<FileCollection>)
```

I get an error like this: 
```
Querying the mapped value of task ':buildThing' property 'outputDir' before task ':buildThing' has completed is not supported
```

This (populating the outputfile based on inputs) seems like a thing people would want to do, but I don't know how... 

This repo contains a gradle project which is the simplest version of this I could make, so it's hopefully easy to understand, and easier to fix...
