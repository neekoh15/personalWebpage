<template>
  <h2>Get to know some of my work</h2>

  <div class="apps">
    <div class="app" name="ISSTracker">
      <div>
        <h3>Satellite tracker</h3>
        I always love all the things related to space and rocket science, so i
        made my own model of our planet to track the ISS and some of the
        Starlink's satellites
      </div>

      <div class="centered">
        <div class="worldMap" ref="mapElement" id="container">
          <div class="options">
            <div class="list">
              <button class="button" @click="changeWidth">EXPAND</button>
              <button class="button" @click="resetRotation">RESET</button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="app" name="heatmap">
      <div>
        <h3>Tools for UX Research</h3>
        Being part of an UX Research has teached me a lot of things, like the
        importance of the user behaviour and how this impact in the succes of an
        app or project
      </div>
      <div class="centered">
        <div class="webPage" ref="webPage" id="webPage-container"></div>
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
    this.ISS = this.satelliteData = [
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
        color: 0x0000ff,
      },

      {
        name: "STARLINK-1007",
        tle: `STARLINK-1007           
        1 44713U 19074A   24013.51526472  .00000131  00000+0  27687-4 0  9990
        2 44713  53.0539 115.8930 0001279  89.2081 270.9055 15.06394028230258`,
        altitude: 102,
        radius: 1,
        x: 0,
        y: 0,
        z: 0,
        pointMesh: null,
        color: 0xff00ff,
      },

      {
        name: "STARLINK-1008 ",
        tle: `STARLINK-1008           
        1 44714U 19074B   24013.49409644  .00002022  00000+0  15459-3 0  9993
        2 44714  53.0551 115.9909 0001682 117.8774 242.2386 15.06396681230251`,
        altitude: 102,
        radius: 1,
        x: 0,
        y: 0,
        z: 0,
        pointMesh: null,
        color: 0xff00ff,
      },

      {
        name: "STARLINK-1009 ",
        tle: `STARLINK-1009           
        1 44715U 19074C   24013.48303090 -.00000550  00000+0 -18047-4 0  9994
        2 44715  53.0548 116.0354 0001461 127.7024 232.4098 15.06394141230246`,
        altitude: 102,
        radius: 1,
        x: 0,
        y: 0,
        z: 0,
        pointMesh: null,
        color: 0xff00ff,
      },

      {
        name: "STARLINK-1010",
        tle: `STARLINK-1010           
        1 44716U 19074D   24013.56780997 -.00001802  00000+0 -10212-3 0  9995
        2 44716  53.0549 115.6542 0001489 104.1608 255.9547 15.06391102230242`,
        altitude: 102,
        radius: 1,
        x: 0,
        y: 0,
        z: 0,
        pointMesh: null,
        color: 0xff00ff,
      },

      {
        name: "STARLINK-1011",
        tle: `STARLINK-1011           
        1 44717U 19074E   24013.56596920 -.00000388  00000+0 -71528-5 0  9994
        2 44717  53.0536 135.6658 0001276  91.5262 268.5873 15.06395673229986`,
        altitude: 102,
        radius: 1,
        x: 0,
        y: 0,
        z: 0,
        pointMesh: null,
        color: 0xff00ff,
      },

      {
        name: "STARLINK-1012",
        tle: `STARLINK-1012           
        1 44718U 19074F   24013.53833585  .00000362  00000+0  43219-4 0  9998
        2 44718  53.0546 115.7903 0001452  94.0724 266.0431 15.06388159230253`,
        altitude: 102,
        radius: 1,
        x: 0,
        y: 0,
        z: 0,
        pointMesh: null,
        color: 0xff00ff,
      },

      {
        name: "STARLINK-1013 ",
        tle: `STARLINK-1013           
        1 44719U 19074G   24013.54569644 -.00002522  00000+0 -15047-3 0  9991
        2 44719  53.0530 115.7556 0001135  94.8107 265.3011 15.06396020231389`,
        altitude: 102,
        radius: 1,
        x: 0,
        y: 0,
        z: 0,
        pointMesh: null,
        color: 0x0000ff,
      },

      {
        name: "STARLINK-1014",
        tle: `STARLINK-1014           
        1 44720U 19074H   24013.56318007 -.00001365  00000+0 -72796-4 0  9999
        2 44720  53.0541 115.6670 0000725  89.0651 271.0421 15.06391041230266`,
        altitude: 102,
        radius: 1,
        x: 0,
        y: 0,
        z: 0,
        pointMesh: null,
        color: 0xff00ff,
      },

      {
        name: "STARLINK-1015",
        tle: `STARLINK-1015           
        1 44721U 19074J   24013.55306288  .00002373  00000+0  17818-3 0  9998
        2 44721  53.0542 115.7236 0001566 133.4463 226.6656 15.06397036230251`,
        altitude: 102,
        radius: 1,
        x: 0,
        y: 0,
        z: 0,
        pointMesh: null,
        color: 0xff00ff,
      },

      {
        name: "STARLINK-1017",
        tle: `STARLINK-1017           
        1 44723U 19074L   24013.50514859 -.00001213  00000+0 -62556-4 0  9994
        2 44723  53.0550 115.9421 0001455 107.8711 252.2437 15.06390005230259`,
        altitude: 102,
        radius: 1,
        x: 0,
        y: 0,
        z: 0,
        pointMesh: null,
        color: 0xff00ff,
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
    this.create3DEarth();
  },

  methods: {
    create3DEarth() {
      this.setupEarth();
      this.setupControls();
      this.createEarthScene();
      this.animationLoop();
    },

    createWebPageScene() {
      const container = document.querySelector("#webPage-container");
      this.WebPageRenderer = new THREE.WebGLRenderer({ alpha: true });
      this.WebGLRenderer.setSize(window.innerWidth, window.innerHeight);
      container.appendChild(this.WebPageRenderer.domElement)
      this.WebPageScene = new THREE.Scene();
      this.webPageCamera = new THREE.PerspectiveCamera(
        75,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
      );

      const geometry = new THREE.RingGeometry(1, 5, 32,);
      geometry.thetaSegments = 4
      const material = new THREE.MeshBasicMaterial({
        color: 0xffff00,
        side: THREE.DoubleSide,
      });
      const mesh = new THREE.Mesh(geometry, material);
      this.WebPageScene.add(mesh);

      console.log(container);
    },

    createEarthScene() {
      // Scene setup
      const container = document.getElementById("container");
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      this.renderer.domElement.style.width = "100%";
      this.renderer.domElement.style.height = "100%";
      //this.renderer.domElement.style.transition = 'width 1s, height 1s'
      //this.renderer.domElement.style.backgroundColor = 'blue'
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
      this.camera.position.z = this.earth.radius * 2;
    },

    setupEarth() {
      this.earth.geometry = new THREE.SphereGeometry(this.earth.radius, 32, 32);
      this.earth.material = new THREE.MeshLambertMaterial();
      this.earth.mesh = new THREE.Mesh(
        this.earth.geometry,
        this.earth.material
      );

      const texture = this.textureLoader.load("/earthModel/earth_daymap.jpg");
      /*const normalMap = this.textureLoader.load(
        "/earthModel/2k_earth_specular_map.tif"
      );*/
      this.earth.material.map = texture;
      //this.earth.material.normalMap = normalMap;
      //this.earth.material.transparent = true;

      // GLOW EFFECT ----
      const glowEffect = this.createGlowEffect();
      this.earth.mesh.add(glowEffect);
    },

    setupControls() {
      // Set up OrbitControls
      this.controls.enableDamping = true; // Optional, but this gives a nice inertia to the rotation
      this.controls.dampingFactor = 0.03;
      this.controls.rotateSpeed = 0.5; // Adjust the speed of rotation
      this.controls.enableZoom = false;
      this.controls.autoRotate = false;
      this.controls.enablePan = false;
    },

    createAmbientLight() {
      const ambientLight = new THREE.AmbientLight(0xffffff, 5);
      //scene.add(ambientLight);
      this.scene.add(ambientLight);
      const pointLight = new THREE.PointLight(0xffffff, 1);
      pointLight.color = new THREE.Color("grey");
      this.camera.add(pointLight);
    },

    createGlowEffect() {
      const spriteMaterial = new THREE.SpriteMaterial({
        map: this.textureLoader.load("/earthModel/earthGlow.png"),
        transparent: true,
        blending: THREE.AdditiveBlending,
      });

      // Create the sprite and scale it to size
      const glowSprite = new THREE.Sprite(spriteMaterial);
      glowSprite.scale.set(205, 1); // Scale it to just over twice the size of the Earth sphere
      glowSprite.alpha = 1;

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

      // Update the pointMesh's position
      // Set the position of the point
      satellite.pointMesh.position.set(
        newPosition[0],
        newPosition[1],
        newPosition[2]
      );
    },

    updateSatellites() {
      this.satelliteData.forEach((satellite) => {
        this.updatePointPosition(satellite);
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
        opacity: 0.2, // Adjust transparency as needed
      });

      let line = new THREE.Line(geometry, material);
      satellite.stella = line;
    },

    addPointToEarth(satellite) {
      const tle = satellite.tle;
      const pointGeometry = new THREE.SphereGeometry(satellite.radius, 32, 32); // Small sphere geometry for the point
      const pointMaterial = new THREE.MeshBasicMaterial({
        color: satellite.color,
      }); // Red color for the point

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

      //this.addStellaPointToEarth(satellite);

      // Add the point mesh to the earth mesh
      this.earth.mesh.add(satellite.pointMesh);
    },

    animationLoop() {
      const animate = () => {
        requestAnimationFrame(animate);

        this.earth.mesh.rotation.y += 0.0003;
        this.controls.update();
        this.updateSatellites();

        this.renderer.render(this.scene, this.camera);
      };

      animate();
      this.renderer.render(this.scene, this.camera);
    },

    changeCameraZoom() {
      this.camera.z += 1;
    },

    changeWidth() {
      const container = document.querySelector("#container");
      const appContainer = document.querySelector(".app");

      if (container.style.width == "100%") {
        container.style.width = "30vw";
        container.style.height = "30vh";
      } else {
        container.style.width = "100%";
        container.style.height = "100%";
      }

      window.scrollTo({
        top: appContainer.getBoundingClientRect().top + window.scrollY,
        behavior: "smooth",
      });
    },

    resetRotation() {
      this.camera.position.y = 0;
      this.camera.position.x = 0;
      this.camera.position.z = this.earth.radius * 2;
    },
  },
};
</script>

<style scoped>
.apps {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  gap: 2rem;
  padding-top: 2rem;
  z-index: 2;
}

.button {
  cursor: pointer;
  color: #98e8cd;
  background-color: #2c3e50;
  border: none;
  border-radius: 4px;
  height: 3rem;
  width: 5rem;
  z-index: 3;
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
  width: 40vw;
  height: 40vh;
  /*border: 1px solid #ccc;*/
  transition: width 1s, height 1s;
}

.centered {
  width: 100%;
  display: flex;
  justify-content: center;
  margin-top: 2rem;
}

.options {
  position: absolute;
  left: 100%;
  top: 0;
  display: flex;
  justify-content: right;
}

.list {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

canvas {
  width: 100%;
  height: 100%;
}

#ISSTracker {
  transition: left 1s;
}
</style>
