# React Shape Editor [![npm version](https://badge.fury.io/js/react-shape-editor.svg)](https://www.npmjs.com/package/react-shape-editor) [![CircleCI](https://circleci.com/gh/fritz-c/react-shape-editor.svg?style=svg)](https://circleci.com/gh/fritz-c/react-shape-editor)

Basic yet flexible shape editor component

[![shape-editor](https://user-images.githubusercontent.com/4413963/59026651-eb403580-8891-11e9-9fea-33843b6386c0.gif)](https://fritz-c.github.io/react-shape-editor/)[![shape-editor](https://user-images.githubusercontent.com/4413963/74218816-59c17700-4cee-11ea-97fb-29b6a299e0ec.gif)](https://codesandbox.io/s/video-labeling-react-shape-editor-86dxw)

**Other projects using react-shape-editor:**

- [Bounding Box Labeler](https://fritz-c.github.io/bounding-box-labeler/) by [@fritz-c](https://github.com/fritz-c) ([source](https://github.com/fritz-c/bounding-box-labeler))
- _(Submit a PR to add yours here!)_

## Installation

```sh
npm i react-shape-editor
```

## Usage / CodeSandbox Examples

- [Basic Example with Drawing](https://codesandbox.io/s/basic-example-with-drawing-react-shape-editor-vcmj3)
- [Selection or Drawing Toggle](https://codesandbox.io/s/selection-or-drawing-toggle-react-shape-editor-7zed8)
- [Selection and Drawing Hybrid](https://codesandbox.io/s/selection-and-drawing-hybrid-react-shape-editor-kejgy)
- [Bounding Boxes on Video](https://codesandbox.io/s/video-labeling-react-shape-editor-86dxw)
- [Performance Test](https://codesandbox.io/s/performance-test-react-shape-editor-kpzwu)

## Components

### ShapeEditor

The wrapper for the entire editor component. Contains the `<svg>` element.

| Prop                          |        Type         | Default | <div style="width: 400px;">Description</div>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| :---------------------------- | :-----------------: | :-----: | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| children<br/>_(required)_     | renderable elements |         | Will include components such as `wrapShape`-wrapped shapes, other library components (`SelectionLayer`/`ImageLayer`/`DrawLayer`) or arbitrary SVG elements                                                                                                                                                                                                                                                                                                                                                                                       |
| vectorHeight<br/>_(required)_ |      `number`       |         | Height of the `<svg>` element viewBox (minus padding), in user units.                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| vectorWidth<br/>_(required)_  |      `number`       |         | Width of the `<svg>` element viewBox (minus padding), in user units.                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| padding                       | `number`\|`object`  |   `0`   | Padding _inside_ the `<svg>`, in px. Useful for displaying SVG content that would normally get clipped off by the edge of the SVG element. Can also help when providing contextual information (e.g., the surrounding parts of an image) while keeping the 0,0 coordinates on the part of the image you are focused on, avoiding having to convert the coordinates back-and-forth. Can be set via a number to apply to all edges, or as an object in the form: `{ top: number, right: number, bottom: number, left: number }` to apply per-side. |
| scale                         |      `number`       |   `1`   | Scale factor of the svg contents. For example, given a `vectorWidth` of `100` and a scale of `0.5`, the rendered DOM element will be 50 px wide.                                                                                                                                                                                                                                                                                                                                                                                                 |
| focusOnAdd                    |       `bool`        | `true`  | If `true`, focus on newly created elements.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| focusOnDelete                 |       `bool`        | `true`  | If `true`, focus on the next-closest element after a shape is deleted.                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| style                         |      `object`       |  `{}`   | Style to apply to the `<svg>` element.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |

---

### wrapShape (HOC)

When used to wrap an SVG element, enables resize and movement functionality.

**Usage**

```js
const WrappedRect = wrapShape(({ height, width /* ... "wrapShape Props Received" */ }) => (
  <rect fill="blue" height={height} width={width} />
))

// later, in render()

<WrappedRect
  shapeId={myId}
  x={12}
  y={56}
  width={20}
  /* ... "WrappedShape Props" */
/>
```

**wrapShape Props Received**

| Prop               |   Type   | <div style="width: 400px;">Description</div>                                                                                                                                                                                                                                              |
| :----------------- | :------: | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| height             | `number` | Height of the shape.                                                                                                                                                                                                                                                                      |
| width              | `number` | Width of the shape.                                                                                                                                                                                                                                                                       |
| scale              | `number` | Scale of the parent `<svg>` element, provided so you can render strokes or other components that maintain a constant size at every zoom level.                                                                                                                                            |
| shapeId            | `string` | Unique identifier for the shape.                                                                                                                                                                                                                                                          |
| x                  | `number` | x-axis offset of the shape. **NOTE:** You should not use this to set the position of your shape, because the `<g>` that wraps the shape already includes this offset.                                                                                                                     |
| y                  | `number` | y-axis offset of the shape. **NOTE:** You should not use this to set the position of your shape, because the `<g>` that wraps the shape already includes this offset.                                                                                                                     |
| disabled           |  `bool`  | If `true`, the shape cannot be moved or resized, and shows no resize handles.                                                                                                                                                                                                             |
| isBeingChanged     |  `bool`  | If `true`, the shape is currently being moved or scaled.                                                                                                                                                                                                                                  |
| active             |  `bool`  | If `true`, the shape has HTML-native focus or is selected via a `SelectionLayer`.                                                                                                                                                                                                         |
| nativeActive       |  `bool`  | If `true`, the shape has HTML-native focus (keyboard events will get applied to it).                                                                                                                                                                                                      |
| isInSelectionGroup |  `bool`  | If you assigned it via a prop on the WrappedShape, this will be available to tell whether or not the shape is in a group of two or more selected shapes (when using the `SelectionLayer` component). Useful for hiding the resize handles when selected in a group, or adding an outline. |
| _other props_      |   any    | Any extra props passed to the WrappedShape are passed down as-is to this component.                                                                                                                                                                                                       |

**WrappedShape Props**

| Prop                        |    Type     |  <div style="width: 100px; overflow-wrap: break-word;">Default</div>  | <div style="width: 400px;">Description</div>                                                                                                                                                                                                                                                                                                                                                       |
| :-------------------------- | :---------: | :-------------------------------------------------------------------: | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| height<br/>_(required)_     |  `number`   |                                                                       | Height of the shape.                                                                                                                                                                                                                                                                                                                                                                               |
| shapeId<br/>_(required)_    |  `string`   |                                                                       | Unique identifier for the shape, to aid in data handling.                                                                                                                                                                                                                                                                                                                                          |
| width<br/>_(required)_      |  `number`   |                                                                       | Width of the shape.                                                                                                                                                                                                                                                                                                                                                                                |
| x<br/>_(required)_          |  `number`   |                                                                       | x-axis offset of the shape.                                                                                                                                                                                                                                                                                                                                                                        |
| y<br/>_(required)_          |  `number`   |                                                                       | y-axis offset of the shape.                                                                                                                                                                                                                                                                                                                                                                        |
| onChange                    |   `func`    |                               `()=>{}`                                | Listener for transformation of this shape triggered by interactions with resize handles, panning, or keyboard shortcuts. Required for user-triggered shape transformations to work. Signature: `(newRect: { x: number, y: number, height: number, width: number }, WrappedShapeProps: object) => void`                                                                                             |
| onDelete                    |   `func`    |                               `()=>{}`                                | Listener for the deletion of this shape via `backspace` or `delete` keys. Required for user-triggered shape deletion to work. Signature: `(event: Event, WrappedShapeProps: object) => void`                                                                                                                                                                                                       |
| active                      |   `bool`    |                                `false`                                | If `true`, the shape is rendered as focused (particularly important when using a `SelectionLayer`). When not using a selection layer, this prop can be left unset, as native HTML focus will handle focus state.                                                                                                                                                                                   |
| constrainMove               |   `func`    |                       non-constraining function                       | A callback for restricting movement during shape transformations (e.g., to lock movement to one axis, keeping the shape inside a predefined boundary or snapping it to a grid). Signature: `({ originalX: number, originalY: number, x: number, y: number, width: number, height: number }) => ({ x: number, y: number })`                                                                         |
| constrainResize             |   `func`    |                       non-constraining function                       | A callback for restricting resizing during shape transformations (e.g., to lock resizing to one axis, keeping the shape inside a predefined boundary or snapping it to a grid). Signature: `({ originalMovingCorner: { x: number, y: number }, startCorner: { x: number, y: number }, movingCorner: { x: number, y: number }, lockedDimension: one of "x" or "y" }) => ({ x: number, y: number })` |
| disabled                    |   `bool`    |                                `false`                                | If `true`, the shape cannot be moved or resized, and shows no resize handles.                                                                                                                                                                                                                                                                                                                      |
| isInSelectionGroup          |   `bool`    |                                `false`                                | Whether or not the shape is in a group of two or more selected shapes (when using the `SelectionLayer` component). Prop is merely forwarded to the wrapped component to be used in customized rendering, e.g., hiding the resize handles when selected in a group, or adding an outline.                                                                                                           |
| keyboardTransformMultiplier |  `number`   |                                  `1`                                  | Multiplier for keyboard-triggered transforms, such as `↑↓←→` keys to move or `shift`+`↑↓←→` keys to resize. For example, with the default setting of `1`, pressing `→` would move the shape 1 px to the right. With a setting of `5`, it would move 5px.                                                                                                                                           |
| onKeyDown                   |   `func`    |                               `()=>{}`                                | Listener for shape `keydown` event. If `event.preventDefault()` is called inside, it will override the default keyboard shortcut behavior. Signature: `(event: Event, WrappedShapeProps: object) => void`                                                                                                                                                                                          |
| onBlur                      |   `func`    |                               `()=>{}`                                | Listener for shape `blur` event. Signature: `(event: Event, WrappedShapeProps: object) => void`                                                                                                                                                                                                                                                                                                    |
| onFocus                     |   `func`    |                               `()=>{}`                                | Listener for shape `focus` event. Signature: `(event: Event, WrappedShapeProps: object) => void`                                                                                                                                                                                                                                                                                                   |
| ResizeHandleComponent       | `Component` | [`DefaultResizeHandleComponent`](src/DefaultResizeHandleComponent.js) | The component to use for shape handles.                                                                                                                                                                                                                                                                                                                                                            |
| wrapperProps                |  `object`   |                                 `{}`                                  | Extra props to add to the SVG `<g>` element wrapping the shape.                                                                                                                                                                                                                                                                                                                                    |
| _other props_               |     any     |                                                                       | Any extra props passed to the WrappedShape are passed down as-is to the component being wrapped.                                                                                                                                                                                                                                                                                                   |

---

### ImageLayer

Renders an svg image element.

| Prop                 |   Type   | Default  | <div style="width: 400px;">Description</div>                                                        |
| :------------------- | :------: | :------: | :-------------------------------------------------------------------------------------------------- |
| src<br/>_(required)_ | `string` |          | URL for the image to display.                                                                       |
| onLoad               |  `func`  | `()=>{}` | Callback for the image load. Signature: `({ naturalWidth: number, naturalHeight: number }) => void` |

---

### DrawLayer

Creates an invisible layer of the SVG that allows users to draw shapes via mouse click-and-drag.

| Prop                        |          Type          | <div style="width: 100px; overflow-wrap: break-word;">Default</div> | <div style="width: 400px;">Description</div>                                                                                                                                                                                                                                                                                                                                                         |
| :-------------------------- | :--------------------: | :-----------------------------------------------------------------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| onAddShape<br/>_(required)_ |         `func`         |                                                                     | Callback for when a shape has finished being drawn. Use it to add the new shape to your data. Signature: `(newRect: { x: number, y: number, height: number, width: number }) => void`                                                                                                                                                                                                                |
| constrainMove               |         `func`         |                      non-constraining function                      | A callback for restricting the initial starting location for the drawing (e.g., to lock movement to one axis, keeping the shape inside a predefined boundary or snapping it to a grid). Signature: `({ originalX: number, originalY: number, x: number, y: number, width: number, height: number }) => ({ x: number, y: number })`                                                                   |
| constrainResize             |         `func`         |                      non-constraining function                      | A callback for restricting the dragged corner when drawing a shape (e.g., to lock resizing to one axis, keeping the shape inside a predefined boundary or snapping it to a grid). Signature: `({ originalMovingCorner: { x: number, y: number }, startCorner: { x: number, y: number }, movingCorner: { x: number, y: number }, lockedDimension: one of "x" or "y" }) => ({ x: number, y: number })` |
| DrawPreviewComponent        | `wrapShape(Component)` | [`DefaultDrawPreviewComponent`](src/DefaultDrawPreviewComponent.js) | The component to preview the shape while dragging. Must be wrapped with `wrapShape`.                                                                                                                                                                                                                                                                                                                 |
| onDrawStart                 |         `func`         |                              `()=>{}`                               | Called when a draw action begins. Signature: `({ startCorner: { x: number, y: number } }) => void`                                                                                                                                                                                                                                                                                                   |
| onDraw                      |         `func`         |                              `()=>{}`                               | Called mid-draw. Signature: `({ startCorner: { x: number, y: number }, movingCorner: { x: number, y: number } }) => void`                                                                                                                                                                                                                                                                            |
| onDrawEnd                   |         `func`         |                              `()=>{}`                               | Called when a draw action ends. `canceled` indicates if it ended due to being canceled via the Escape key. Signature: `({ startCorner: { x: number, y: number }, movingCorner: { x: number, y: number }, canceled: boolean }) => void`                                                                                                                                                               |

---

### SelectionLayer

Creates an invisible layer of the SVG that allows users to select shapes via mouse click-and-drag.

| Prop                              |          Type          |   <div style="width: 100px; overflow-wrap: break-word;">Default</div>   | <div style="width: 400px;">Description</div>                                                                                                                                                                                                                                                                                             |
| :-------------------------------- | :--------------------: | :---------------------------------------------------------------------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| selectedShapeIds<br/>_(required)_ |       `string[]`       |                                                                         | shapeIds that belong to the currently selected group. Should be populated with shapeIds passed back from `onSelectionChange`.                                                                                                                                                                                                            |
| onSelectionChange                 |         `func`         |                                `()=>{}`                                 | Listener for the addition or removal of shapes from the selection group (e.g., by shift+clicking extra shape or drawing new selection). Required for user-triggered selection to work. Signature: `(selectedShapeIds: string[]) => void`                                                                                                 |
| onChange                          |         `func`         |                                `()=>{}`                                 | Listener for transformation of shapes in the selection triggered by interactions with resize handles, panning, or keyboard shortcuts. Required for user-triggered shape transformations on selections to work. Signature: `(newRects: { x: number, y: number, height: number, width: number }[], selectedShapesProps: object[]) => void` |
| onDelete                          |         `func`         |                                `()=>{}`                                 | Listener for the deletion of all shapes in this selection via `backspace` or `delete` keys. Required for user-triggered shape deletion to work. Signature: `(event: Event, selectedShapesProps: object[]) => void`                                                                                                                       |
| SelectionDrawComponent            | `wrapShape(Component)` | [`DefaultSelectionDrawComponent`](src/DefaultSelectionDrawComponent.js) | The component to preview the selection while dragging. Must be wrapped with `wrapShape`.                                                                                                                                                                                                                                                 |
| SelectionComponent                | `wrapShape(Component)` |     [`DefaultSelectionComponent`](src/DefaultSelectionComponent.js)     | The component that visually wraps around the selected shapes (i.e., the selection outline). Must be wrapped with `wrapShape`.                                                                                                                                                                                                            |
| minimumDistanceForSelection       |        `number`        |                                  `15`                                   | Minimum height or width that the drawn selection must be in order to perform the selection.                                                                                                                                                                                                                                              |
| keyboardTransformMultiplier       |        `number`        |                                   `1`                                   | Multiplier for keyboard-triggered transforms, such as `↑↓←→` keys to move or `shift`+`↑↓←→` keys to resize. For example, with the default setting of `1`, pressing `→` would move shapes in the selection 1 px to the right. With a setting of `5`, they would move 5px.                                                                 |
| selectionComponentProps           |        `object`        |                                  `{}`                                   | Extra props to pass to the `SelectionComponent`.                                                                                                                                                                                                                                                                                         |
| children                          |  renderable elements   |                                 `null`                                  | `wrapShape`-wrapped shapes that are targets for selection by this selection group. Can also include other library components (`SelectionLayer`/`ImageLayer`/`DrawLayer`) or arbitrary SVG elements.                                                                                                                                      |

## Contributing

After cloning the repository and running `yarn install` inside, you can use the following commands to develop and build the project.

```sh
# Starts a dev server that hosts a demo page with the component.
yarn start

# Runs the library tests
yarn test

# Lints the code with eslint
yarn lint

# Lints and builds the code, placing the result in the dist directory.
# This build is necessary to reflect changes if you're
#  `npm link`-ed to this repository from another local project.
yarn build
```

Pull requests are welcome!

## Special Thanks

[![CrossBrowserTesting.com](https://user-images.githubusercontent.com/4413963/70622179-b48f1180-1c5e-11ea-9b77-e38d8e878d5a.png)](https://crossbrowsertesting.com/)

## License

MIT
