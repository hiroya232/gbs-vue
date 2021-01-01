<template>
  <div>
    <MultiList></MultiList>
    <CoopMultiList></CoopMultiList>
  </div>
</template>

<script>
import MultiList from "./components/MultiList.vue";
import CoopMultiList from "./components/CoopMultiList.vue";

export default {
  name: "App",
  components: {
    MultiList,
    CoopMultiList,
  },
};
</script>
