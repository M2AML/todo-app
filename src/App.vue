<script setup>
import TodoList from "./components/todoList.vue";
import sunIcon from "./assets/images/icon-sun.svg";
import moonIcon from "./assets/images/icon-moon.svg";
import { ref } from "vue";

const switcherIcon = ref(sunIcon);
const bgImgTheme = ref("bgDark");
const activeTheme = ref("dark");
const themeSwitcher = () => {
  if (switcherIcon.value === sunIcon) {
    switcherIcon.value = moonIcon;
    bgImgTheme.value = "bgLight";
    activeTheme.value = "light";
  } else {
    switcherIcon.value = sunIcon;
    bgImgTheme.value = "bgDark";
    activeTheme.value = "dark";
  }
};
</script>

<template>
  <div id="bg_img" :class="`${bgImgTheme}`"></div>
  <div id="bg_color" :class="`${activeTheme}`"></div>
  <main class="container">
    <header>
      <h1>TODO</h1>
      <button @click="themeSwitcher" class="btn_switcher">
        <img :src="`${switcherIcon}`" alt="sun icon" />
      </button>
    </header>
    <TodoList :id="`${activeTheme}`" />
    <p class="drag_drop">Drag and Drop to reorder list</p>
  </main>
  <footer :class="`${activeTheme}`">
    Challenge by
    <a href="https://www.frontendmentor.io?ref=challenge" target="_blank"
      >Frontend Mentor</a
    >. Coded by <a href="#">m2aml</a>.
  </footer>
</template>

<style scoped>
#bg_img {
  position: absolute;
  top: 0;
  width: 100%;
  height: 300px;
  transition: all 0.5s ease;
}
#bg_color {
  position: absolute;
  top: 0;
  z-index: -1;
  width: 100%;
  height: 100%;
  transition: all 0.5s ease;
}
#bg_color.dark {
  background: hsl(235, 21%, 11%);
}
#bg_color.light {
  background: hsl(0, 0%, 98%);
}
.bgLight {
  background-image: url("./assets/images/bg-desktop-light.jpg");
  background-size: cover;
}
.bgDark {
  background-image: url("./assets/images/bg-desktop-dark.jpg");
  background-size: cover;
}
.container {
  margin: auto;
  padding-top: 80px;
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 40%;
  height: auto;
}
.btn_switcher {
  all: unset;
  cursor: pointer;
}
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  width: 100%;
  font-weight: 400;
}
header h1 {
  letter-spacing: 10px; 
  font-size: 3rem;
  font-weight: 700;
  color: hsl(0, 0%, 98%);
  margin-bottom: 15px;
}
header img {
  width: 40px;
}
.drag_drop {
  margin-top: 32px;
  color: hsl(235, 19%, 35%);
  font-size: 0.9rem;
}
footer {
  position: absolute;
  bottom: 0;
  text-align: center;
  width: 100%;
  transition: all 0.5s ease;
  font-size: 1.2rem;
  padding: 8px;
}
footer a {
  all: unset;
  color: hsl(220, 98%, 61%);
  cursor: pointer;
}
footer.dark {
  color: hsl(0, 0%, 98%);
}
footer.light {
  color: hsl(235, 21%, 11%);
}
@media screen and (max-width: 780px) {
  .container {
    width: 85%;
  }
  .bgLight {
    background-image: url("./assets/images/bg-mobile-light.jpg");
  }
  .bgDark {
    background-image: url("./assets/images/bg-mobile-dark.jpg");
  }
  footer {
    margin-top: 10px;
  }
}
@media screen and (max-width: 500px) {
  #bg_img {
    height: 260px;
  }
}
</style>
