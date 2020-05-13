<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>
        {{ currentQuestion.question | parse }}
      </template>

      <hr class="my-4" />

      <p>
        List of answers.
      </p>

      <b-button variant="primary" href="#">Submit</b-button>
      <b-button variant="success" href="#" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: {
    currentQuestion: Object,
    next: Function,
  },
  filters: {
    parse: function(value) {
      return value.replace(/&quot;/g, '"').replace(/&#039;/g, "'");
    },
  },
};
</script>
