<template>
  <div>
    <GetTweet
      multiType="共闘マルチ"
      search-keyword="［グラブル］マルチバトル参加者募集！"
      enemy=""
    ></GetTweet>
  </div>
</template>

<script>
import GetTweet from "./GetTweet.vue";

export default {
  data: function () {
    return {
      coopMultiList: [],
    };
  },
  components: {
    GetTweet,
  },
};
</script>
