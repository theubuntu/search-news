<template>
  <v-app>
    <v-main>
      <router-view/>
    <v-row>
      <v-container>
        <v-col
        >
        <v-text-field
        label="Buscar noticias por frase, palabra clave o fuente"
        solo
        @keyup="buscar()"
        v-model="buscador"
        >
        
        </v-text-field>
        </v-col>
      </v-container>
    </v-row>

    <v-container>
    <v-row>
      <v-card
      class="mx-auto my-3"
      max-width="350"
      v-for="noticia in news"
      :key="noticia.url"
      >
        <v-img
        height="200px"
        :src="noticia.urlToImage"
        >
          
        </v-img>
        <v-card-title>
          {{noticia.title}}
        </v-card-title>
        <v-card-subtitle>
          {{noticia.source.name}}
        </v-card-subtitle>
        <v-card-text>
          {{noticia.description}}
        </v-card-text>
        <v-card-actions>
          <v-btn
          text
          :href="noticia.url"
          color="teal accent-4"
          >
            Leer Más
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-row>
    </v-container>
    </v-main>
  </v-app>
</template>

<script>
import axios from "axios";
export default {
  name: 'App',

  data: () => ({
    //
    news:{},
    buscador:''
  }),
  methods:{
    async getNews(){
      let apiURL='https://newsapi.org/v2/top-headlines?country=mx&apiKey=ddd1d09c488e47648e3e1b3292598dbf'
      let noticias = await this.axios.get(apiURL)
      this.news=noticias.data.articles
      console.log(this.news)
    },
    buscar(){
      this.axios.get(`https://newsapi.org/v2/everything?q=${this.buscador}&apiKey=ddd1d09c488e47648e3e1b3292598dbf`)
        .then(res=>{
            this.news=res.data.articles
                console.log(res)
                }).catch(error=>{
                    console.log("Error", error)
                })
    }
  },
  created(){
    this.getNews()
  }
};
</script>
