<template>
  <span v-bind:style="{ transform: `translateY(${y}px) translateX(${x}px)` }">{{ char }}</span>
</template>

<style scoped>
span {
  position: absolute;
  transform-origin: left;
  /* transition: transform 10ms ease-in-out; */
}
</style>

<script>
export default {
  data() {
    return {
      x: 0,
      y: 0,
      running: true,
      char: ["👏", "🎉", "⭐️"][(Math.random() * 3) | 0]
    };
  },
  mounted() {
    this.x = Math.random() * (window.innerWidth / 2);
    this.fall();
  },
  methods: {
    fall(delta = 1) {
      this.y += (2 + Math.random() * 6) | 0;

      this.x = this.x - Math.sin(Math.random() * 10 * this.y);

      if (this.y > window.innerHeight) {
        this.running = false;
        console.log("stop falling");
        this.$emit("stop");
      } else {
        requestAnimationFrame(() => this.fall());
      }
    }
  }
};
</script>
