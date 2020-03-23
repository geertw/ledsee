<template>
  <div class="ledsign">
    <div v-for="col in parseInt(width) - 1" v-bind:key="col" class="col">
      <LedCol :pixels="pixelArray[col]" :height="height"></LedCol>
      <!-- <div v-for="row in parseInt(height)" v-bind:key="row" class="row">
        <span class="dot active"></span>
      </div> -->
    </div>
  </div>
</template>

<script>
import LedCol from './LedCol.vue'


export default {
  name: 'LedSign',
  components: {
    LedCol
  },
  data() {
    return {
      offset: 0
    }
  },
  props: {
    msg: String,
    width: Number,
    height: Number
  },
  created() {
    setInterval(() => {
      this.move();
    }, 50);
  },
  methods: {
    move() {
      this.offset++;
      if (this.offset >= this.pixelImage.length) {
        this.offset = 0;
      }
    }
  },
  computed: {
    pixelImage() {
      let pixels  =[];
      for (let i = 0; i < 50; i++) {
        if (i % 4 == 0) {
          pixels.push([0,1,0,1,0,1,0,1]);
          // pixels.push([1,1,1,1,1,1,1,1]);
        } else if (i % 4 == 1) {
          pixels.push([1,0,1,0,1,0,1,0]);
        } else if (i % 4 == 2) {
          pixels.push([0,1,0,1,0,1,0,1]);
        } else {
          pixels.push([1,0,1,0,1,0,1,0]);
          // pixels.push([0,0,0,0,0,0,0,0]);
        }
      }

      pixels.push([1,1,1,1,1,1,1,1]);
      pixels.push([1,1,1,1,1,1,1,1]);
      pixels.push([1,1,1,1,1,1,1,1]);

      pixels.push([0,0,0,0,0,0,0,0]);
      pixels.push([0,0,0,0,0,0,0,0]);
      pixels.push([0,0,0,0,0,0,0,0]);


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

      // console.log(this.pixelImage.slice(this.offset));

      pixels = pixels.concat(this.pixelImage.slice(this.offset));

      while (pixels.length < this.width) {
        pixels = pixels.concat(this.pixelImage);
      }

      // for (let i = 0; i <= this.width; i++) {
      //   pixels.push([0,0,0,0,0,0,0,0]);
      // }

      return pixels;
      // return this.pixelImage;

      // return pixels;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.col {
  display: inline-block;
  line-height: 0;
}

/* .dot {
  width: 4px;
  height: 4px;
  border-radius: 4px;
  background-color: #222;
  display: inline-block;
  margin: 2px;
}
.dot.active {
  background-color: red;
} */
.ledsign {
  background: #111;
  display: inline-block;
  padding: 16px;
  border-radius: 4px;
}
</style>
