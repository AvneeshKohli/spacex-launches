<template>
  <div id="app">
    <h1>SpaceX Launch Programs</h1>
    <div class="filter-section">
      <h2>Filters</h2>
      <p>Launch Year</p>
      <div class="annual-btns-grid">
        <button
          type="button"
          v-for="(year,index) in years"
          v-bind:key="index"
          class="btn"
          v-on:click="updateView($event,year)"
        >{{year}}</button>
      </div>
      <p>Successful Launch</p>
      <div class="annual-btns-grid">
        <button type="button" class="btn" v-on:click="updateView($event,'Launch success')">True</button>
        <button type="button" class="btn" v-on:click="updateView($event,'Launch fail')">False</button>
      </div>
      <p>Successful Landing</p>
      <div class="annual-btns-grid">
        <button type="button" class="btn" v-on:click="updateView($event,'Land success')">True</button>
        <button type="button" class="btn" v-on:click="updateView($event,'Land fail')">False</button>
      </div>
    </div>
    <div v-if="isLodaing" class="loader">
      <div class="loading-spinner"></div>
    </div>
    <div v-if="showCardGrid" class="launch-card-wrapper">
      <launch-card v-for="launch in launches" :launch="launch" v-bind:key="launch.flight_number" />
    </div>
  </div>
</template>

<script>
import LaunchCard from "./components/LaunchCard.vue";
import axios from "axios";
export default {
  name: "App",
  components: {
    LaunchCard
  },

  data: () => ({
    launches: [],
    showCardGrid: true,
    years: [],
    isLodaing: true,
    basePath: "https://api.spacexdata.com/v3/launches?limit=100",
    currentPath: ""
  }),
  async created() {
    for (let i = 2006; i <= 2020; i++) {
      this.years.push(i);
    }
    const { data } = await axios.get(
      "https://api.spacexdata.com/v3/launches?limit=100"
    );
    data.forEach(launch => {
      this.launches.push(launch);
    });
    this.$router.push('/');
    this.isLodaing = false;
  },

  methods: {
    updateView(event, val) {
      if (isNaN(val) === false) {
        let el = document.querySelector(".active-btn");
        if (el !== null) {
          el.classList.remove("active-btn");
        }
        let launchYear = "&launch_year=" + val;
        event.target.classList.add("active-btn");
        if (this.basePath.includes("launch_year")) {
          let existingYear = this.basePath.substring(
            this.basePath.indexOf("&launch_year"),
            this.basePath.indexOf("&launch_year") + 17
          );

          this.basePath = this.basePath.replace(existingYear, launchYear);
        } else {
          this.basePath = this.basePath + launchYear;
        }
      } else if (val.includes("Launch")) {
        let el = document.querySelector(".active-launch-btn");
        if (el !== null) {
          el.classList.remove("active-launch-btn");
        }
        event.target.classList.add("active-launch-btn");
        let new_launch = val.includes("success")
          ? "&launch_success=true"
          : "&launch_success=false";
        if (this.basePath.includes("&launch_success=true")) {
          this.basePath = this.basePath.replace(
            "&launch_success=true",
            new_launch
          );
        } else if (this.basePath.includes("&launch_success=false")) {
          this.basePath = this.basePath.replace(
            "&launch_success=false",
            new_launch
          );
        } else {
          this.basePath = this.basePath + new_launch;
        }
      } else {
        //landing block
        let el = document.querySelector(".active-land-btn");
        if (el !== null) {
          el.classList.remove("active-land-btn");
        }
        event.target.classList.add("active-land-btn");
        let new_land = val.includes("success")
          ? "&land_success=true"
          : "&land_success=false";
        if (this.basePath.includes("&land_success=true")) {
          this.basePath = this.basePath.replace("&land_success=true", new_land);
        } else if (this.basePath.includes("&land_success=false")) {
          this.basePath = this.basePath.replace(
            "&land_success=false",
            new_land
          );
        } else {
          this.basePath = this.basePath + new_land;
        }
      }
      console.log(this.basePath);
      this.showCardGrid = false;
      axios.get(this.basePath).then(response => {
        this.launches = [];
        this.launches = response.data;
      });
      let url = this.basePath.substring(
        this.basePath.indexOf("limit") + 10,
        this.basePath.length
      );
      this.$router.push({ path: url, component: this.$el });
      this.showCardGrid = true;
    }
  }
};
//CODE BY AVNEESH KOHLI
</script>

<style>
html {
  margin: 0;
  padding: 0;
}
body {
  background-color: #eeeeee;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  font-size: 100%;
}
h2 {
  margin-top: 0;
}
#app {
  max-width: 1440px;
  margin: 0 auto;
  position: relative;
}
.loader {
  background: #eeeeee;
  opacity: 0.65;
  width: 100%;
  position: absolute;
  top: 0;
  z-index: 1;
  height: 100vh;
}
.filter-section {
  background-color: white;
  display: block;
  z-index: 1;
  padding: 1em;
  border-radius: 8px;
  margin: 0 0.8em 1em;
}
.filter-section p {
  width: 60%;
  text-align: center;
  font-size: 20px;
  margin: 1rem auto;
  border-bottom: 1px solid #bbb;
  padding-bottom: 4px;
}
.annual-btns-grid {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  margin: 0 1em;
}
.btn {
  background-color: #a7ce95;
  border-radius: 6px;
  border: none;
  display: inline-block;
  cursor: pointer;
  color: #000000;
  font-family: "Courier New", Courier, monospace;
  font-size: 14px;
  text-decoration: none;
  width: 41%;
  margin-bottom: 0.9rem;
  height: 2.2rem;
}
.btn.active-btn {
  background-color: #6a9c50;
}

.btn:active {
  background-color: #6a9c50;
  position: relative;
  border: none;
  top: 1px;
}
.btn:focus {
  outline: none;
}
.launch-card-wrapper {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-evenly;
  margin: 0.8em;
}
@media (min-width: 700px) {
  .launch-card-wrapper {
    margin: 0 0 0 28%;
    position: absolute;
    top: 3.7rem;
    min-width: -webkit-fill-available;
  }
  .btn {
    width: 46%;
    margin-bottom: 0.6rem;
    height: 1.8rem;
  }
  .filter-section p {
    width: 74%;
    font-size: 16px;
  }
  .btn:hover,
  .btn.active-btn,
  .btn.active-launch-btn,
  .btn.active-land-btn {
    background-color: #6a9c50;
  }

  .filter-section {
    width: 23%;
    margin: 0;
  }
}
@media (min-width: 1024px) {
  .launch-card-wrapper {
    margin-left: 17%;
  }
  .btn {
    width: 46%;
    margin-bottom: 0.6rem;
    height: 1.8rem;
  }
  .filter-section p {
    width: 74%;
    font-size: 16px;
  }
  .filter-section {
    width: 14%;
  }
}
.loading-spinner {
  width: 100px;
  height: 100px;
  border-top: 3px solid #000;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  position: absolute;
  left: 45%;
  transform: translate(-100%, -50%);
  top: 45%;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
