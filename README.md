# Sourcery Templates

A collection of templates for [Sourcery](https://github.com/krzysztofzablocki/Sourcery), written in [Stencil](https://github.com/kylef/Stencil).

The templates include:

- [Arbitrary.stencil](Arbitrary.stencil): autogeneration of `Arbitrary` conformance for [SwiftCheck](https://github.com/typelift/SwiftCheck);
	- add with `// sourcery: arbitrary`;
	- specify type constraint for generic parameters with a `genericParameterProtocols` annotation (ex. `// sourcery: genericParameterProtocols = "Equatable & Sequence"`);
- [Equatable.stencil](Equatable.stencil): autogeneration of `Equatable` conformance for structs, enums and classes, based on stored properties and cases;
	- add with `// sourcery: equatable`;
- [Init.stencil](Init.stencil): autogeneration of a memberwise initializer for structs and classes, based on stored properties;
	- this is a inline generator, meaning that it will add the code directly in the source file, and won't generate an external `Init.generated.swift` file;
	- set the start for the inline generation with `// sourcery:inline:__TYPE_NAME__.Init` and set the end with `// sourcery:end`;
- [Match.stencil](Match.stencil): autogeneration of `match` functions for enums (basically, `switch`, but an actual expression that returns something), useful for closures in which a single expression is required for type inference;
	- add with `// sourcery: match`;
- [Lens.stencil](Lens.stencil): autogeneration of "lenses" for structs, based on the implementation proposed in [FunctionalKit](https://github.com/facile-it/FunctionalKit) (please refer to [this article](https://broomburgo.github.io/fun-ios/post/lenses-and-prisms-in-swift-a-pragmatic-approach/) for more info);
	- add with `// sourcery: lens`;
	- add `chain` option for automatic chaining of lenses: with `// sourcery: lens = "chain"` you'll be able to compose lenses with `TypeName.lens.prop1.prop2.prop3`;
- [Prism.stencil](Prism.stencil): autogeneration of "prisms" for enums, based on the implementation proposed in [FunctionalKit](https://github.com/facile-it/FunctionalKit) (please refer to [this article](https://broomburgo.github.io/fun-ios/post/lenses-and-prisms-in-swift-a-pragmatic-approach/) for more info)
	- add with `// sourcery: prism`;
	- add `chain` option for automatic chaining of prisms: with `// sourcery: prism = "chain"` you'll be able to compose prisms with `TypeName.prism.case1.case2.case3`;

Please note that lens and prism chaining **only works** with *non-nested* types, e.g. it won't work for types like `TypeName.NestedType`, due to a limitation with Sourcery.

------

Please refer to the Sourcery [documentation](https://cdn.rawgit.com/krzysztofzablocki/Sourcery/master/docs/index.html) for how to use these templates in your projects.

------

For each generated source file is possible to define optional `import` statements, in case the generated code requires some dependencies to compile. To add those imports, its sufficient to define somewhere in your code a private struct called `SourceryImports_TemplateName`, and add stored properties (their type is not relevant) with the same name as the imported library.

For example, the following struct:

```swift
private struct SourceryImports_Equatable {
  let Foo = 0
  let Bar = 0
}
```

Will produce the following lines at the top of the `Equatable.generated.swift` file:

```swift
import Foo
import Bar
```

To each property you can add a `// sourcery: testable` annotation to add a `@testable` attribute to the `import`.

------

## Integration

Each project is different, so there's no generic bash script or YAML file included here: just download the templates and use them as you like, or simply add a submodule in a `Templates` folder.
