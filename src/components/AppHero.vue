<template>
  <h1 class="sr-only">Flip-O-Rama</h1>
  <img
    srcset="/image/flip-o-rama-title@2x.png 2x, /image/flip-o-rama-title.png 1x"
    src="/image/flip-o-rama-title.png"
    alt="Flip-O-Rama"
    class="title"
  />
  <section :class="$style.description">
    <p>Welcome to Flip-O-Rama, a Halloween themed card matching game for my dearest son Edward.</p>
  </section>
</template>

<style module>
.description {
  font-family: 'Titillium Web', sans-serif;
}
.description p {
  margin: 0;
  font-size: 1rem;
  padding-bottom: 30px;
}
.title {
  padding-bottom: 30px;
}
</style>