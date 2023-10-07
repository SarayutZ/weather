<template>
  <div class="app-container">
    <h1>สภาพอากาศ</h1>

    <div class="forecast-container">
      <div class="forecast-item" v-for="(item, index) in items" :key="index">
        <div class="forecast-time">{{ formatTime(item.dt_txt) }}</div>
        <div class="forecast-description">{{ item.weather[0].description }}</div>
        <div class="forecast-temp">{{ kelvinToCelsius(item.main.temp) }}°C</div>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { ref, onMounted } from "vue";

const items = ref([]);
const url = "https://api.openweathermap.org/data/2.5/forecast?lat=44.34&lon=10.99&appid=264f08ebe173d6c08b7e05d256f9b714";

function fetchWeatherData() {
  axios
    .get(url)
    .then((response) => {
      items.value = response.data.list;
    })
    .catch((err) => {
      console.error(err);
    });
}

onMounted(() => {
  fetchWeatherData();
});

function formatTime(dateTime) {
  const date = new Date(dateTime);
  const options = {
    year: 'numeric',
    month: 'long',
    day: 'numeric',
    hour: 'numeric',
    minute: 'numeric',
  };
  return date.toLocaleString('en-US', options);
}

function kelvinToCelsius(kelvin) {
  return (kelvin - 273.15).toFixed(2);
}
</script>

<style>

/* สไตล์ของหน้าแอปพลิเคชัน */
.app-container {
  background-color: #f0f0f0;
  padding: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  text-align: center;
}

/* สไตล์ของหัวข้อสภาพอากาศ */
h1 {
  font-size: 24px;
  color: #333;
  margin-bottom: 20px;
}

/* สไตล์ของคอนเทนเนอร์ของรายการสภาพอากาศ */
.forecast-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.forecast-item {
  background-color: #fff;
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 10px;
  margin: 10px;
  width: 200px;
  box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.2);
  transition: transform 0.2s;
}

.forecast-item:hover {
  transform: scale(1.05);
}

.forecast-time {
  font-weight: bold;
  color: #007bff;
}

.forecast-description {
  margin-top: 5px;
  color: #666;
}

.forecast-temp {
  margin-top: 5px;
  font-weight: bold;
  color: #333;
}

</style>