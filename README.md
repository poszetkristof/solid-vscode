## 1. Identify SOLID Principles
Chosen open source project: [VS Code](https://github.com/microsoft/vscode)

| Principle | Examples |
| -------- | -------- |
| **Single Responsibility Principle**   | *callHierarchyTree.ts*, [L76-L81](https://github.com/microsoft/vscode/blob/main/src/vs/workbench/contrib/callHierarchy/browser/callHierarchyTree.ts#L76#L81)    |
|          | Compares two elements of type `Call`.  |
| **Open/Closed Principle**    | *dispose.ts*, [L15-L40](https://github.com/microsoft/vscode/blob/main/extensions/simple-browser/src/dispose.ts#L15#L40)     |
|          | `Disposable` abstract class can be extended and subclasses can add their specific functionality, without modifying the base class.  |
| **Liskov Substitution Principle**    | *verifier.ts*, [L12-L56](https://github.com/microsoft/vscode/blob/main/src/vs/base/common/verifier.ts#L12#L56)          |
|          | Subclasses (e.g. `BooleanVerifier`, `NumberVerifier`, `SetVerifier`) of base abstract class `Verifier` can be easily substituted without breaking the application. |
| **Interface Seggregation Principle**    | *logger.ts*, [L257-L262](https://github.com/microsoft/vscode/blob/main/test/automation/src/logger.ts#L10#L43)     |
|          | `Logger` interface has a single `log` method that serves a single purpose, to log messages. Each class (`ConsoleLogger`, `FileLogger`, `MultiLogger`) uses `log` for different use cases.
| **Dependency Inverion Principle**    | *pathService.ts*, [L12-L21](https://github.com/microsoft/vscode/blob/main/src/vs/workbench/services/path/electron-sandbox/pathService.ts#L12#L21)     |
|          | `PathService` class depends on abstractions, extends `AbstractPathService`, and all params from constructor are abstractions.   |


## 2. Violations of SOLID and Other Principles
Chosen open source project: [VS Code](https://github.com/microsoft/vscode)

| Principle | Examples |
| -------- | -------- |
| **Single Responsibility Principle**   | *actionbar.ts*, [L67-L600](https://github.com/microsoft/vscode/blob/main/src/vs/base/browser/ui/actionbar/actionbar.ts#L67#L600)    |
|          | The class `ActionBar` has many responsibilities, like registering and handling focus and blur + keyup and keydown events, instantiating and handling the `ActionRunner`   |
|          | Separate the action handling, create a separate class for it.  |
|          | Separate also the DOM manipulation logic, extract into a separate class.  |
| **Open/Closed Principle**    | *folding.ts*, [L73-L80](https://github.com/microsoft/vscode/blob/main/extensions/typescript-language-features/src/languageFeatures/folding.ts#L73#L80)
|          | In case if new span kind will be introduced, need to modify `getFoldingRangeKind` method of class `TypeScriptFoldingProvider` |
|          | Proposal: store key-value pairs in a map and extract it.  |
| **Liskov Substitution Principle**    | *dirtyDiffDecorator.ts*, [L62-L71](https://github.com/microsoft/vscode/blob/main/src/vs/workbench/contrib/scm/browser/dirtydiffDecorator.ts#L62#L71)     |
|          | Object of `ActionRunner` can't be replaced with object of `DiffActionRunner` because the additional precondition in `runAction` of subclass - `if (action instanceof MenuItemAction)`.  |
|          | The `run` method of `MenuItemAction` behaves differently, better to remove the check between L65-L67 (*DiffActionRunner*) and refactor `run` method.  |
| **Interface Seggregation Principle**    | *tree.ts*, [L219-L221](https://github.com/microsoft/vscode/blob/main/extensions/references-view/src/tree.ts#L219#L221)     |
|          | Fix: break down the `TreeDragAndDropController<T>` interface into smaller interfaces.  |
| **Dependency Inverion Principle**    | *typeHierarchyPeek.ts*, starting from [L69](https://github.com/microsoft/vscode/blob/main/src/vs/workbench/contrib/typeHierarchy/browser/typeHierarchyPeek.ts#L69)     |
|          | High-level module (`TypeHierarchyTreePeekWidget` class) depends on multiple low-level modules (e.g. `Dimension`, `TypeHierarchyTree`)  |
|          | Instead of depending on `Dimension` (contains specific implementation), it should depend on `IDimension` interface.  |
|          | Remove concrete implementation of `TypeHierarchyTree`, looks like it was just created to "mock" a type. |

**YAGNI**: in some functions there are unused params

**DRY**: abstract class **Disposable** is implemented in the same way at multiple places (*media-preview/src/util*, *simple-browser/src*, *typescript-language-features/src/utils*)

**KISS**: *actionbar.ts*, [L67-L600](https://github.com/microsoft/vscode/blob/main/src/vs/base/browser/ui/actionbar/actionbar.ts#L67#L600): ActionBar class should be simpler
