<template>
  <div class="root">
    <button v-on:click="fullscreen" class="zoom">
      <img src="/zoom.svg" />
    </button>
    <div v-if="playing">
      <span class="status">{{ index + 1 }}/{{ total }}</span>
      <img :src="images[index]" class="big" />
      <Progress :ttl="ttl" :key="index" v-on:done="nextImage"></Progress>
    </div>
    <div class="gameover instructions" v-else-if="gameover">
      <p class="tada animated infinite delay-2s">GAME OVER!</p>

      <button class="button" v-on:click="start">
        Start again
        <span v-if="index === 0">{{ loaded }}/{{ total }}</span>
      </button>
    </div>
    <div class="instructions" v-else>
      <p>This is slide show karaoke.</p>
      <p>
        You have
        <input type="number" min="2" v-model="total" />
        randomly selected images and
        <input type="number" min="2" v-model="ttl" /> seconds per slide to
        present your random story.
      </p>
      <p>Good luck…</p>
      <button class="button" v-on:click="start">
        Start now
        <span v-if="index === 0">{{ loaded }}/{{ total }}</span>
      </button>
    </div>
  </div>
</template>

<style scoped>
.status {
  position: absolute;
  top: 20px;
  left: 20px;
  opacity: 0.5;
}
.instructions {
  display: flex;
  flex-direction: column;
  height: 100%;
  align-items: center;
  justify-content: center;
  margin: 0 48px;
}

.instructions input[type='number'] {
  border-radius: 4px;
  font-family: inherit;
  font-size: inherit;
  background: rgba(0, 0, 0, 0.2);
  text-align: center;
  border: none;
  color: inherit;
  width: 100px;
}

.instructions input[type='number']:hover {
  background: black;
}

p {
  margin: 24px 0;
}

p:last-of-type {
  margin-bottom: 48px;
}

.zoom {
  position: absolute;
  top: 20px;
  right: 20px;
  cursor: pointer;
  background: none;
  border: none;
  opacity: 0.6;
}

.zoom:hover {
  opacity: 1;
}

.root {
  background: #222;
  color: #efefef;
  font-size: 48px;
  line-height: 56px;
}

:fullscreen .zoom {
  display: none;
}

.button {
  font-size: 48px;
  padding: 32px;
  border: 0;
  background: rgb(244, 67, 54);
  border-radius: 8px;
  color: rgb(255, 255, 255);
  cursor: pointer;
}

img.big {
  max-width: 100%;
  object-fit: contain;
  height: 100vh;
  display: block;
  margin: 0 auto;
}
</style>

<script>
import Progress from '../components/Progress';

console.log('🎉⭐️🎉⭐️🎉⭐️🎉⭐️🎉⭐️');

console.log(
  '%cSource image collection: https://unsplash.com/collections/1816930/images-for-slide-karaoke',
  'font-weight: bold; '
);
console.log(
  '%cSource code: https://github.com/remy/karaoke',
  'font-weight: bold'
);
console.log('🎉⭐️🎉⭐️🎉⭐️🎉⭐️🎉⭐️');

function getImage() {
  const accessKey =
    '16a858065e350acd52b8f598cb9cec6056c566f7f6852c6cfb530bb76d297e0d';
  const url = `https://api.unsplash.com/photos/random?collections=1816930&orientation=landscape&w=800&h=600&client_id=${accessKey}`;
  return fetch(url)
    .then((res) => {
      if (!res.ok) throw new Error('Failed to fetch image from Unsplash API');
      return res.json();
    })
    .then((data) =>
      data.urls && data.urls.regular ? data.urls.regular : data.urls.full
    );
}

export default {
  components: { Progress },
  data() {
    return {
      playing: false,
      gameover: false,
      index: -1,
      ttl: 20,
      loaded: 0,
      total: 5,
      images: [],
    };
  },
  methods: {
    fullscreen() {
      if (!document.fullscreenElement) {
        document.documentElement.requestFullscreen();
      }
    },
    nextImage() {
      this.index++;
      if (this.index === this.images.length) {
        this.gameover = true;
        this.playing = false;
      }
    },
    async start() {
      this.loaded = 0;
      this.index = 0;
      // populate the this.images
      this.images = await Promise.all(
        Array.from({ length: this.total }, () =>
          getImage().then((url) => {
            this.loaded++;
            return url;
          })
        )
      );

      // quickly check and replace if there's any dupes
      const dupes = [];
      this.images.forEach((_, i) => {
        if (this.images.lastIndexOf(_) !== i) {
          dupes.push(i);
        }
      });

      if (dupes.length) {
        await Promise.all(
          dupes.map((i) => {
            return getImage().then((url) => (this.images[i] = url));
          })
        );
      }

      this.playing = true;
    },
  },
};
</script>
