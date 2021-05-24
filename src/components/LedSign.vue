<template>
  <div class="ledsign">
    <div v-for="col in parseInt(width)" v-bind:key="col" class="col">
      <LedCol :pixels="pixelArray[col - 1]" :height="height"></LedCol>
    </div>
  </div>
</template>

<script>
import LedCol from "./LedCol.vue";
// import font from "../fonts/5x7";
import font from "../fonts/arial";

export default {
  name: "LedSign",
  components: {
    LedCol
  },
  data() {
    return {
      offsetX: 0,
      moveX: 1,
    };
  },
  props: {
    message: String,
    width: Number,
    height: Number,
    scrollMode: {
      type: String,
      default: "auto"
    },
    align: {
      type: String,
      default: "center"
    },
    scrollSpacing: {
      type: Number,
      default: 5
    },
  },
  created() {
    setInterval(() => {
      this.move();
    }, 60);
  },
  methods: {
    move() {
      this.offsetX += this.moveX;
      if (this.offsetX >= this.pixelImage.length + this.scrollSpacing) {
        this.offsetX = 0;
      } else if (this.offsetX < 0) {
        this.offsetX = this.pixelImage.length + this.scrollSpacing - 1;
      }
    }
  },
  computed: {
    pixelImage() {
      let pixels = [];

      for (let i = 0; i < this.message.length; i++) {
        let char = this.message[i];

        if (Object.prototype.hasOwnProperty.call(font, char)) {
          let charDef = font[char];

          for (let col = 0; col < charDef.length; col++) {
            let colPixels = [];

            for (let row = 0; row < this.height; row++) {
              let pixelVal = (charDef[col] & (1 << row)) === 0 ? 0 : 1;
              colPixels[row] = pixelVal;
            }

            pixels.push(colPixels);
          }
        } else {
          // Unrecognized character
          pixels.push([1, 1, 1, 1, 1, 1, 1]);
          pixels.push([1, 1, 1, 1, 1, 1, 1]);
          pixels.push([1, 1, 1, 1, 1, 1, 1]);
          pixels.push([1, 1, 1, 1, 1, 1, 1]);
          pixels.push([1, 1, 1, 1, 1, 1, 1]);
        }

        pixels.push([0, 0, 0, 0, 0, 0, 0]);
      }

      return pixels;
    },
    pixelArray() {
      let scrolling = false;
      let pixels = [];

      if (this.pixelImage.length == 0) {
        for (let i = 0; i < this.width; i++) {
          let pixelCol = [];
          for (let y = 0; y < this.height; y++) {
            pixelCol.push(0);
          }
          pixels.push(pixelCol);
        }

        return pixels;
      }

      scrolling =
        this.scrollMode == "scroll" ||
        (this.scrollMode == "auto" && this.shouldScroll);

      if (scrolling) {
        pixels = pixels.concat(this.pixelImage.slice(this.offsetX));

        let offsetSpacing = Math.min(0, this.pixelImage.length - this.offsetX);

        // Always end with spacing:
        for (let i = 0; i < this.scrollSpacing + offsetSpacing; i++) {
          let pixelCol = [];
          for (let y = 0; y < this.height; y++) {
            pixelCol.push(0);
          }
          pixels.push(pixelCol);
        }

        while (pixels.length < this.width) {
          pixels = pixels.concat(this.pixelImage);

          // Always end with spacing:
          for (let i = 0; i < this.scrollSpacing; i++) {
            let pixelCol = [];
            for (let y = 0; y < this.height; y++) {
              pixelCol.push(0);
            }
            pixels.push(pixelCol);
          }
        }
      } else {
        let pixelsLeft = 0;
        let pixelsRight = 0;

        if (this.align == "left") {
          // Left:
          pixelsLeft = 0;
          pixelsRight = this.width - this.pixelImage.length - pixelsLeft;
        } else if (this.align == "right") {
          // Right:
          pixelsRight = 0;
          pixelsLeft = this.width - this.pixelImage.length - pixelsRight;
        } else {
          // Center:
          pixelsLeft = Math.round((this.width - this.pixelImage.length) / 2);
          pixelsRight = this.width - this.pixelImage.length - pixelsLeft;
        }

        for (let i = 0; i < pixelsLeft; i++) {
          let pixelCol = [];
          for (let y = 0; y < this.height; y++) {
            pixelCol.push(0);
          }
          pixels.push(pixelCol);
        }

        pixels = pixels.concat(this.pixelImage);

        for (let i = 0; i < pixelsRight; i++) {
          let pixelCol = [];
          for (let y = 0; y < this.height; y++) {
            pixelCol.push(0);
          }
          pixels.push(pixelCol);
        }
      }

      return pixels;
    },
    shouldScroll() {
      return this.width < this.pixelImage.length;
    }
  }
};
</script>

<style scoped>
.col {
  display: inline-block;
  line-height: 0;
}

.ledsign {
  background: #111;
  display: inline-block;
  padding: 16px;
  border-radius: 4px;
}
</style>
