<template>
  <div class="aboutMe">
    <h2>About me</h2>
    <div class="description">
      <div>
        From the past years i've been working and building all kind of different projects. From fancy ux-apps, realtime data visualzation and even videogames!
      </div>
      <div class="content">
        <p>With a Bachellor Degree in Electrical Engineering from National Technologic University and a
        profound passion for software development, I've dedicated my career to
        mastering Python and its applications in UX research, data
        analysis, machine learning, and many other areas.</p>
        <p>
          My journey has taken me from game
        design to the intricate world of data science, always with one goal: to
        innovate and solve complex problems.
        </p>
        <p>
          In my free time, I enjoy building and designing all kind of webpages, further fueling my creativity and problem-solving skills.
        </p>  
      </div>
    </div>

    <!--<div class="img-carousel"></div>-->

  </div>

  
</template>

<script>
export default {
  name: 'AboutMe',
}
</script>

<style scoped>
.aboutMe {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.description {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.img-carousel {
  width: 100%;
  min-height: 10rem;
  background-image: url("/public/codeCarousel.png");
  background-size: 100%;
}

.content {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}
</style>
