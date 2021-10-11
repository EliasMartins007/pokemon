<template>
  <q-page class="flex flex-center">
    <h2>{{ name }}</h2>
    <q-img :src="url" width="350px" />
    <div class="row justify-around full-width">
      <q-input filled v-model="search" label="digite nome do pokemon" />
      <q-btn color="amber" label="Pesquisar" @click="getPokemon" />
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";
import api from "../services/api";

export default defineComponent({
  name: "PageIndex",
  data() {
    //return { currentPokemon: {} }; //veim como obj {} por isso chaves olhae isso na api da empresa
    return { name: "", url: "", search: "ditto" };
  },

  async beforeMount() {
    // api
    //   .get("/pokemon/2/")
    //   .then((response) => {
    //     // handle success
    //     console.log(response);
    //     //sucesso tra pokemon
    //     //this.currentPokemon = response.data;
    //     this.name = response.data.name;
    //     this.url = response.data.sprites.other.dream_world.front_default;
    //   })
    //   .catch((error) => {
    //     // handle error
    //     console.log(error);
    //   })
    //   .then(() => {
    //     // always executed
    //   });
    await this.getPokemon();
  },
  methods: {
    getPokemon() {
      api
        .get(`/pokemon/${this.search}/`)
        .then((response) => {
          // handle success
          console.log(response);
          //sucesso tra pokemon
          //this.currentPokemon = response.data;
          this.name = response.data.name;
          this.url = response.data.sprites.other.dream_world.front_default;
          this.triggerPositive();
        })
        .catch((error) => {
          // handle error
          //console.log(error);
          this.triggerNegative();
        });
    }, //fim metodo
    triggerPositive() {
      this.$q.notify({
        type: "positive",
        position: "top",
        message: "Pokemon encontrado !",
      });
    },

    triggerNegative() {
      this.$q.notify({
        type: "negative",
        position: "top",
        message: "OPS Ocorreu um erro, tente novamente!",
      });
    },
  },
});
</script>
