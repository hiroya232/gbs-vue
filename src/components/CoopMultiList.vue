<template>
  <div>
    <GetMultiTweet
      multiType="共闘マルチ"
      search-keyword="［グラブル］マルチバトル参加者募集！"
    ></GetMultiTweet>
  </div>
</template>

<script>
import GetMultiTweet from "./GetMultiTweet.vue";

export default {
  data: function () {
    return {
      coopMultiList: [],
    };
  },
  components: {
    GetMultiTweet,
  },
};
</script>
