<template>
    <div>
      <div class="container">
        <div class="row" id="marvel-row">
          <div v-for="hero in heroes" :key="hero.id" class="col-md-4">
            <a @click="openModal(hero)">
              <img :src="hero.thumbnail" :alt="hero.name" class="img-thumbnail">
            </a>
            <h3 class="title" style="text-align: center; padding-right: 1.75cm; font-weight: bold; color:white ;">{{ hero.name }}</h3>
          </div>
        </div>
      </div>
  
      <div class="modal fade" id="exampleModalLong" tabindex="-1" role="dialog" aria-labelledby="exampleModalLongTitle" aria-hidden="true">
        <div class="modal-dialog" role="document">
          <div class="modal-content">
            <div class="modal-header" style="text-align: center;">
              <h5 class="modal-title" id="exampleModalLongTitle" style="text-align: center;">Información adicional </h5>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close" @click="closeModal">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <div class="modal-body d-flex align-items-center justify-content-center">
              <p v-if="selectedSerie">
                <h5><strong>serie 1: </strong> {{ selectedSerie.series }}</h5>
              </p>
              <p v-if="selectedHero">
                <h3><strong>{{ selectedHero.name }}</strong></h3>
                <img :src="selectedHero.thumbnail" :alt="selectedHero.name" class="img-thumbnail">
                <h5><strong>descripción:</strong> {{ selectedHero.description }}</h5>
                <h5><strong>cantidad de comics:</strong> {{ selectedHero.totalcomics }}</h5>
                <h5><strong>cantidad de series:</strong> {{ selectedHero.totalseries }}</h5>
                <h5><strong>cantidad de stories:</strong> {{ selectedHero.totalstories }}</h5>
                <h5><strong>cantidad de eventos:</strong> {{ selectedHero.totalevents }}</h5>
                <h5><strong>cantidad de eventos:</strong> {{ selectedHero.series.name }}</h5>
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
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        heroes: [],
        selectedHero: null,
        selectedSerie: null
      };
    },
    methods: {
      openModal(hero) {
        this.selectedHero = hero;
        this.selectedSerie = hero.series; // Suponiendo que deseas mostrar la primera serie del héroe
        $('#exampleModalLong').modal('show');
      },
      closeModal() {
        this.selectedHero = null;
        this.selectedSerie = null;
        $('#exampleModalLong').modal('hide');
      },
    },
    mounted() {
      const urlAPI = 'https://gateway.marvel.com:443/v1/public/characters?ts=2&apikey=1af92334b461b82edba5135a94169b1f&hash=442bad013f3ffcf998c84ee9283b8434';
  
      axios.get(urlAPI)
        .then((response) => {
          this.heroes = response.data.data.results.map((hero) => ({
            id: hero.id,
            name: hero.name,
            description: hero.description,
            totalcomics: hero.comics.available,
            totalseries: hero.series.available,
            totalstories: hero.stories.available,
            totalevents: hero.events.available,
            thumbnail: `${hero.thumbnail.path}.${hero.thumbnail.extension}`,
            series: hero.series.items[0] // Guarda todas las series del héroe
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
  