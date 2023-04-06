<template>
  <div class="container">
    <header>
      <div class="sun"></div>
      <div class="cloud"></div>
    </header>
    <main>
      <div class="wrap">
        <select v-model="locationName">
          <option value="" selected disabled>請選擇地區</option>
          <option
            v-for="location of locations"
            :key="location.locationName"
            :value="location.locationName"
          >
            {{ location.locationName }}
          </option>
        </select>
        <template v-for="location of locations" :key="location.locationName">
          <div
            class="weather-container"
            v-show="locationName === location.locationName"
          >
            <div class="title">
              <h2 class="location-name">{{ locationName }}</h2>
              <div class="weather">
                {{ location.weatherElement[0].time[0].parameter.parameterName }}
              </div>
            </div>
            <div class="description">
              <div class="max-temperature">
                <h3>最高溫</h3>
                <p>
                  {{
                    location.weatherElement[4].time[0].parameter.parameterName
                  }}&deg;C
                </p>
              </div>
              <div class="min-temperature">
                <h3>最低溫</h3>
                <p>
                  {{
                    location.weatherElement[2].time[0].parameter.parameterName
                  }}&deg;C
                </p>
              </div>
              <div class="rainfall">
                <h3>降雨機率</h3>
                <p>
                  {{
                    location.weatherElement[1].time[0].parameter.parameterName
                  }}%
                </p>
              </div>
            </div>
          </div>
        </template>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
const { VITE_WEATHERS } = import.meta.env;

let locations = ref([])

let locationName = ref("")

function getWeather () {
  fetch(VITE_WEATHERS)
    .then((res) => res.json())
    .then((json) => {
      locations.value = json.records.location
    })
    .catch((error) => {
      alert("系統異常，請稍後再試");
    });
}

onMounted(() => {
  getWeather();
})

</script>

<style>
.container {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

header {
  width: 100%;
}

.sun {
  position: absolute;
  width: 90px;
  height: 90px;
  border-radius: 50%;
  top: 0;
  left: 0;
  transform: translate(-50%, -50%);
  z-index: -1;
  animation: shine 3s infinite linear;
  background-color: orange;
}

.cloud {
  position: absolute;
  width: 100px;
  height: 100px;
  filter: blur(6px);
  border-radius: 50%;
  top: 0;
  right: 0;
  z-index: -1;
  opacity: 0.7;
  background: white;
  animation: cloudmove 3s infinite linear;
}

.cloud::before {
  content: "";
  background: white;
  width: 90px;
  height: 90px;
  filter: blur(3px);
  position: absolute;
  border-radius: 50%;
  top: -25px;
  left: 60px;
}

.cloud::after {
  content: "";
  background: white;
  width: 180px;
  height: 80px;
  filter: blur(3px);
  position: absolute;
  border-radius: 50%;
  bottom: -5px;
  left: 40px;
}

main {
  width: 500px;
  background-color: var(--primary);
  border-radius: 10px;
  padding: 10px;
}

.wrap {
  border-radius: 10px;
  overflow: hidden;
}

select {
  display: block;
  background-color: var(--bg1);
  color: var(--bg2);
  width: 100%;
  padding: 10px;
  height: 50px;
  border: none;
  outline: none;
  cursor: pointer;
  font-size: 1.2rem;
  font-weight: bold;
}

.weather-container {
  background-color: var(--weather-container);
  width: 100%;
  padding: 10px 20px 20px;
  color: var(--bg1);
}

.title {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-bottom: 15px;
  border-bottom: 1px solid var(--bg1);
  margin-bottom: 15px;
}

.location-name {
  font-size: 1.5rem;
  letter-spacing: 0.15rem;
}

.weather {
  background-color: var(--bg1);
  color: var(--weather-container);
  padding: 5px;
  font-size: 1.2rem;
  font-weight: bold;
  border-radius: 5px;
}

.description {
  display: flex;
  justify-content: space-evenly;
  align-items: center;
}

.max-temperature,
.min-temperature,
.rainfall {
  text-align: center;
}

h3 {
  font-size: 1.5rem;
  margin-bottom: 10px;
}

p {
  font-size: 1.4rem;
}

@keyframes shine {
  0% {
    box-shadow: 0 0 10px orange, 0 0 60px orange, 0 0 200px yellow,
      inset 0 0 50px yellow;
  }

  50% {
    box-shadow: 0 0 10px orange, 0 0 10px orange, 0 0 30px yellow,
      inset 0 0 50px yellow;
  }

  100% {
    box-shadow: 0 0 10px orange, 0 0 60px orange, 0 0 200px yellow,
      inset 0 0 50px yellow;
  }
}

@keyframes cloudmove {
  0% {
    transform: translate(70%, -50%);
  }

  50% {
    transform: translate(10%, -50%);
  }

  100% {
    transform: translate(70%, -50%);
  }
}
</style>
