<template>
<div class="home">
<Hero/>
<!--search-->
    <div class="container search">
      <input @keyup.enter="$fetch" type="text" placeholder="Search" v-model.lazy="searchInput">
      <button  @click="clearSearch" class="button" v-show="searchInput !==''" >Clear Search</button>
    </div>
<!--loading-->
<Loading v-if="$fetchState.pending"/>
    <!--movies-->
<div v-else class="container movies">
  <!--searched movies-->
  <div  v-if="searchInput !== ''" id="movies-grid" class="movies-grid">
    <div class="movie" v-for="(movie,index) in searchedMovie" :key="index"> 
      <h1>{{searchInput}}</h1>
      <div class="movie-img">
        <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" alt=""/>
        <p class="review">{{movie.vote_average}}</p>
        <p class="overview">{{movie.overview}}</p>
      </div>
      <div class="info">
        <p class="title">{{movie.title.slice(0,25)}} <span v-if="movie.title.length > 25">...</span></p>
        <p class="release"> 
          Realeased:
          {{
              new Date(movie.release_date).toLocaleString('en-us',{
            month:'long',
            day:'numeric',
           year:'numeric',})
          }}
        </p>
        <NuxtLink class="button button-light" :to="{name:'movies-movieid',params:{movieid: movie.id} }">Get More Info</NuxtLink>
      </div>
    </div>
   </div>
   <div v-else id="movies-grid" class="movies-grid">
    <div class="movie" v-for="(movie,index) in movies" :key="index"> 
      <div class="movie-img">
        <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" alt=""/>
        <p class="review">{{movie.vote_average}}</p>
        <p class="overview">{{movie.overview}}</p>
      </div>
      <div class="info">
        <p class="title">{{movie.title.slice(0,25)}} <span v-if="movie.title.length > 25">...</span></p>
        <p class="release"> 
          Realeased:
          {{
              new Date(movie.release_date).toLocaleString('en-us',{
            month:'long',
            day:'numeric',
           year:'numeric',})
          }}
        </p>
        <NuxtLink class="button button-light" :to="{name:'movies-movieid',params:{movieid: movie.id} }">Get More Info</NuxtLink>
      </div>
    </div>
   </div>
  </div>
</div>
</template>

<script>
import axios from 'axios'
import Hero from '../components/Hero.vue'
import Loading from '../components/Loading.vue'

export default {
  components: { Hero, Loading },
   title: 'Movie App - Latest Streaming Movie Info',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Get all the latest streaming movies in theaters & online',
        },
        {
          hid: 'keywords',
          name: 'keywords',
          content: 'movies, stream, stremaing',
        },
      ],
  data(){
    return{
      movies:[],
      searchedMovie:[],
      searchInput:"",
    }
  },
  async fetch(){
    if(this.searchInput ===''){
       await this.getMovies()
    }
      this.searchedMovies()
  },
  fetchDelay:1000,
  methods:{
    async getMovies(){
      const data = axios.get(`https://api.themoviedb.org/3/movie/now_playing?api_key=668ab8274088334d5a8aec46794fb802&language=en-US&page=1`)
      const result = await data
      result.data.results.forEach(movie => {
        this.movies.push(movie)
      })
      },
     async searchedMovies(){
       this.searchedMovie = []
       const data = axios.get(`https://api.themoviedb.org/3/search/movie?api_key=668ab8274088334d5a8aec46794fb802&language=en-US&page=1&query=${this.searchInput}`)
       const result = await data
       result.data.results.forEach(movie => {
        this.searchedMovie.push(movie)
      })
      console.log(this.searchedMovie);
     },
     clearSearch(){
       this.searchInput = ''
       this.searchedMovie = []
     }
  }
}
</script>

<style lang="scss">
.home {
  .loading {
    padding-top: 120px;
    align-items: flex-start;
  }
  .search {
    display: flex;
    padding: 32px 16px;
    input {
      max-width: 35rem;
      width: 100%;
      padding: 12px 6px;
      font-size: 1.6rem;
      border: none;
      &:focus {
        outline: none;
      }
    }
    .button {
      border-top-left-radius: 0;
      border-bottom-left-radius: 0;
      font-size: 1.4rem;
    }
  }
  .movies {
    padding: 32px 16px;
    .movies-grid {
      display: grid;
      column-gap: 3.2rem;
      row-gap: 64 px;
      grid-template-columns: 1fr;
      @media (min-width: 500px) {
        grid-template-columns: repeat(2, 1fr);
      }
      @media (min-width: 750px) {
        grid-template-columns: repeat(3, 1fr);
      }
      @media (min-width: 1100px) {
        grid-template-columns: repeat(4, 1fr);
      }
      .movie {
        position: relative;
        display: flex;
        flex-direction: column;
        .movie-img {
          position: relative;
          overflow: hidden;
          &:hover {
            .overview {
              transform: translateY(0);
              
            }
          }
          img {
            display: block;
            width: 100%;
            height: 100%;
          }
          .review {
            position: absolute;
            top: 0;
            left: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            width: 4rem;
            height: 4rem;
            background-color: #c92502;
            color: #fff;
            border-radius: 0 0 16px 0;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
              0 2px 4px -1px rgba(0, 0, 0, 0.06);
              font-size: 1.5rem;
          }
          .overview {
            line-height: 1.5;
            position: absolute;
            bottom: 0;
            background-color: rgba(201, 38, 2, 0.9);
            padding: 20px;
            color: #fff;
            transform: translateY(100%);
            transition: 0.3s ease-in-out all;
            font-size: 1.4rem;
          }
        }
        .info {
          margin-top: auto;
          .title {
            margin-top: .8rem;
            color: #fff;
            font-size: 2rem;
          }
          .release {
            font-size: 1.5rem;
            margin-top: .8rem;
            color: #c9c9c9;
          }
          .button {
            margin: 8px 0;
          
            font-size: 1.3rem;
          }
        }
      }
    }
  }
}
</style>