<template>
    <div class="container">
      <div class="row" id="marvel-row">
        <div v-for="hero in heroes" :key="hero.id" class="col-md-4">
          <a :href="hero.url" target="_blank">
            <img :src="hero.thumbnail" :alt="hero.name" class="img-thumbnail">
          </a>
          <h3 class="title">{{ hero.name }}</h3>
          <h3 >{{ hero.total }}</h3>
        </div>
      </div>
    </div>
</template>
  
<script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        heroes: []
      };
    },
    mounted() {
      const urlAPI = 'https://gateway.marvel.com:443/v1/public/characters?ts=2&apikey=1af92334b461b82edba5135a94169b1f&hash=442bad013f3ffcf998c84ee9283b8434';
  
      axios.get(urlAPI)
        .then((response) => {
          this.heroes = response.data.data.results.map((hero) => ({
            name: hero.name,
            total: hero.comics.Count,
            thumbnail: `${hero.thumbnail.path}.${hero.thumbnail.extension}`,
            url: hero.name
          }));
        })
        .catch((error) => {
          console.error('Error al obtener datos de Marvel API:', error);
        });
    }
  };
  </script>

<style>
img.img-thumbnail{
  width: 80%;
  height: 80%;
}
</style>
  
  
