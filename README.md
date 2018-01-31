# Sourcery Templates

A collection of templates for [Sourcery](https://github.com/krzysztofzablocki/Sourcery), written in [Stencil](https://github.com/kylef/Stencil).

The templates include:

- [Arbitrary.stencil](Arbitrary.stencil): autogeneration of `Arbitrary` conformance for [SwiftCheck](https://github.com/typelift/SwiftCheck);
  - add with `// sourcery: arbitrary`;
  - specify type constraint for generic parameters with a `genericParameterProtocols` annotation (ex. `// sourcery: genericParameterProtocols = "Equatable & Sequence"`);
- [Equatable.stencil](Equatable.stencil): autogeneration of `Equatable` conformance for structs, enums and classes, based on stored properties and cases;
  - add with `// sourcery: equatable`;
- [Init.stencil](Init.stencil): autogeneration of a memberwise initializer for structs and classes, based on stored properties;
  - add with `// sourcery: init`;
- [Match.stencil](Match.stencil): autogeneration of `match` functions for enums (basically, `switch`, but an actual expression that returns something), useful for closures in which a single expression is required for type inference;
  - add with `// sourcery: match`;
- [Lens.stencil](Lens.stencil): autogeneration of "lenses" for structs, based on the implementation proposed in [FunctionalKit](https://github.com/facile-it/FunctionalKit) (please refer to [this article](https://broomburgo.github.io/fun-ios/post/lenses-and-prisms-in-swift-a-pragmatic-approach/) for more info);
  - add with `// sourcery: lens`;
- [Prism.stencil](Prism.stencil): autogeneration of "prisms" for enums, based on the implementation proposed in [FunctionalKit](https://github.com/facile-it/FunctionalKit) (please refer to [this article](https://broomburgo.github.io/fun-ios/post/lenses-and-prisms-in-swift-a-pragmatic-approach/) for more info)
  - add with `// sourcery: prism`;

------

Please refer to the Sourcery [documentation](https://cdn.rawgit.com/krzysztofzablocki/Sourcery/master/docs/index.html) for how to use these templates in your projects.

------

For each generated source file is possible to define optional `import` statements, in case the generated code requires some dependencies to compile. To add those imports, its sufficient to define somewhere in your code a private struct called `SourceryImports_TemplateName`, and add stored properties (their type is not relevant) with the same name as the imported library.

For example, the following struct:

```swift
private struct SourceryImports_Equatable {
  let Foo = ()
  let Bar = ()
}
```

Will produce the following lines at the top of the `Equatable.generated.swift` file:

```swift
import Foo
import Bar
```

To each property you can add a `// sourcery: testable` annotation to add a `@testable` attribute to the `import`.
