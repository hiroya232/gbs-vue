<template>
  <div>
    <button @click="getTweets">共闘マルチ検索</button>
    <div
      class="coop-multi-list"
      v-for="coopMulti in coopMultiList"
      :key="coopMulti.id"
      v-clipboard:copy="coopMulti.id"
      v-clipboard:success="onCopy"
      v-clipboard:error="onError"
    >
      <div>{{ coopMulti.info }}</div>
      <div>{{ coopMulti.id }}</div>
    </div>
  </div>
</template>

<script>
import Twitter from "twitter";

export default {
  data: function () {
    return {
      coopMultiList: [],
    };
  },
  methods: {
    getTweets() {
      let client = new Twitter({
        consumer_key: process.env.VUE_APP_CONSUMER_KEY,
        consumer_secret: process.env.VUE_APP_CONSUMER_SECRET,
        access_token_key: process.env.VUE_APP_ACCESS_TOKEN_KEY,
        access_token_secret: process.env.VUE_APP_ACCESS_TOKEN_SECRET,
      });

      //ツイート取得
      let self = this;
      client.stream(
        "statuses/filter",
        { track: "［グラブル］マルチバトル参加者募集！" },
        function (stream) {
          stream.on("data", function (tweet) {
            let splittedTweet = tweet.text.split(/[ \n]/);
            let coopInfo = splittedTweet.slice(-1)[0];
            let coopId = splittedTweet.slice(0, 1)[0].split("：")[0];
            // console.log(tweet);
            console.log(splittedTweet);
            console.log(coopInfo);
            console.log(coopId);

            //データ加工
            let coopMultiInfos = {
              id: coopId,
              info: coopInfo,
            };

            self.coopMultiList.splice(0, 0, coopMultiInfos);

            if (self.coopMultiList.length > 10) {
              self.coopMultiList.splice(10, 1);
            }
          });

          stream.on("error", function (error) {
            console.log(error);
          });
        }
      );
    },
    onCopy: function (id) {
      alert("You just copied: " + id.text);
    },
    onError: function () {
      alert("Failed to copy texts");
    },
  },
};
</script>
