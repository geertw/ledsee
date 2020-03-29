<template>
  <div class="ledsign">
    <div v-for="col in parseInt(width)" v-bind:key="col" class="col">
      <LedCol :pixels="pixelArray[col - 1]" :height="height"></LedCol>
    </div>
  </div>
</template>

<script>
import LedCol from './LedCol.vue'
import font from '../fonts/5x7';

export default {
  name: 'LedSign',
  components: {
    LedCol
  },
  data() {
    return {
      offsetX: 0,
      moveX: 1
    }
  },
  props: {
    message: String,
    width: Number,
    height: Number
  },
  created() {
    setInterval(() => {
      this.move();
    }, 60);
  },
  methods: {
    move() {
      this.offsetX += this.moveX;
      if (this.offsetX >= this.pixelImage.length) {
        this.offsetX = 0;
      } else if (this.offsetX < 0) {
        this.offsetX = this.pixelImage.length - 1;
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

              pixels.push(colPixels)
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

      // Always end with 5 pixels spacing:
      for (let i = 0; i < 5; i++) {
        pixels.push([0, 0, 0, 0, 0, 0, 0]);
      }

      return pixels;
    },
    pixelArray() {
      let pixels = [];

      if (this.pixelImage.length == 0) {
        for (let i = 0; i <= this.width; i++) {
          pixels.push([0,0,0,0,0,0,0,0]);
        }

        return pixels;
      }

      pixels = pixels.concat(this.pixelImage.slice(this.offsetX));

      while (pixels.length < this.width) {
        pixels = pixels.concat(this.pixelImage);
      }

      return pixels;
    }
  }
}
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
