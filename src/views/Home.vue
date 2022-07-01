<template>
  <div class="home">
    <div class="container">
      <form class="form" @submit.prevent="onSubmitHandle">
        <small>All components are searchable</small>
        <div class="categories">
          <h3>Categories</h3>
          <base-widget
            :url="'https://lobster.tools/api/v1/categories'"
            v-model="selectedCategories"
          />
          <p>v-model - {{ selectedCategories }}</p>
        </div>
        <div class="projects">
          <h3>Projects</h3>
          <base-widget
            :url="'https://lobster.tools/api/v1/projects'"
            v-model="selectedProjects"
          />
          <p>v-model - {{ selectedProjects }}</p>
        </div>
        <div class="selected-elements">
          <h3>Projects with selected elements</h3>
          <base-widget
            :url="'https://lobster.tools/api/v1/categories'"
            v-model="selectedElements"
          />
          <p>v-model - {{ selectedElements }}</p>
        </div>
        <div class="form__actions">
          <button class="btn" type="submit">Submit</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import BaseWidget from "../components/BaseWidget.vue";
import { defineComponent, ref } from "vue";
export default defineComponent({
  components: { BaseWidget },
  setup() {
    // vars
    let selectedCategories = ref([]);
    let selectedProjects = ref([]);
    let selectedElements = ref([27, 29]);
    // methods
    function onSubmitHandle() {
      //   send selected items to the server
      alert("send selected items to the server");
    }
    return {
      selectedCategories,
      selectedProjects,
      selectedElements,
      onSubmitHandle,
    };
  },
});
</script>

<style lang="scss">
.form {
  &__actions {
    display: flex;
    align-items: center;
    gap: 5px;
    margin-top: 10px;
  }
}
</style>
