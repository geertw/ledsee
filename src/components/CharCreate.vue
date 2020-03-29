<template>
    <div>
    <div>
        <div v-for="col in parseInt(width)" v-bind:key="col" class="col">
            <div v-for="row in parseInt(height)" v-bind:key="row" class="row">
                <span class="dot" :class="{ 'active': pixels[col - 1][row - 1] == 1 }" @click="toggle(col - 1, row - 1)"></span>
            </div>
        </div>
    </div>
    character code: {{ charDef }}

    <hr>

    <input type="text" v-model="charDef"><button @click="loadCharDef()">load</button>

    </div>
</template>

<script>
export default {
  name: 'CharCreate',
  data() {
      return {
          pixels: [],
          charDef: "[ 1, 2, 4, 0, 0 ]"
      }
  },
  created() {
      for (let col = 0; col < this.width; col++) {
          this.pixels[col] = [];
          for (let row = 0; row < this.height; row++) {
              this.$set(this.pixels[col], row, 0);
          }
      }
  },
  methods: {
      toggle(col, row) {
        let colPixels = this.pixels[col];
        colPixels[row] = 1 - colPixels[row];
        this.$set(this.pixels, col, colPixels);
      },
      loadCharDef() {
          let chars = JSON.parse(this.charDef);

        for (let col = 0; col < this.width; col++) {
            let colPixels = [];
            for (let row = 0; row < this.height; row++) {
                let pixelVal = (chars[col] & (1 << row)) === 0 ? 0 : 1;
                colPixels[row] = pixelVal;
            }
            this.$set(this.pixels, col, colPixels);
        }
    }
  },
  watch: {
      pixels() {
          let hexPixels = [];

          for (let col = 0; col < this.width; col++) {
              let colValue = 0;

              for (let row = 0; row < this.height; row++) {
                  colValue |= (this.pixels[col][row] << row);
              }

              hexPixels.push(colValue);
          }

          this.charDef =  JSON.stringify(hexPixels);
      }
  },
  props: {
    width: Number,
    height: Number
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.col {
  display: inline-block;
  line-height: 0;

  background: black;
}

.dot {
  width: 8px;
  height: 8px;
  /* border-radius: 8px; */
  background-color: #222;
  display: inline-block;
  /* margin: 1px; */
  cursor: pointer;
  border: 1px solid #000;
}
.dot.active {
  background-color: red;
}
.dot:hover {
  background-color: red;
  border: 1px solid red;
}

</style>
