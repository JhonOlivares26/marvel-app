<template>
    <div class="container">
      <div class="row" id="marvel-row">
        <div v-for="hero in heroes" :key="hero.id" class="col-md-4">
          <a :href="hero.url" target="_blank">
            <img :src="hero.thumbnail" :alt="hero.name" class="img-thumbnail">
          </a>
          <h3 class="title">{{ hero.name }}</h3>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        heroes: []
      };
    },
    mounted() {
      const urlAPI = 'https://gateway.marvel.com:443/v1/public/characters?ts=2&apikey=1af92334b461b82edba5135a94169b1f&hash=442bad013f3ffcf998c84ee9283b8434';
  
      fetch(urlAPI)
        .then((res) => res.json())
        .then((json) => {
          this.heroes = json.data.results.map((hero) => ({
            name: hero.name,
            thumbnail: `${hero.thumbnail.path}.${hero.thumbnail.extension}`,
            url: hero.urls[0].url
          }));
        })
        .catch((error) => {
          console.error('Error al obtener datos de Marvel API:', error);
        });
    }
  };
  </script>
  
