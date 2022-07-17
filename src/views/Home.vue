<template>
  <div class="home">
    <v-snackbar v-model="no_candidate" top color="error">
      <span class="text-h6"
        >Candidato não encontrado, acesse a lista de
        <span class="text-decoration-underline font-weight-bold"
          >candidatos</span
        >
        para mais detalhes</span
      >
      <template v-slot:action="{ attrs }">
        <v-btn color="white" text v-bind="attrs" @click="no_candidate = false">
          Fechar
        </v-btn>
      </template>
    </v-snackbar>
    <v-dialog
      v-model="success_msg"
      transition="dialog-top-transition"
      max-width="600"
      persistent
    >
      <template v-slot:default="dialog">
        <v-card>
          <v-toolbar color="success" dark class="text-h6"
            >Votação Finalizada com sucesso</v-toolbar
          >
          <v-card-text>
            <div class="text-h5 pa-5">
              Parabéns! Seu voto já está contabilizado. Para acessar as
              pontuações basta
              <span class="text-decoration-underline primary--text"
                >clicar aqui</span
              >.
            </div>
          </v-card-text>
          <v-card-actions class="justify-end">
            <v-btn text @click="dialog.value = false">Fechar</v-btn>
          </v-card-actions>
        </v-card>
      </template>
    </v-dialog>
    <div class="urn-size">
      <v-row class="urn-box">
        <v-col class="view-urn-box d-flex justify-center align-center" cols="7">
          <div
            class="d-flex justify-center align-center flex-column"
            v-if="candidate_view"
          >
            <h1>{{ voting_title }}</h1>
            <div v-if="voteState === 'voting'" class="d-flex">
              <v-row
                class="mt-6"
                v-for="(votes, index) in voteValue"
                :key="index"
              >
                <v-col
                  class="number-box d-flex justify-center align-center mx-5"
                  cols="3"
                  ><span class="font-weight-bold">{{ votes }}</span></v-col
                >
              </v-row>
            </div>
            <div v-if="voteState === 'start'" class="d-flex">
              <v-row class="mt-6">
                <v-col
                  class="number-box d-flex justify-center align-center mx-2 pisca"
                ></v-col>
                <v-col
                  class="number-box d-flex justify-center align-center mx-2 pisca"
                ></v-col>
                <v-col
                  v-if="villains_turn === true"
                  class="number-box d-flex justify-center align-center mx-2 pisca"
                ></v-col>
              </v-row>
            </div>
          </div>
          <div v-else>
            <candidates
              :pic="candidate.pic"
              :name="candidate.name"
              :partido="candidate.partido"
              :number="candidate.number"
              :white-vote="white_vote"
            />
          </div>
        </v-col>
        <v-col class="col-bg d-flex align-center justify-center">
          <div>
            <div class="numbers-shrink">
              <v-row>
                <v-col class="d-flex justify-center align-center">
                  <v-btn @click="vote(1)">1</v-btn>
                </v-col>
                <v-col class="d-flex justify-center align-center">
                  <v-btn @click="vote(2)">2</v-btn>
                </v-col>
                <v-col class="d-flex justify-center align-center">
                  <v-btn @click="vote(3)">3</v-btn>
                </v-col>
              </v-row>
              <v-row>
                <v-col class="d-flex justify-center align-center">
                  <v-btn @click="vote(4)">4</v-btn>
                </v-col>
                <v-col class="d-flex justify-center align-center">
                  <v-btn @click="vote(5)">5</v-btn>
                </v-col>
                <v-col class="d-flex justify-center align-center">
                  <v-btn @click="vote(6)">6</v-btn>
                </v-col>
              </v-row>
              <v-row>
                <v-col class="d-flex justify-center align-center">
                  <v-btn @click="vote(7)">7</v-btn>
                </v-col>
                <v-col class="d-flex justify-center align-center">
                  <v-btn @click="vote(8)">8</v-btn>
                </v-col>
                <v-col class="d-flex justify-center align-center">
                  <v-btn @click="vote(9)">9</v-btn>
                </v-col>
              </v-row>
              <v-row>
                <v-col class="d-flex justify-center align-center">
                  <v-btn @click="vote(0)">0</v-btn>
                </v-col>
              </v-row>
            </div>
            <v-row class="mt-16">
              <v-col class="d-flex justify-center align-center">
                <v-btn
                  @click="
                    white_vote = true;
                    candidate_view = false;
                  "
                  >BRANCO</v-btn
                >
              </v-col>
              <v-col class="d-flex justify-center align-center">
                <v-btn color="warning" @click="clear">CORRIGIR</v-btn>
              </v-col>
              <v-col class="d-flex justify-center align-center">
                <v-btn color="success" @click="confirm">CONFIRMAR</v-btn>
              </v-col>
            </v-row>
            <v-row v-if="villains_turn">
              <v-col class="d-flex justify-start align-center">
                <v-btn color="primary" @click="restart">REINICIAR</v-btn>
              </v-col>
            </v-row>
          </div>
        </v-col>
      </v-row>
    </div>
  </div>
</template>

<script>
import candidates from "../components/candidates.vue";
import candidates_list from "../candidates.js";

export default {
  components: {
    candidates,
  },
  data() {
    return {
      voting_title: "MELHOR PROTAGONISTA",
      voteState: "start",
      voteValue: [],
      candidate_view: true,
      candidate: [],
      protagonists: [],
      antagonists: [],
      villains_turn: false,
      success_msg: false,
      white_vote: false,
      no_candidate: false,
    };
  },
  mounted() {
    this.protagonists = candidates_list.protagonists_data;
    this.antagonists = candidates_list.antagonists_data;
  },
  methods: {
    vote(value) {
      if (this.villains_turn === false) {
        if (this.voteValue.length < 2) {
          this.voteValue.push(value);
          console.log(this.voteValue);
          this.voteState = "voting";
        }
      } else {
        if (this.voteValue.length < 3) {
          this.voteValue.push(value);
          console.log(this.voteValue);
          this.voteState = "voting";
        }
      }
      const formatedVoteValue = this.voteValue.toString().replace(/,/g, "");
      const voteValue = parseInt(formatedVoteValue);
      if (this.villains_turn === false) {
        const protagonists_number = this.protagonists.map(
          (candidate) => candidate.number
        );
        if (protagonists_number.includes(voteValue)) {
          this.candidate = this.protagonists.find(
            (candidate) => candidate.number === voteValue
          );
          this.candidate_view = false;
          this.white_vote = false;
        } else {
          this.voteValue.length === 2 ? (this.no_candidate = true) : null;
        }
      } else {
        const antagonists_number = this.antagonists.map(
          (candidate) => candidate.number
        );
        if (antagonists_number.includes(voteValue)) {
          this.candidate = this.antagonists.find(
            (candidate) => candidate.number === voteValue
          );
          this.candidate_view = false;
          this.white_vote = false;
        } else {
          this.voteValue.length === 3 ? (this.no_candidate = true) : null;
        }
      }
    },
    clear() {
      this.voteValue = [];
      this.voteState = "start";
      this.candidate_view = true;
      this.candidate = [];
    },
    confirm() {
      if (this.villains_turn === false) {
        this.villains_turn = true;
        this.voteState = "start";
        this.candidate_view = true;
        this.candidate = [];
        this.voteValue = [];
        this.voting_title = "MELHOR ANTAGONISTA";
      } else {
        this.villains_turn = false;
        this.voteState = "start";
        this.candidate_view = true;
        this.candidate = [];
        this.voteValue = [];
        this.voting_title = "MELHOR PROTAGONISTA";
        this.success_msg = true;
      }
    },
    restart() {
      this.voteState = "start";
      this.candidate_view = true;
      this.candidate = [];
      this.voteValue = [];
      this.voting_title = "MELHOR PROTAGONISTA";
      this.villains_turn = false;
    },
  },
};
</script>

<style lang="scss" scoped>
.home {
  width: 100%;
  height: 90vh;
  display: flex;
  align-items: center;
  justify-content: center;

  .col-bg {
    background-color: #80808076;
  }

  .urn-size {
    background: #80808043;
    padding: 2em;
    border-radius: 7px;
    width: 60%;

    .urn-box {
      height: 500px;
      .view-urn-box {
        background: white;
        width: 100%;
        height: 100%;

        .number-box {
          border: 1px solid black;
          height: 40px;
        }
      }

      .numbers-shrink {
        width: 70%;
        margin: 0 auto;
      }
    }
  }
}

@keyframes pisca {
  0% {
    opacity: 0.2;
  }
  50% {
    opacity: 1;
  }
  100% {
    opacity: 0.2;
  }
}

.pisca {
  animation-name: pisca;
  animation-duration: 1s;
  animation-iteration-count: infinite;
}
</style>
