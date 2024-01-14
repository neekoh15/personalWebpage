<template>
  
  <main class="contenido">
    
    <div class="header">
      <div class="fixedHeader">
        <button v-if="heatmapOn.value" id="heatmap" @click="clearHeatmap">
            CLEAR HEATMAP
          </button>
        <label class="logo-container" @click="scrollTop">
          <img class="logo" src="/logo.svg" />
          <div id="logo-name">
            <div id="name"></div>
            <div id="lastName"></div>
          </div>
        </label>

        <div class="buttons">
          <div class="sectionButtons">
            <button @click.prevent="goTo('#aboutMe')" class="headerButtons">
              ABOUT ME
            </button>
            <button @click.prevent="goTo('#appsSection')" class="headerButtons">
              APPS
            </button>
            <button
              @click.prevent="goTo('#workWithMeSection')"
              class="headerButtons"
            >
              CONTACT
            </button>
          </div>

          <div class="logoButtons">
            <img
            id="GITHUB"
            @click="linkTo($event)"
            src="/icons/github.svg"
            class="socialLogo"
          />
          <img
            id="LINKEDIN"
            @click="linkTo($event)"
            src="/icons/linkedin.svg"
            class="socialLogo"
          />
          </div>

          

          
        </div>
      </div>
    </div>

    <div class="body">
      
      <section class="welcome-wrapper">
        <Welcome />
      </section>

      <div>
        <section id="aboutMe">
          <AboutMe />
        </section>
        <div class="imgCarousel"></div>
      </div>

      <section id="appsSection">
        <div id="appsSection-container">
          <AppsSection />
        </div>
        <button v-if="!heatmapOn.value" id="HEATMAP" @click="enableHeatmap">
          HEATMAP
        </button>
      </section>

      <section id="workWithMeSection">
        <div class="workWithMe">
          <WorkWithMe />
        </div>
      </section>

      <footer></footer>
    </div>
  </main>
</template>

<script>
import AppsSection from "./components/AppsSection.vue";
import Welcome from "./components/Welcome.vue";
import AboutMe from "./components/AboutMe.vue";
import WorkWithMe from "./components/WorkWithMe.vue";

import { heatmapPoints } from "./store/store.js";
import { ref } from "vue";

import Heatmap from "heatmap.js";

export default {
  name: "App",
  components: {
    Welcome,
    AppsSection,
    AboutMe,
    WorkWithMe,
  },

  data() {
    this.heatmapOn = ref(false);
  },

  methods: {
    fixedHeader() {
      const contenido = document.querySelector(".body");
      const header = document.querySelector(".fixedHeader");
      const buttonsDiv = document.querySelector(".buttons");
      const buttons = document.querySelectorAll(".buttons button");
      const logo = document.querySelector(".logo");
      const socialLogos = document.querySelectorAll('.socialLogo')
      const logoFirstName = document.querySelector("#name");
      const logoLastName = document.querySelector("#lastName");

      var height = contenido.getBoundingClientRect().top;

      var headerBackgroundColor = "rgba(0,0,0,1)";
      headerBackgroundColor = "#2c3e50";

      if (-height > 50) {
        if (header.style.backgroundColor != headerBackgroundColor) {
          header.style.backgroundColor = headerBackgroundColor;
          header.style.paddingTop = "0.5rem";
          header.style.borderBottom = "1px solid #98e8cd";
          buttonsDiv.style.gap = "1rem";

          buttons.forEach((button) => {
            button.style.height = "2rem";
            button.style.width = "6rem";
            //button.innerHTML= ''
          });

          logo.style.height = "2rem";

          socialLogos.forEach((logo)=> {
          logo.style.height ="1.5rem"
        })

          logoFirstName.innerHTML = "Nicolas";
          logoFirstName.style.color = "white";

          logoLastName.innerHTML = "Martinez";
          logoLastName.style.color = "#98e8cd";
        }
      } else {
        header.style.backgroundColor = null;
        header.style.paddingTop = "1rem";
        header.style.borderBottom = "none";

        buttons.forEach((button) => {
          button.style.height = "3rem";
          button.style.width = "8rem";
          //button.innerHTML= button.id
        });
        logo.style.height = "3rem";

        socialLogos.forEach((logo)=> {
          logo.style.height ="2.5rem"
        })


        logoFirstName.innerHTML = "Nicolas";
        logoFirstName.style.color = "rgba(0, 0, 0, 0)";

        logoLastName.innerHTML = "Martinez";
        logoLastName.style.color = "rgba(0, 0, 0, 0)";
        buttonsDiv.style.gap = "2rem";
      }
    },

    scrollTop() {
      window.scrollTo({ top: 0, behavior: "smooth" });
    },

    linkTo(event) {
      if (event.target.id == "GITHUB") {
        window.open("https://github.com/neekoh15", "_blank");
      } else if (event.target.id == "LINKEDIN") {
        window.open(
          "https://www.linkedin.com/in/nicmartinez93/?locale=en_US",
          "_blank"
        );
      }
    },

    goTo(divId) {
      var elmnt = document.querySelector(divId);
      elmnt.scrollIntoView();
    },

    enableHeatmap() {
      this.heatmapOn.value = true;
      this.heatmapInstance.setData({ max: 5, data: heatmapPoints.value });
      [...document.getElementsByClassName("heatmap-canvas")].forEach(
        (e) => (e.style.opacity = 1)
      );

      document.onmousemove = this.continousPainting

      //heatmapPoints.value = [];
    },

    continousPainting(ev) {
      const point = {
        x: ev.pageX,
        y: ev.pageY,
        value: 5,
      };

      this.heatmapInstance.addData(point);
    },

    clearHeatmap() {
      [...document.getElementsByClassName("heatmap-canvas")].forEach((e) => {
        e.style.opacity = 0;
      });

      setTimeout(() => {
        [...document.getElementsByClassName("heatmap-canvas")].forEach((e) => {
          e.remove();
        });

        this.heatmapInstance = Heatmap.create({
          container: this.HeatmapContainer,
          radius: 70,
        });
      }, 2000);

      this.heatmapOn.value = false;
    },
  },

  mounted() {
    this.HeatmapContainer = document.querySelector(".contenido");

    this.heatmapInstance = Heatmap.create({
      container: this.HeatmapContainer,
      radius: 70,
    });

    window.addEventListener("scroll", this.fixedHeader);
    window.onmousemove = function (ev) {
      const point = {
        x: ev.clientX + window.scrollX,
        y: ev.clientY + window.scrollY,
        value: 1,
      };

      /*if (heatmapPoints.value.length > 1000) {
        heatmapPoints.value.shift();
      }*/
      heatmapPoints.value.push(point);
    };
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
  font-family: Helvetica, Arial, sans-serif;
  font-family: "Hagrid-Regular";
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0;
  padding: 0;
  color: white;

  background-image: url("/public/background.png");
  background-size: 90%;
  background-repeat: repeat;
  animation: desplazamientoFondo 100s ease-in-out infinite alternate;
  font-size: 17px;
}

.logo-container {
  display: flex;
  align-items: center;
  flex-direction: row;
  gap: 0.3rem;
}

#logo-name {
  display: flex;
  flex-direction: row;
  gap: 0.3rem;
}

#logo-name {
  cursor: pointer;
}

a {
  line-height: 3rem;
  padding: 0;
  text-decoration: none;
  font-weight: 300 !important;
  font-size: 14px;
  font-family: "Hagrid-Regular";
  display: flex;
  height: 3rem;

  width: 10rem;
  border: 1px solid #98e8cd;
  background: none;
  color: #98e8cd;
  border-radius: 1.5rem;

  text-decoration: none;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
}

@keyframes desplazamientoFondo {
  0% {
    background-position-y: 0;
  }
  100% {
    background-position-y: 100%;
  }
}

@keyframes desplazamiendoHorizontal {
  0% {
    background-position-x: 0;
  }
  100% {
    background-position-x: 100%;
  }
}

.logo {
  transition: height 1s;
  height: 3rem;
}

.socialLogo {
  transition: height 1s;
  height: 2.5rem;
}

.logo:hover {
  cursor: pointer;
}

#name {
  transition: color 1s;
}

#lastName {
  transition: color 1s;
}

section {
  position: relative;
  display: flex;
  flex-direction: column;
  margin-right: 7%;
  margin-left: 7%;
  text-align: left;
  scroll-behavior: smooth;
  z-index: 1;
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
  z-index: 10;
}

.fixedHeader .buttons {
  display: flex;
  gap: 2rem;
  transition: gap 1s;
}

.buttons button {
  height: 3rem;
  width: 8rem;
  border: 1px solid #98e8cd;
  background: none;
  color: #98e8cd;
  border-radius: 1.5rem;
  transition: height 1s, width 1s, font-size 1s;
  font-weight: 700;
}

.headerButtons {
  border: none !important;
}

.logoButtons {
  display: flex;
  gap: 1rem;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.buttons button:hover {
  cursor: pointer;
}

.sectionButtons {
  display: flex;
  justify-content: center;
  align-items: center;
}

.body {
  padding-top: 15%;
  display: flex;
  flex-direction: column;
  gap: 10rem;
}


#appsSection-container {
  width: 100%;
  height: 100%;
}

#appsSection {
  padding-top: 0rem;
}

h1 {
  font-size: 60px !important;
}
h2 {
  font-size: 30px;
}

.contenido {
  width: 100%;
  height: 100%;
  z-index: 1;
  background-color: rgba(0,0,0, 0.3);
}

.heatmap-canvas {
  width: 100%;
  height: 100%;
  opacity: 0;
  transition: opacity 2s;
}

footer {
  background-color: #2c3e50;
  height: 10rem;
}

#heatmap {
  position: fixed;
  left: 85%;
  top: 80%;
  height: 3rem;
  width: 10rem;
  border-radius: 0.3rem;
  background-color: #98e8cd;
  border: none;
  color: #2c3e50;
  cursor: pointer;
  font-weight: bold;
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
