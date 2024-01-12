<template>
  <h2>Apps section</h2>
  <div class="apps">
    <div class="app" name="ISSTracker">
      Watch the ISS in real time!
      <div class="worldMap" ref="mapElement" id="container">
        <!-- Add a marker or dot to represent the satellite -->
      </div>
    </div>
  </div>
</template>

<script>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";
import { getLatLngObj } from "tle.js";

export default {
  name: "AppsSection",
  data() {
    this.satelliteData = [
      {
        name: "ISS (ZARYA)",
        tle: `ISS (ZARYA)
        1 25544U 98067A   24011.38118347  .00015819  00000-0  28198-3 0  9998
        2 25544  51.6404  17.2554 0003852  20.7619 339.3526 15.50260516434080`,
        altitude: 110,
        radius: 2,
        x: 0,
        y: 0,
        z: 0,
        pointMesh: null,
        stella: null,
        stellaPoints: [],
      },
    ];

    this.scene = new THREE.Scene();
    this.camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    );

    this.renderer = new THREE.WebGLRenderer({ alpha: true });

    this.textureLoader = new THREE.TextureLoader();

    this.earth = {
      radius: 100,
      geometry: null,
      material: null,
      mesh: null,
      texture: null,
      satellites: [],
    };

    this.controls = new OrbitControls(this.camera, this.renderer.domElement);

    this.actualTime = new Date().getTime();
  },

  mounted() {
    this.setupEarth();
    this.setupControls();
    this.createScene();
    this.animationLoop();
  },

  methods: {
    createScene() {
      // Scene setup
      const container = document.getElementById("container");
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      container.appendChild(this.renderer.domElement);

      // Lighting
      this.createAmbientLight();

      //scene.add(camera);
      this.scene.add(this.camera);

      this.satelliteData.forEach((satellite) => {
        this.addPointToEarth(satellite);
      });

      //scene.add(earthMesh);
      this.scene.add(this.earth.mesh);
      // Camera position
      this.camera.position.z = this.earth.radius * 3;
    },

    setupEarth() {
      this.earth.geometry = new THREE.SphereGeometry(this.earth.radius, 32, 32);
      this.earth.material = new THREE.MeshLambertMaterial();
      this.earth.mesh = new THREE.Mesh(
        this.earth.geometry,
        this.earth.material
      );

      const texture = this.textureLoader.load(
        "/earthModel/equirectangular-projection.jpg"
      );

      this.earth.material.map = texture;
      this.earth.material.transparent = true;

      // GLOW EFFECT ----
      const glowEffect = this.createGlowEffect();
      this.earth.mesh.add(glowEffect);
    },

    setupControls() {
      // Set up OrbitControls
      this.controls.enableDamping = true; // Optional, but this gives a nice inertia to the rotation
      this.controls.dampingFactor = 0.1;
      this.controls.rotateSpeed = 0.5; // Adjust the speed of rotation
      this.controls.enableZoom = false;
      this.controls.autoRotate = false;
      this.controls.enablePan = false;
    },

    createAmbientLight() {
      const ambientLight = new THREE.AmbientLight(0xffffff, 10);
      //scene.add(ambientLight);
      this.scene.add(ambientLight);
      const pointLight = new THREE.PointLight(0xffffff, 1);
      pointLight.color = new THREE.Color("grey");
      this.camera.add(pointLight);
    },

    createGlowEffect() {
      const spriteMaterial = new THREE.SpriteMaterial({
        map: this.textureLoader.load("/earthModel/glow.png"),
        transparent: true,
        blending: THREE.AdditiveBlending,
      });

      // Create the sprite and scale it to size
      const glowSprite = new THREE.Sprite(spriteMaterial);
      glowSprite.scale.set(110, 110, 100); // Scale it to just over twice the size of the Earth sphere
      glowSprite.alpha = 0;

      return glowSprite;
    },

    calcPosFromLatLonRad(lat, lon, radius) {
      var phi = (90 - lat) * (Math.PI / 180);
      var theta = (lon + 180) * (Math.PI / 180);

      let x = -(radius * Math.sin(phi) * Math.cos(theta));
      let z = radius * Math.sin(phi) * Math.sin(theta);
      let y = radius * Math.cos(phi);

      return [x, y, z];
    },


    updatePointPosition(satellite) {
      // Calculate the new position
      if (satellite.stellaPoints.length >= 20) {
        satellite.stellaPoints.shift();
      }

      const tle = satellite.tle;
      const latLonObj = getLatLngObj(tle, this.actualTime);
      const newPosition = this.calcPosFromLatLonRad(
        latLonObj.lat,
        latLonObj.lng,
        satellite.altitude
      );

      satellite.x = newPosition[0];
      satellite.y = newPosition[1];
      satellite.z = newPosition[2];

      const newPoint = new THREE.Vector3(satellite.x, satellite.y, satellite.y);
      satellite.stellaPoints.push(newPoint);

      // Update the pointMesh's position
      // Set the position of the point
      satellite.pointMesh.position.set(
        newPosition[0],
        newPosition[1],
        newPosition[2]
      );
    },

    updateSatellites() {
      this.actualTime += 1000;
      this.satelliteData.forEach((satellite) => {
        this.scene.remove(satellite.stella)
        this.addStellaPointToEarth(satellite)
        this.scene.add(satellite.stella)
        this.updatePointPosition(satellite)
      });
    },

    addStellaPointToEarth(satellite) {
      // Assuming you have an array of THREE.Vector3 objects
      let points = [];
      for (let i = 0; i < 21; i++) {
        const timestamp = this.actualTime - 2000000 + 100000 * i;
        const latLon = getLatLngObj(satellite.tle, timestamp);
        const point = this.calcPosFromLatLonRad(
          latLon.lat,
          latLon.lng,
          satellite.altitude
        );

        const vector = new THREE.Vector3(point[0], point[1], point[2]);
        points.push(vector);
      }

      // Create a BufferGeometry

      let geometry = new THREE.BufferGeometry().setFromPoints(points);

      const colors = [];
      for (let i = 0; i < points.length; i++) {
        const color = new THREE.Color();
        color.setHSL(i / points.length, 1.0, 0.5); // Example: changing hue to create a gradient
        colors.push(color.r, color.g, color.b);
      }

      // Add the color information to the geometry
      geometry.setAttribute(
        "color",
        new THREE.Float32BufferAttribute(colors, 3)
      );

      // Create a material that uses vertex colors
      let material = new THREE.LineBasicMaterial({
        vertexColors: true, // This is important
        transparent: true,
        opacity: 0.6, // Adjust transparency as needed
      });

      let line = new THREE.Line(geometry, material);
      satellite.stella = line
    },

    addPointToEarth(satellite) {
      const tle = satellite.tle;
      const pointGeometry = new THREE.SphereGeometry(satellite.radius, 32, 32); // Small sphere geometry for the point
      const pointMaterial = new THREE.MeshBasicMaterial({ color: 0xff0000 }); // Red color for the point

      // Use the function to calculate the position
      const latLonObj = getLatLngObj(tle);
      const position = this.calcPosFromLatLonRad(
        latLonObj.lat,
        latLonObj.lng,
        satellite.altitude
      );

      satellite.x = position[0];
      satellite.y = position[1];
      satellite.z = position[2];

      satellite.pointMesh = new THREE.Mesh(pointGeometry, pointMaterial);

      // Set the position of the point
      satellite.pointMesh.position.set(position[0], position[1], position[2]);

      this.addStellaPointToEarth(satellite);

      // Add the point mesh to the earth mesh
      this.earth.mesh.add(satellite.pointMesh);

      this.scene.add(satellite.stella);
    },

    animationLoop() {
      const animate = () => {
        requestAnimationFrame(animate);

        this.earth.mesh.rotation.y += 0.0;
        this.controls.update();
        this.updateSatellites();
        this.renderer.render(this.scene, this.camera);
      };

      animate();
      this.renderer.render(this.scene, this.camera);
    },
  },
};
</script>

<style scoped>
.apps {
  width: 100%;
  height: 100%;
}

.app {
  width: 100%;
  height: 100%;
}
.worldMap {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  border-radius: 10rem;
  border: 1px solid #ccc;
}

canvas {
  width: 100%;
  height: 100%;
}

.satellite-marker {
  position: absolute;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background-color: red;
}
</style>
