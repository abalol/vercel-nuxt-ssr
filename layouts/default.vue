<template>
  <div>
    <Nuxt />
  </div>
</template>

<style>
/* A Modern CSS Reset */
*,
*::before,
*::after {
  box-sizing: border-box;
}
ul[class],
ol[class] {
  padding: 0;
}
body,
h1,
h2,
h3,
h4,
p,
ul[class],
ol[class],
figure,
blockquote,
dl,
dd {
  margin: 0;
}
html {
  scroll-behavior: smooth;
  font-family: 'Noto Sans JP';
  color: #333;
}
body {
  min-height: 100vh;
  text-rendering: optimizeSpeed;
  line-height: 1.5;
  background-color: #f0f4f4;
}
ul[class],
ol[class] {
  list-style: none;
}
a:not([class]) {
  text-decoration-skip-ink: auto;
}
img {
  max-width: 100%;
  display: block;
}
article > * + * {
  margin-top: 1em;
}
input,
button,
textarea,
select {
  border: 1px solid #135487;
}
input:focus,
button:focus,
textarea:focus,
input:focus {
  outline: none;
}
input {
  padding: 8px;
  width: 100%;
  margin: 0 0 4px 0;
}
textarea {
  padding: 8px;
  width: 100%;
  height: 300px;
}
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
</style>
