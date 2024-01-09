<template>
  <main class="contenido">
    <div class="header">
      <div class="fixedHeader">
        <img class="logo" src="/logo.svg" @click="scrollTop"/>

        <div class="buttons">
          <button id="GITHUB">GITHUB</button>
          <button id="LINKEDIN">LINKEDIN</button>
        </div>
      </div>
    </div>

    <div class="body">
      <section class="welcome-wrapper">
        <Welcome />
      </section>

      <section id="aboutMe">
        <AboutMe/>
      </section>

      <section id="heatmapSection">
        <div id="heatmap-container">
          <HeatmapSection />
        </div>
      </section>

      <section id="workWithMeSection">
        <div class="workWithMe"></div>
      </section>
    </div>
  </main>
</template>

<script>
import HeatmapSection from "./components/HeatmapSection.vue";
import Welcome from "./components/Welcome.vue";
import AboutMe from "./components/AboutMe.vue";

export default {
  name: "App",
  components: {
    Welcome,
    HeatmapSection,
    AboutMe
  },
  methods: {
    fixedHeader() {
      const contenido = document.querySelector(".body");
      const header = document.querySelector(".fixedHeader");
      const buttons = document.querySelectorAll(".buttons button")
      const logo = document.querySelector('.logo')

      var height = contenido.getBoundingClientRect().top;

      //console.log("height: ", height);
      var headerBackgroundColor = "rgba(0,0,0,1)";

      if (-height > 50) {
        if (header.style.backgroundColor != headerBackgroundColor) {
          header.style.backgroundColor = headerBackgroundColor;
          header.style.paddingTop = "0.5rem";
          header.style.borderBottom = '1px solid #98e8cd'

          buttons.forEach((button) => {
            button.style.height = '2rem'
            button.style.width = '5rem'
            //button.innerHTML= ''
          })

          logo.style.height = '2rem'
        }
      } else {
        header.style.backgroundColor = null;
        header.style.paddingTop = "1rem";
        header.style.borderBottom = 'none'

        buttons.forEach((button) => {
            button.style.height = '3rem'
            button.style.width = '10rem'
            //button.innerHTML= button.id
          })

          logo.style.height = '3rem'
      }
    },

    scrollTop() {
      window.scrollTo({top: 0, behavior: 'smooth'});
    }
  },

  mounted() {
    console.log("onMounted");
    window.addEventListener("scroll", this.fixedHeader);
  },
};
</script>

<style>
* {
  margin: 0;
  padding-top: 0;
}

html {
  scroll-behavior: smooth;
  height: 100%;
}

#app {

  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0;
  padding: 0;
  color: white;
  font-family: "Hagrid-Regular";
  
  background-image: url("/public/background.png");
  background-size: 90%;
  background-repeat: repeat;
  animation: desplazamientoFondo 50s ease-in-out infinite alternate;
}

@keyframes desplazamientoFondo {
  0% {
    background-position-y: 0;
  }
  100% {
    background-position-y: 100%;
  }
}
.logo {
  transition: height 1s;
}

.logo:hover {
  cursor: pointer;
}

section {
  position: relative;
  display: flex;
  border: 1px solid rgba(100, 100, 100, 0.1);
  margin-right: 7%;
  margin-left: 7%;
  text-align: left;
  scroll-behavior: smooth;
}

header {
  box-sizing: border-box;
}

.fixedHeader {
  position: fixed;
  width: 100%;
  box-sizing: border-box;
  display: flex;
  justify-content: space-between;
  padding-top: 1rem;
  padding-bottom: 0.5rem;
  align-items: center;
  transition: background-color 1s, padding 1s;
  padding-left: 7%;
  padding-right: 7%;
  z-index: 1;
}

.fixedHeader .buttons {
  display: flex;
  gap: 2rem;
}

.buttons button {
  height: 3rem;
  width: 10rem;
  border: 1px solid #98e8cd;
  background: none;
  color: #98e8cd;
  border-radius: 1.5rem;
  transition: height 1s, width 1s, font-size 1s;
}

.buttons button:hover {
  cursor: pointer;
}

.body {
  padding-top: 15%;
  display: flex;
  flex-direction: column;
  gap: 10rem;
}
#heatmap-container {
  width: 100%;
  height: 100%;
}
.welcome-wrapper {
}

#heatmapSection {
  padding-top: 5rem;
}

h1 {
  font-size: 60px !important;
}
h2 {
  font-size: 30px;
}


@font-face {
  font-family: "Hagrid-Regular";
  src: url("/public/hagrid/Hagrid-Regular-trial.ttf") format("truetype");
}

@font-face {
  font-family: "Hagrid-Italic";
  src: url("/public/hagrid/Hagrid-Italic-trial.ttf") format("truetype");
}

@font-face {
  font-family: "Hagrid-Text-Extrabold";
  src: url("/public/hagrid/Hagrid-Text-Extrabold-trial.ttf") format("truetype");
}

@font-face {
  font-family: "Hagrid-Text-Extrabold-Italic";
  src: url("/public/hagrid/Hagrid-Text-Extrabold-Italic-trial.ttf")
    format("truetype");
}
</style>
