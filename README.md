# boxes

## Project setup

```bash
npm install
```

### Compiles and hot-reloads for development

```bash
npm run serve
```

### Compiles and minifies for production

```bash
npm run build
```

### Lints and fixes files

```bash
npm run lint
```

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).

## TODO

- [x] Task 1: TODO complete function, will convert n to integer
- [x] Task 2: TODO complete this function draw_circle
- [x] Task 3: TODO grab box_list from props
- [x] Task 4: TODO get other colours from props
- [x] Task 5: TODO handle if label is undefined
- [x] Task 6: TODO draw circles (using earlier created function) at box.[x/y]_min and box.[x/y]_max
- [x] Task 7: TODO draw lines between box.[x/y]_min and box.[x/y]_max
- [x] Task 8: TODO RGBA fill style
- [x] Task 9: TODO TODO if the mouse position is within the rectangle and/or circles we drew emit an event 'box_hover', with the current index 'i', this.mouse_position.raw.x and this.mouse_position.raw.y

### Room for improvement

- [ ] Right now hovering over the circle won't emit the event 'box_hover'
- [ ] Add a way to add a new box
- [ ] Add a way to remove a box
- [ ] Add a way to change the colour of a box
- [ ] Add a way to change the label of a box
- [ ] Add a way to change the size of a box
- [ ] Add a way to change the position of a box
- [ ] Another approach to detect mouse event right now it initializes the component every time the mouse moves
