<template>
  <div class="widget">
    <v-select
      multiple
      :modelValue="selected"
      @update:modelValue="updateModelValue"
      :reduce="(item) => item.id"
      label="title"
      :options="list"
      class="widget-select"
    />
    <div class="widget__action-right">
      <button
        @click="selectAllHandle"
        :disabled="selected.length === list.length"
        class="btn"
      >
        Select All
      </button>
    </div>
    <div class="widget__action">
      <button @click="cleanAll" type="button" class="btn">Clear</button>
    </div>
  </div>
</template>

<script>
import { defineComponent, onMounted, computed, ref } from "vue";
import axios from "axios";
let tokenStr = "BT3HK2NpCnyrKiDo";
axios.defaults.headers.common["Authorization"] = "Bearer " + tokenStr;

export default defineComponent({
  name: "BaseWidget",
  props: {
    modelValue: {
      type: Array,
      default: () => [],
    },
    url: {
      type: String,
      required: true,
    },
  },
  setup(props, context) {
    //   vars
    let list = ref([]);
    let selected = computed({
      get: () => props.modelValue,
      set: (value) => context.emit("update:modelValue", value),
    });

    onMounted(async () => {
      console.log(selected.value);
      await fetchData(props.url);
    });
    // methods
    function selectAllHandle() {
      let options = list.value;
      let optionsIds = [];
      options.forEach((item) => {
        optionsIds.push(item.id);
      });
      selected.value = optionsIds;
    }
    function updateModelValue(val) {
      context.emit("update:modelValue", val);
    }
    function cleanAll() {
      selected.value = [];
    }
    async function fetchData(url) {
      try {
        var formData = new FormData();
        formData.append("limit", 0);
        formData.append("offset", 0);
        await axios.post(url, formData).then((res) => {
          const data = res.data.message.data;
          list.value = data;
        });
      } catch (error) {
        return;
      }
    }
    return {
      list,
      selectAllHandle,
      selected,
      fetchData,
      cleanAll,
      updateModelValue,
    };
  },
});
</script>
<style lang="scss">
.widget {
  margin-top: 20px;
  display: flex;
  align-items: flex-end;
  gap: 5px;
  .widget-select {
    flex: 1;
  }
}
</style>
