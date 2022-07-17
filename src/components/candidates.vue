<template>
  <div class="candidate-wrapper d-flex justify-center align-center flex-column">
    <img :src="pic" alt="candidate" class="candidate-pic" />
    <div
      v-if="!whiteVote"
      class="d-flex justify-center align-center flex-column"
    >
      <h1 class="mt-6">{{ name }}</h1>
      <p><span class="font-weight-bold">Partido:</span> {{ partido }}</p>
      <p><span class="font-weight-bold">Número:</span> {{ number }}</p>
    </div>
    <div v-else>
      <h1 class="mt-3 text-center">VOTO BRANCO</h1>
      <p class="text-center grey--text">
        Todo candidato não encontrado será votado por branco
      </p>
      <p class="text-center grey--text">
        O voto será distribuido
        <span class="text-decoration-underline">aleatoramente</span>
      </p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    pic: {
      type: String,
      required: true,
      default: require("../assets/user.jpg"),
    },
    name: {
      type: String,
      required: true,
      default: "Default name",
    },
    partido: {
      type: String,
      required: true,
      default: "Default partido",
    },
    number: {
      type: Number,
      required: true,
      default: 0,
    },
    whiteVote: {
      type: Boolean,
      required: true,
      default: false,
    },
  },
};
</script>

<style lang="scss" scoped>
.candidate-wrapper {
  width: 100%;
  .candidate-pic {
    width: 50%;
    height: auto;
  }
}
</style>
