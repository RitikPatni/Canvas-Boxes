<template>
  <div id="app">
    <button @click="draw_mode = !draw_mode" class="button">
      Toggle Draw Mode:{{ draw_mode ? 'Off' : 'On' }}
    </button>
    <p>Hovering Box {{ current_box && current_box.id }}</p>
    <div @mousemove="updateCoordinates">
      <Canvas
        :ord="ord"
        :box_list="box_list"
        :canvas_transform="canvas_transform"
        :current_box="current_box"
        :draw_mode="draw_mode"
        :mouse_position="mouse_position"
        :refresh="refresh"
        :show_annotations="show_annotations"
        @box_hover="hoveringOnBox"
      />
    </div>
  </div>
</template>

<script>
import Canvas from './components/Canvas.vue';

export default {
  name: 'App',
  components: {
    Canvas,
  },
  data() {
    return {
      ord: 0,
      box_list: [
        {
          id: 1,
          soft_delete: false,
          label: {
            is_visible: true,
            colour: {
              rgba: {
                r: 239,
                g: 198,
                b: 155,
                a: 1,
              },
              hex: '#EFC69B',
            },
            name: 'Selected Box',
          },
          selected: true,
          x_min: 50,
          x_max: 270,
          y_min: 50,
          y_max: 150,
          special_condition: true,
        },
        {
          id: 2,
          soft_delete: false,
          label: {
            is_visible: true,
            colour: {
              rgba: {
                r: 175,
                g: 27,
                b: 63,
                a: 1,
              },
              hex: '#AF1B3F',
            },
            name: 'Random Box 2',
          },
          selected: false,
          x_min: 540,
          x_max: 760,
          y_min: 50,
          y_max: 150,
          special_condition: false,
        },
        {
          id: 3,
          soft_delete: false,
          label: {
            is_visible: true,
            colour: {
              rgba: {
                r: 71,
                g: 49,
                b: 68,
                a: 1,
              },
              hex: '#473144',
            },
            name: 'Box 3',
          },
          selected: false,
          x_min: 320,
          x_max: 510,
          y_min: 250,
          y_max: 450,
          special_condition: false,
        },
      ],
      current_box: null,
      refresh: false,
      mouse_position: {
        x: 0,
        y: 0,
        raw: {
          x: 0,
          y: 0,
        },
      },
      draw_mode: false,
      canvas_transform: {
        scale: 1,
      },
      show_annotations: true,
      rawMousePositions: {
        x: undefined,
        y: undefined,
      },
    };
  },
  methods: {
    updateCoordinates(event) {
      const bounds = event.target.getBoundingClientRect();
      const mousePositions = {
        x: event.clientX - bounds.left,
        y: event.clientY - bounds.top,
        raw: {
          x: event.clientX,
          y: event.clientY,
        },
      };
      this.mouse_position = mousePositions;
    },
    hoveringOnBox(i, mousePositionX, mousePositionY) {
      this.current_box = this.box_list[i];
      this.rawMousePositions.x = mousePositionX;
      this.rawMousePositions.y = mousePositionY;
    },
  },
};
</script>

<style>
/* http://meyerweb.com/eric/tools/css/reset/ 
   v2.0 | 20110126
   License: none (public domain)
*/

html,
body,
div,
span,
applet,
object,
iframe,
h1,
h2,
h3,
h4,
h5,
h6,
p,
blockquote,
pre,
a,
abbr,
acronym,
address,
big,
cite,
code,
del,
dfn,
em,
img,
ins,
kbd,
q,
s,
samp,
small,
strike,
strong,
sub,
sup,
tt,
var,
b,
u,
i,
center,
dl,
dt,
dd,
ol,
ul,
li,
fieldset,
form,
label,
legend,
table,
caption,
tbody,
tfoot,
thead,
tr,
th,
td,
article,
aside,
canvas,
details,
embed,
figure,
figcaption,
footer,
header,
hgroup,
menu,
nav,
output,
ruby,
section,
summary,
time,
mark,
audio,
video {
  margin: 0;
  padding: 0;
  border: 0;
  font-size: 100%;
  font: inherit;
  vertical-align: baseline;
}
/* HTML5 display-role reset for older browsers */
article,
aside,
details,
figcaption,
figure,
footer,
header,
hgroup,
menu,
nav,
section {
  display: block;
}
body {
  line-height: 1;
}
ol,
ul {
  list-style: none;
}
blockquote,
q {
  quotes: none;
}
blockquote:before,
blockquote:after,
q:before,
q:after {
  content: '';
  content: none;
}
table {
  border-collapse: collapse;
  border-spacing: 0;
}
#app {
  display: grid;
  place-items: center;
  padding: 2rem;
  gap: 1rem;
}
.button {
  background-color: #2c3e50;
  color: #fff;
  padding: 10px;
  border-radius: 4px;
  border: none;
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 10px;
  cursor: pointer;
}
</style>
