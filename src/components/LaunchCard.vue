<template>
  <div class="card">
    <img :src="launch.links.mission_patch_small" class="card-image" alt="my-img" />
    <p class="launch-name">{{launch.mission_name}} #{{launch.flight_number}}</p>
    <span class="mission-detail">
      Mission Ids:
      <ul>
        <li class="mission-id-value">{{launch.mission_id[0] | missionIdCheck}}</li>
      </ul>
    </span>
    <div class="info-grid">
      <div class>Launch Year</div>
      <div>{{launch.launch_year}}</div>
      <div>Successful Launch</div>
      <div>{{launch.launch_success}}</div>
      <div>Successful Landing</div>
      <div>{{launch.rocket.first_stage.cores[0].land_success | landSuccessCheck}}</div>
    </div>
  </div>
</template>
<script>
export default {
  props: ["launch"],

  mounted() {},

  filters: {
    missionIdCheck(value) {
      if (typeof value === "undefined") {
        return "Not Provided";
      } else {
        return value;
      }
    },
    landSuccessCheck(value) {
      if (value === null) {
        return "No Info";
      } else {
        return value;
      }
    }
  }
};
//CODE BY AVNEESH KOHLI
</script>
<style>
ul {
  margin: 0;
}
.card {
  border-radius: 5px;
  box-shadow: 0 0.1rem 1rem rgba(0, 0, 0, 0.22);
  padding: 1em;
  background: #fff;
  width: 100%;
  display: table;
  transition: transform 0.25s;
  margin: 0 0 1.5em;
  min-height: 22em;
}
.card-image {
  border-radius: 3px;
  width: 100%;
  height: 50%;
  background-color: #eeeeee;
}
.launch-name {
  color: #223f7a;
  font-size: 1.2em;
  font-weight: 600;
  margin: 0.8rem 0px;
}
.mission-detail {
  display: block;
  color: #000;
  font-size: 1.2em;
  margin: 0 0 0.6rem 0;
  font-weight: 600;
}
.mission-id-value {
  font-weight: normal;
  color: #223f7a;
  font-size: 0.95em;
}
.info-grid {
  display: grid;
  grid-template-rows: 1.6rem 2rem 2rem;
  grid-template-columns: 50% 50%;
  grid-gap: 15px 5px;
  color: #223f7a;
  font-size: 1.1em;
}
.info-grid div {
  display: flex;
  flex-direction: column;
  justify-content: center;
}
.info-grid div:nth-child(odd) {
  color: #000;
  font-weight: 600;
}
@media (min-width: 700px) {
  .card {
    padding: 2;
    width: 41.5%;
    margin: 0 1% 3.5% 1%;
  }
  .launch-name {
    font-size: 1em;
    margin: 0.5rem 0px;
  }
  .mission-detail {
    font-size: 1em;
  }
  .info-grid {
    grid-gap: 10px 5px;
    font-size: 0.9em;
  }
}
@media (min-width: 1024px) {
  .card {
    padding: 1% 1% 2%;
    width: 18.5%;
    transition: transform 0.25s;
    margin: 0 1% 3.5% 1%;
  }
  .card:hover {
    transform: scale(1.05);
  }
}
</style>