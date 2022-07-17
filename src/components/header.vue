<template>
  <div class="header pa-3">
    <div class="d-flex justify-space-between align-center">
      <v-btn color="primary" @click="candidates_view = true">candidatos</v-btn>
      <v-btn color="success">pontuação</v-btn>
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
  </div>
</template>

<script>
import candidates_list from "../candidates.js";

export default {
  data() {
    return {
      candidates_view: false,
      protagonists: [],
      antagonists: [],
    };
  },
  mounted() {
    this.protagonists = candidates_list.protagonists_data;
    this.antagonists = candidates_list.antagonists_data;
    console.log(this.protagonists);
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
