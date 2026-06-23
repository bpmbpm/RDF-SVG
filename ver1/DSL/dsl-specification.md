# DSL1: описание групп функций

## State

`App` хранит состояние приложения:

- `files` - загруженные SVG-файлы.
- `currentFile` и `currentSvgDoc` - активная диаграмма.
- `selectedElement` - выбранный VAD-объект.
- `zoom` - масштаб области просмотра.
- `rdfTriples` - RDF-триплеты документа.
- `treeState` - раскрытые узлы treeView и активный узел.

## Группы DSL

### `DSL.files`

- `load(file)` - читает SVG-файл, добавляет или заменяет его в state, обновляет treeView
  и открывает диаграмму.

### `DSL.tree`

- `render()` - строит treeView:
  - корневой узел `root`;
  - вложенные диаграммы по RDF-предикату `vad:hasParentObj`;
  - ветку `объекты диаграммы` для каждого SVG;
  - список `.vad-element` внутри ветки объектов.

### `DSL.diagram`

- `open(entry, options)` - парсит SVG, извлекает RDF, показывает диаграмму и подключает
  обработчики выбора/контекстного меню.
- `focus(elementId)` - выделяет объект диаграммы и прокручивает область просмотра к нему.

### `DSL.rdf`

- `extractDoc(svgDoc)` - читает RDF/XML из metadata верхнего уровня SVG.
- `extractElement(el)` - читает RDF/XML из metadata выбранного объекта.
- `updateElement(el, triples)` - пересобирает RDF/XML выбранного объекта.

### `DSL.viewport`

- `zoom(value)` - задает масштаб SVG в области просмотра.

### `DSL.ui`

- `contextMenu(x, y, el)` - показывает меню для VAD-объекта.
- `properties()` - открывает панель свойств.

## Сценарии

Открытие диаграммы:

```js
DSL.files.load(file);
DSL.tree.render();
DSL.diagram.open(entry);
```

Выбор объекта из treeView:

```js
DSL.diagram.open(entry, { keepTree: true });
DSL.diagram.focus(elementId);
DSL.tree.render();
```

Редактирование RDF:

```js
const triples = DSL.rdf.extractElement(element);
DSL.rdf.updateElement(element, triples);
```

## Что добавить дальше

- `DSL.vad.primitives` для генерации SVG-блоков VAD: process, role, document,
  application, event, connector.
- `DSL.vad.layout` для дорожек, сетки и маршрутизации стрелок.
- `DSL.rdf.trig` для полноценного чтения TriG named graphs.
- `DSL.history` для undo/redo.
- `DSL.commands` для единообразного выполнения действий UI и тестирования.
