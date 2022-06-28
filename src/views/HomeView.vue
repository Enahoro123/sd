<template>
  <div class="home">    
    <div class="background" v-if="data"
    :style="{'background-image': 'url('+ imageUrl + data.weather[0].main + ')'}"
    ></div>
      <div class="left-side">
          <h3>The Weather</h3> 


                <div 
                v-if="!error && data !== null && data !== undefined && data.cod === 200"
                class="every"
                >
                    <h1  class="temp">{{ data.main.temp }}°C</h1>
                      <div class="location">
                        <h1  class="name">{{ data.name }}</h1>
                        <small>
                            <p class="description">{{ data.weather[0].description }}</p>
                        </small>
                      </div>
                      <div class="weather">
                        <img class="icon" :src="'https://openweathermap.org/img/wn/' + data.weather[0].icon +'@2x.png'" alt="">
                      </div>
                </div>

                <div 
                v-if="!error && data !== null && data !== undefined && data.cod === 400"
                class="every"
                >
                    <h1  class="temp">{{ data.main.temp }}°C</h1>
                      <div class="location">
                        <h1  class="name">{{ data.name }}</h1>
                        <small>
                            <p class="description">{{ data.weather[0].description }}</p>
                        </small>
                      </div>
                      <div class="weather">
                        <img class="icon" :src="'https://openweathermap.org/img/wn/' + data.weather[0].icon +'@2x.png'" alt="">
                      </div>
                </div>
                <div v-if="error" class="error">
                  <h2>{{ error }}</h2>
                </div>
      </div>
        

        <aside>
            <form>
                <input type="text" v-model="city" class="search-bar" placeholder="Enter city name">
                <button @click.prevent="fetchWeather">
                    <span class="material-icons">
                        search
                    </span>       
                      </button>
            </form>
            <div v-if="!error && data !== null && data !== undefined &&  data.cod === 200">
              <ul class="details">
                <h4>Weather Details</h4>
                <li> <span>Feels Like </span> <span>{{ data.main.feels_like }}°</span></li>
                <li> <span>Humidity   </span> <span>{{ data.main.humidity }}%</span></li>
                <li> <span>Wind       </span> <span>{{ data.wind.speed }}km/h</span></li>
                <li> <span>Pressure   </span> <span>{{ data.main.pressure }}</span></li>
              </ul>
            </div>

            <div v-if="!error && data !== null && data !== undefined && !error && data.cod === 400">
              <ul class="details">
                <h4>Weather Details</h4>
                <li> <span>Feels Like </span> <span>{{ data.main.feels_like }}°</span></li>
                <li> <span>Humidity   </span> <span>{{ data.main.humidity }}%</span></li>
                <li> <span>Wind       </span> <span>{{ data.wind.speed }}km/h</span></li>
                <li> <span>Pressure   </span> <span>{{ data.main.pressure }}</span></li>
              </ul>
            </div>
        </aside>

  </div>
</template>

<script>
import { ref } from '@vue/reactivity'

export default {
  name: 'HomeView',
  setup() {
    const weathers = ref([]);
    const city = ref('')
    const data = ref(null)
    const error = ref(false)
    const imageUrl = ref(`https://source.unsplash.com/1600x900/?`)
    

    const fetchWeather = async ()  => {
      try {
        const res = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${city.value}&units=metric&appid=66822e77dd146ca9b9cd4f0b603da3bc`); 
        if(!res.ok) {
          throw Error("Sorry, city not found")
          // Promise.reject(error.value);
        }
        data.value = await res.json();
        error.value = false
        console.log(await res) 
      } catch (e) {
        error.value = true
        error.value = e.message;
        console.log(error.value)  
      }

    }


      return { error, city, weathers, fetchWeather, data, imageUrl};

  }
}
</script>

<style>

h1, h3 {
  font-weight: 400;
}
.home, .background {
  width: 100%;
  min-height: 100vh;
  transition: 500ms;
  position: absolute;
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
  background-image: url('https://source.unsplash.com/1600x900/?weather');
}
.background::before {
  top: 0;
  left: 0;
  content: "";
  width: 100%;
  height: 100%;
  position: absolute;
  background: rgba(0, 0, 0, 0.3);
  z-index: 0;
}

  .left-side {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: flex;
    color: #fff;
    justify-content: space-between;
    align-items: flex-start;
    flex-direction: column;
    padding: 2em 3em 4em 3em;
  }
  .left-side > div {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .temp, .location, .weather {
    margin: 0 1em;
  } 

  .loaction h1 {
    margin: 0;
    font-size: 3em;
    margin-bottom: 0.2em;
  }
  .description {
    font-size: 15px;
  }

  .temp {
    font-size: 5em;
    margin: 0;
  }

  .weather img {
    display: block;
    margin: 0.5em 0;
  }

  aside{
    width: 40%;
    padding: 45px 60px 60px 60px;
    position: absolute;
    right: 0;
    top: 0;
    min-height: 100vh;
    background:rgba(110, 110, 110, 0.25);
    box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.3);
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.18);
    z-index: 1;
    overflow-Y: scroll;
  }

  aside form {
    padding-bottom: 3em;
  }

  aside::-webkit-scrollbar {
    display:none;
  }

  
  /* buttons */
 button{ 
    position: absolute;
    top: 0;
    right: 0;
    padding: 1.3em;
    margin: 0;
    border: none;
    outline: none;
    background: #fa6d1b;
    color: #fff;
    cursor: pointer;
    font-size: 1.2em;
    transition: 0.4s;
  }
  button:hover{
    background: #fff;
    color: #000;
  }
  
  /* forms */
  form {
    color: #ccc;
    /* margin: 0 auto; */
  }
  .search-bar{
    border: none;
    width: 80%;
    color: #fff;
    border: none;
    padding: 0 1em 0.5em 0;
    outline: none;
    background: none;
    font-size: 1.1em;
    border-bottom: 1px solid #ccc;
  }  

  .search-bar:focus {
    outline: none;
  }

  .search-bar::placeholder {
    color: #ccc;
  }
  
  aside ul {
    padding: 0 0 1em 0;
    margin: 2em 0;
    border-bottom: 1px #ccc solid;
  }

  aside ul li {
    color: #ccc;
    margin: 2.5em 0;
  }

  aside ul h4 {
    margin: 3em 0;
  }

  .city {
    display: block;
    cursor: pointer;
  }

  .city:hover {
    color: #fff;
  }

  .details li {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .details > h4 {
    text-align: left;
    color: #fff;
  }

  /* .location {
    margin:  auto 10px;
    padding-right: 20px;
    padding-left: 20px;
  } */

  /* .name {
    text-align: left;
    font-weight: 500;
    font-size: 50px;
  } */

@media only screen and (max-width: 975px) {
  .left-side {
    font-size: 13px;
  }
}
@media only screen and (max-width: 800px) {
  aside, .left-side {
    position: relative;
    width: 100%;
    top: initial;
  }
  .error {
    padding-top: 20px;
    font-size: 2em;
  }
}
@media only screen and (max-width: 600px) {
  .home {
    font-size: 12px;
  }
  .every {
    font-size: 10px;
  }
}
@media only screen and (max-width: 620px) {
  .left-side > div {
    width: 100%;
    display: inline;
    text-align: center;
  }
  .weather {
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .temp, .error {
    padding-top: 20px;
  }
}
@media only screen and (max-width: 300px) {
  .home .background {
    min-height: 40em;
  }
}

</style>
