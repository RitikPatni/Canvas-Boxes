<template>
  <canvas ref="myCanvas" width="800" height="600" class="canvas-bg"></canvas>
</template>
<script>
/*
 * Instructions: complete all TODOs.
 *
 * Feel free to upgrade the code if you see areas of opportunity.
 * (Don't assume the starter code is good.)
 *
 * This VueJS component draws boxes on an HTML canvas
 * See boxes example image in folder
 * A box is defined by it's min and max x/y points.
 *
 * Bonus points - mock up input data and construct a test case.
 *
 */

import Vue from 'vue';
export default Vue.extend({
  props: {
    ord: {
      type: Number,
      default: 0,
    },
    box_list: {
      type: Array,
      required: true,
    },
    current_box: {
      type: Object,
      required: true,
    },
    refresh: {
      type: Boolean,
      default: false,
    },
    mouse_position: {
      type: Object,
      required: true,
    },
    draw_mode: {
      type: Boolean,
      default: false,
    },
    canvas_transform: {
      type: Object,
      required: true,
    },
    show_annotations: {
      type: Boolean,
      default: false,
    },
  },

  /* ord, order of drawing on canvas
   * box_list, list of dictionaries,
   * current_box, dictionary,
   * refresh, integer / hack for forcing reactive property change
   * mouse_position, dictionary, x, y
   * draw_mode, boolean
   * canvas_transform, dictionary,
   * show_annotations, boolean
   * */
  mounted() {
    this.init();
  },
  watch: {
    mouse_position: function () {
      if (!this.draw_mode) {
        this.init();
      }
    },
  },
  methods: {
    init: function () {
      const canvas = this.$refs.myCanvas;
      const ctx = canvas.getContext('2d');
      this.draw(ctx, () => ctx.closePath());
    },
    draw: function (ctx, done) {
      if (this.show_annotations) {
        let circle_size = 8 / this.canvas_transform['scale'];
        let font_size = 20 / this.canvas_transform['scale'];
        ctx.font = font_size + 'px Verdana';
        // TODO grab box_list
        let boxes = this.box_list;
        for (const i in boxes) {
          let box = boxes[i];
          if (!box.soft_delete) {
            if (box.label.is_visible) {
              ctx.beginPath();
              ctx.lineWidth = '2';
              // TODO get other colours
              const r = box.label?.colour.rgba.r || 255;
              const g = box.label?.colour.rgba.g || 255;
              const b = box.label?.colour.rgba.b || 255;
              const a = box.label?.colour.rgba.a || 255;
              const rgba = `rgba(${r},${g},${b},${a})`;

              if (box.selected) {
                ctx.fillStyle = 'blue';
              } else {
                ctx.fillStyle = rgba;
              }
              ctx.textAlign = 'start';
              // TODO handle if label is undefined
              this.add_label(
                box.label?.name || 'Figure',
                this.toInt(box.x_min),
                this.toInt(box.y_min),
                ctx
              );
              // TODO draw circles (using earlier created function) at box.[x/y]_min and box.[x/y]_max
              this.draw_circle(
                circle_size,
                this.toInt(box.x_min),
                this.toInt(box.y_min),
                ctx
              );
              this.draw_circle(
                circle_size,
                this.toInt(box.x_max),
                this.toInt(box.y_max),
                ctx
              );
              // TODO draw dashed line if special condition is true else draw solid line
              if (box.special_condition) {
                ctx.setLineDash([5, 5]);
              } else {
                ctx.setLineDash([]);
              }
              // TODO RGBA fill style
              ctx.fillStyle = rgba;
              // TODO draw rectangle
              this.draw_rectangle(
                this.toInt(box.x_min),
                this.toInt(box.y_min),
                this.toInt(box.x_max - box.x_min),
                this.toInt(box.y_max - box.y_min),
                ctx
              );
              if (!this.draw_mode) {
                if (
                  this.mouse_position.x >= box.x_min &&
                  this.mouse_position.x <= box.x_max &&
                  this.mouse_position.y >= box.y_min &&
                  this.mouse_position.y <= box.y_max
                ) {
                  /* TODO
                   * if the mouse position is within the rectangle and/or circles we drew
                   * emit an event 'box_hover', with the current index 'i',
                   * this.mouse_position.raw.x and this.mouse_position.raw.y
                   * */
                  this.$emit(
                    'box_hover',
                    i,
                    this.mouse_position.raw.x,
                    this.mouse_position.raw.y
                  );
                }
              }

              if (box.selected) {
                ctx.strokeStyle = 'blue';
              } else {
                ctx.strokeStyle = box.label.colour.hex;
              }
              ctx.stroke();
            }
          }
        }
      }
      done();
    },
    toInt: function (n) {
      return parseInt(n);
    },
    draw_circle: function (circle_size, x, y, ctx) {
      ctx.beginPath();
      ctx.arc(x, y, circle_size, 0, 2 * Math.PI);
      ctx.fill();
    },
    draw_rectangle: function (x, y, width, height, ctx) {
      ctx.beginPath();
      ctx.rect(x, y, width, height);
      ctx.closePath();
    },
    add_label: function (label, x, y, ctx) {
      ctx.beginPath();
      ctx.fillText(label, x, y);
      ctx.closePath();
    },
  },
});
</script>
<style >
.canvas-bg {
  background-image: url('https://source.unsplash.com/800x600/?cat,dog');
}
</style>