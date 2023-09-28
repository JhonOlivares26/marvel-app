<template>
    <div class="container">
      <div class="row" id="marvel-row">
        <div v-for="hero in heroes" :key="hero.id" class="col-md-4">
          <a @click="openModal(hero)">
          <img :src="hero.thumbnail" :alt="hero.name" class="img-thumbnail">
        </a>
          <h3 class="title">{{ hero.name }}</h3>
        </div>
      </div>
    </div>

    <div class="modal fade" id="exampleModalLong" tabindex="-1" role="dialog" aria-labelledby="exampleModalLongTitle" aria-hidden="true">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLongTitle">Información adicional</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body d-flex align-items-center justify-content-center">
            <p v-if="selectedHero">
              <h3><strong>{{ selectedHero.name }}</strong></h3>
              <img :src="selectedHero.thumbnail" :alt="selectedHero.name" class="img-thumbnail">
              <h5>cantidad de comics: {{ selectedHero.totalcomics }}</h5>   
              <h5>cantidad de series: {{ selectedHero.totalseries }}</h5> 
              <h5>cantidad de stories: {{ selectedHero.totalstories }}</h5> 
              <h5>cantidad de eventos: {{ selectedHero.totalevents }}</h5>            
              
            </p>
            <p v-else>
              No se ha seleccionado un héroe.
            </p>
          </div>
          <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-dismiss="modal" @click="closeModal">Cerrar</button>
        </div>
        </div>
      </div>
    </div>
</template>
  
<script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        heroes: [],
        selectedHero: null
      };
    },
    methods: {
    openModal(hero) {
      this.selectedHero = hero;
      $('#exampleModalLong').modal('show');
    },
    closeModal() {
      this.selectedHero = null;
      $('#exampleModalLong').modal('hide');
    },
    },
    mounted() {
      const urlAPI = 'https://gateway.marvel.com:443/v1/public/characters?ts=2&apikey=1af92334b461b82edba5135a94169b1f&hash=442bad013f3ffcf998c84ee9283b8434';
  
      axios.get(urlAPI)
        .then((response) => {
          console.log(response.data.data.results)
          this.heroes = response.data.data.results.map((hero) => ({
            name: hero.name,
            totalcomics: hero.comics.available,
            totalseries: hero.series.available,
            totalstories: hero.stories.available,
            totalevents: hero.events.available,
            thumbnail: `${hero.thumbnail.path}.${hero.thumbnail.extension}`,
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

#exampleModalLong{
  text-align: center;
}
</style>
  
  
