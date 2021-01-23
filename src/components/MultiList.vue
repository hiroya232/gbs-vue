<template>
  <div>
    <GetTweet
      multiType="救援マルチ"
      search-keyword="参加者募集！"
      enemy="アーカーシャ"
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
