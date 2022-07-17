<template>
  <div class="header pa-3">
    <div class="d-flex justify-space-between align-center">
      <v-btn color="primary" @click="candidates_view = true">candidatos</v-btn>
      <v-btn color="success" @click="candidates_data = true">pontuação</v-btn>
    </div>
    <v-dialog v-model="candidates_view" width="500">
      <v-card>
        <v-card-title class="text-h5 primary white--text lighten-2">
          Candidatos
        </v-card-title>
        <v-row class="mx-2">
          <v-col>
            <v-card-text class="py-5 d-flex">
              <div class="d-flex justify-center align-center flex-column">
                <h2>Protagonistas</h2>
                <div v-for="(protagonist, index) in protagonists" :key="index">
                  <div class="candidate-card mx-2 my-4">
                    <img
                      :src="protagonist.pic"
                      alt="candidate"
                      class="candidate-pic"
                    />
                    <h1 class="py-2 text-center">{{ protagonist.name }}</h1>
                    <h2 class="text-center pb-3">{{ protagonist.number }}</h2>
                  </div>
                </div>
              </div>
            </v-card-text>
          </v-col>
          <v-col>
            <v-card-text class="py-5 d-flex">
              <div class="d-flex justify-center align-center flex-column">
                <h2>Antagonistas</h2>
                <div v-for="(antagonists, index) in antagonists" :key="index">
                  <div class="candidate-card mx-2 my-4">
                    <img
                      :src="antagonists.pic"
                      alt="candidate"
                      class="candidate-pic"
                    />
                    <h1 class="py-2 text-center">{{ antagonists.name }}</h1>
                    <h2 class="text-center pb-3">{{ antagonists.number }}</h2>
                  </div>
                </div>
              </div>
            </v-card-text>
          </v-col>
        </v-row>
        <v-divider></v-divider>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="candidates_view = false"
            >Fechar</v-btn
          >
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="candidates_data">
      <v-row>
        <v-col>
          <h2 class="white--text text-center">Protagonistas</h2>
          <v-data-table
            :headers="headers"
            :items="protagonists"
            :items-per-page="5"
            :custom-sort="customSort"
            class="elevation-1"
          >
            <template #[`item.pic`]="{ item }">
              <img :src="item.pic" style="width: 60px; height: 60px" />
            </template>
          </v-data-table>
        </v-col>
        <v-col>
          <h2 class="white--text text-center">Antagonistas</h2>
          <v-data-table
            :headers="headers"
            :items="antagonists"
            :items-per-page="5"
            :custom-sort="customSort"
            class="elevation-1"
          >
            <template #[`item.pic`]="{ item }">
              <img :src="item.pic" style="width: 60px; height: 60px" />
            </template>
          </v-data-table>
        </v-col>
      </v-row>
    </v-dialog>
  </div>
</template>

<script>
import candidates_list from "../candidates.js";

export default {
  data() {
    return {
      candidates_view: false,
      candidates_data: false,
      protagonists: [],
      antagonists: [],
      headers: [
        {
          text: "Foto",
          align: "start",
          sortable: false,
          value: "pic",
          width: "10px",
        },
        {
          text: "Nome",
          sortable: false,
          value: "name",
        },
        { text: "Partido", value: "partido", sortable: false },
        { text: "Número", value: "number", sortable: false },
        { text: "Votos", value: "votes" },
      ],
    };
  },
  mounted() {
    if (
      localStorage.getItem("protagonists") &&
      localStorage.getItem("antagonists")
    ) {
      this.protagonists = JSON.parse(localStorage.getItem("protagonists"));
      this.antagonists = JSON.parse(localStorage.getItem("antagonists"));
    } else {
      this.protagonists = candidates_list.protagonists_data;
      this.antagonists = candidates_list.antagonists_data;
    }
  },
  methods: {
    customSort(items, index, isDesc) {
      items.sort((a, b) => {
        if (isDesc != "false") {
          return a[index] < b[index] ? -1 : 1;
        } else {
          return b[index] < a[index] ? -1 : 1;
        }
      });
      return items;
    },
  },
};
</script>

<style lang="scss" scoped>
.candidate-card {
  border: 1px solid black;
  border-radius: 6px;
  width: 100%;
  .candidate-pic {
    width: 100%;
  }
}
</style>
