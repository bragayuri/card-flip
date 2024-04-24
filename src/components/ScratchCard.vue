<template>
  <div class="scratch-card" ref="scratchCard">
    <canvas
      ref="canvas"
      @mousedown="startScratch"
      @mouseup="endScratch"
      @mouseleave="endScratch"
      @mousemove="scratch"
      :width="width"
      :height="height"
    ></canvas>
    <div class="hidden-content" v-show="contentRevealed">
      <slot></slot>
      <!-- Use slot to pass in custom content from parent -->
    </div>
  </div>
</template>

<script>
export default {
  name: "ScratchCard",
  props: {
    id: { type: String, default: "" },
    width: {
      type: Number,
      default: 300,
    },
    height: {
      type: Number,
      default: 150,
    },
  },
  data() {
    return {
      isScratching: false,
      contentRevealed: false,
    };
  },
  mounted() {
    this.initCanvas();
  },
  methods: {
    initCanvas() {
      const canvas = this.$refs.canvas;
      const ctx = canvas.getContext("2d");
      ctx.fillStyle = "#c0c0c0"; // Set the color of the scratch-off layer
      ctx.fillRect(0, 0, this.width, this.height);
      ctx.globalCompositeOperation = "destination-out";
    },
    startScratch(event) {
      this.isScratching = true;
      this.scratch(event);
      event.stopPropagation(); // Prevent any parent handlers from being notified of the event
    },
    endScratch(event) {
      if (!this.isScratching) return; // Avoid redundant calls
      this.isScratching = false;
      this.scratch(event); // Optionally continue to scratch till the mouse is up
      event.stopPropagation(); // Stop the event from propagating to prevent card flip
      this.contentRevealed = this.checkReveal();
      this.$emit("scratching", false); // Notify the parent component scratching has ended
    },
    scratch(event) {
      if (!this.isScratching) return;
      const canvas = this.$refs.canvas;
      const ctx = canvas.getContext("2d");
      const rect = canvas.getBoundingClientRect();
      const x = event.clientX - rect.left;
      const y = event.clientY - rect.top;
      ctx.beginPath();
      ctx.arc(x, y, 20, 0, 2 * Math.PI);
      ctx.fill();
    },
    checkReveal() {
      const canvas = this.$refs.canvas;
      const ctx = canvas.getContext("2d");
      const imageData = ctx.getImageData(0, 0, this.width, this.height).data;
      let transparent = 0;
      for (let i = 3; i < imageData.length; i += 4) {
        if (imageData[i] === 0) transparent++;
      }
      const transparency = transparent / (this.width * this.height);
      if (transparency > 0.5 && !this.contentRevealed) {
        this.contentRevealed = true;

        this.$emit("contentRevealed", this.id);
        return true;
      }
      return false;
    },
  },
};
</script>

<style>
.scratch-card {
  position: relative;
  overflow: hidden;
}
.hidden-content {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #333;
  font-size: 20px;
}
</style>
