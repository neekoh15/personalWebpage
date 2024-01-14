<template>
  <h2>Apps section</h2>

  <div class="apps">
    <div class="app" name="ISSTracker">
        Watch the ISS in real time!
        <div class="worldMap">
        </div>
    </div>
    
  </div>
</template>

<script>
import { onMounted } from "vue";

export default {
  name: "AppsSection",
  setup() {


    onMounted(() => {
    });

    return {
    };
  },
};

</script>

<style scoped>

.worldMap {
  width: 100%;
  min-height: 38rem;
  background-image: url('/public/worldMap.png');
  background-size: 100%;
  border-radius: 10rem;
}
</style>
