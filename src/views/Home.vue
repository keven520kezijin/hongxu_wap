<template>
  <div class="home">
    <h-form :formId="active" />
  </div>
</template>

<script>
// @ is an alias to /src
import hForm from "./form.vue";

export default {
  name: "Home",
  components: { hForm },
  data() {
    return {
      village: ["虹旭小区", "虹旭二小区", "虹警新苑", "中星雅苑"],
      active: 0
    };
  }
};
</script>
