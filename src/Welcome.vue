<template>
  <div class="welcomeMsg">
          <h3>Welcome to my personal webpage!</h3>
          <h1>I am <b>Nicolas Martinez</b></h1>
          <h2>Join me on a journey where code meets the cosmos.</h2>
          <a href="#aboutMe" class="learnMore a">Learn more</a>
        </div>
</template>

<script>
export default {
  name: 'WelcomeMsg',
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.welcomeMsg {
  display: flex;
  flex-direction: column;
  justify-content: left;
  align-items: left;
  text-align: left;
  width: 70%;
}

.welcomeMsg b {
  color: #98e8cd;
}

.learnMore {
  
  height: 3rem;
  width: 10rem;
  border: 1px solid #98e8cd;
  background: none;
  color: #98e8cd;
  border-radius: 1.5rem;
  margin-top: 2rem;
  text-decoration: none;
  display: flex;
  align-items: center ;
  justify-content: center;
  text-align: center;
}

.a {
  line-height: 3rem;
  padding: 0;
  margin-top: 2rem;
  text-decoration: none;
  font-weight: 300 !important;
  font-family: 'Hagrid-Regular';
  display: flex;
  height: 3rem;
}


</style>
