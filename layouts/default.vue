<template>
  <Nuxt/>
</template>

<script>
export default {
  head: {
    title: "slide show karaoke",
    meta: [
      { charset: "utf-8" },
      { name: "viewport", content: "width=device-width, initial-scale=1" }
    ],
    link: [
      {
        rel: "icon",
        sizes: "192x192",
        href: "/favicon.png"
      },
      {
        href: "https://fonts.googleapis.com/css?family=Rubik",
        rel: "stylesheet"
      },
      { rel: "stylesheet", href: "animate.min.css" },
      { href: "/style.css", rel: "stylesheet" }
    ]
  }
};
</script>
