<template>

  <div class="window" :class="{ 'fullscreen' : fullscreen }">
    <div class="title-bar">
      <div class="title-bar-text">VueFlickr Search</div>
      <div class="title-bar-controls">
        <button aria-label="Minimize"></button>
        <button aria-label="Maximize" @click="toggleFullscreen"></button>
        <button aria-label="Close"></button>
      </div>
    </div>
    <div class="window-body">

      <div class="window-body-header">
        <div class="field-row">
          <label for="search-term">Search Term</label>
          <input id="search-term" type="text" v-model="searchTerm" @keyup.enter="getResults(searchTerm)"/>
          <button @click="getResults(searchTerm)">Search</button>
        </div>
      </div>

      <div class="window-body-results">

        <div v-if="preResults" class="pre-results">
          <img src="../assets/image.png">
          <p>Enter search term...</p>
        </div>

        <div v-if="noResults" class="pre-results">
          <img src="../assets/question.png">
          <p>What does that even mean</p>
        </div>

        <div v-else-if="!hasResults" class="no-results">

        </div>

        <div v-else-if="hasResults" class="results">
          <img v-for="r in results.photos.photo" :key="r.id" :src="r.url_n" :alt="r.title">
        </div>

      </div>


    </div>
  </div>

</template>

<script>

  export default {

    name: 'SearchWindow',

    data() {
      return {
        searchTerm: '',
        results: {},
        preResults: true,
        noResults: false,
        fullscreen: false
      }
    },

    mounted() {

    },

    methods: {
      getResults(query) {
        let vm = this;

        if ( !query ) {
          vm.preResults = true;
          vm.noResults = false;
          vm.results = {};
          return
        }

        let params = '?api_key=6957f4a8f63023047ae094f762e2d351'
                     + '&method=flickr.photos.search'
                     + '&format=json'
                     + '&nojsoncallback=1'
                     + '&media=photos'
                     + '&extras=url_n'
                     + '&sort=relevance'
                     + '&per_page=40'
                     + '&text=' + query

        fetch('https://api.flickr.com/services/rest' + params)
        .then(response => response.text())
        .then(responseText => {

          let responseJSON = JSON.parse(responseText);

          vm.preResults = false;
          if ( responseJSON.photos.total === '0' ) {
            vm.noResults = true
          } else {
            vm.results = responseJSON;
          }

        })
      },

      toggleFullscreen() {
        this.fullscreen = !this.fullscreen;
      }
    },

    computed: {
      hasResults : function(){
        return Object.keys(this.results).length !== 0;
      }
    }
  }

</script>

<style scoped>

  .window {
    width: 50vw;
    max-width: 800px;
    margin-top: -50px;
  }

  .window.fullscreen {
    width: 100vw;
    max-width: none;
    height: calc(100vh - 30px);
    margin-top: -30px;
    display: flex;
    flex-direction: column;
  }

  .window.fullscreen .window-body {
    flex-grow: 1;
    display: flex;
    flex-direction: column;
  }

  .window.fullscreen .window-body .window-body-results {
    flex-grow: 1;
  }

  .window.fullscreen .window-body .window-body-results .results img {
    width: 20%;
    height: 320px;
  }

  .window-body {
    margin: 0 8px 8px
  }

  .window-body-header {
    margin: 20px 0;
  }

  .window-body-header .field-row label {
    margin-right: 10px;
    font-weight: bold;
  }

  .window-body-header .field-row input[type="text"] {
    flex-grow: 1;
  }

  .window-body-results {
    background: #fff;
    box-shadow: inset -1px -1px #fff, inset 1px 1px grey, inset -2px -2px #dfdfdf, inset 2px 2px #0a0a0a;
    height: 400px;
    padding: 6px;
    margin: 20px 0 10px;
    overflow-y: scroll;
  }

  .window-body-results .pre-results {
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
  }

  .window-body-results .results {
    display: flex;
    flex-wrap: wrap;
  }

  .window-body-results .results img {
    width: 25%;
    padding: 10px;
    height: 200px;
    object-fit: cover;
  }

  @media (min-width: 1400px) {
    .window-body-results .results img {
      height: 250px;
    }
  }


</style>
