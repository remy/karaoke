<template>
  <div class="progress">
    <div ref="progress" class="current"></div>
  </div>
</template>

<style scoped>
.progress {
  width: 100%;
  z-index: 10;
  position: absolute;
  bottom: 0;
  left: 0;
  height: 40px;
  right: 0;
}

.current {
  width: 100vw;
  background: rgba(0, 255, 0, 0.4);
  height: 40px;
  left: 0;
  bottom: 0;
}
</style>


<script>
export default {
  props: { ttl: Number },
  mounted() {
    const el = this.$refs.progress;
    const animation = el.animate(
      [{ transform: "translateX(0vw)" }, { transform: "translateX(-100vw)" }],
      this.ttl * 1000
    );

    animation.onfinish = () => {
      this.$emit("done");
    };
  }
};
</script>
