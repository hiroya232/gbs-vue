<template>
  <div>
    <button @click="getTweets(searchKeyword)">{{ multiType }}</button>
    <div
      class="coop-multi-list"
      v-for="coopMulti in coopMultiList"
      :key="coopMulti.id"
      v-clipboard:copy="coopMulti.roomId"
      v-clipboard:success="onCopy"
      v-clipboard:error="onError"
    >
      <div>{{ coopMulti.info }} {{ coopMulti.roomId }}</div>
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
  props: ["multiType", "searchKeyword"],
  methods: {
    getTweets(searchKeyword) {
      console.log("検索開始");
      let client = new Twitter({
        consumer_key: process.env.VUE_APP_CONSUMER_KEY,
        consumer_secret: process.env.VUE_APP_CONSUMER_SECRET,
        access_token_key: process.env.VUE_APP_ACCESS_TOKEN_KEY,
        access_token_secret: process.env.VUE_APP_ACCESS_TOKEN_SECRET,
      });

      //ツイート取得
      let self = this;
      let id = 0;
      client.stream(
        "statuses/filter",
        { track: searchKeyword },
        function (stream) {
          stream.on("data", function (tweet) {
            let splittedTweet = tweet.text.split(/[ \n]/);
            let coopInfo = splittedTweet.slice(-1)[0];
            let roomId = splittedTweet.slice(0, 1)[0].split("：")[0];
            console.log("ツイート取得");

            //ルームidが同じ時は無視
            if (
              self.coopMultiList.map((obj) => obj.roomId).includes(roomId) ||
              self.coopMultiList === null
            ) {
              return;
            }

            console.log(splittedTweet);
            console.log(coopInfo);
            console.log(roomId);

            //データ加工
            let coopMultiInfos = {
              id: id,
              roomId: roomId,
              info: coopInfo,
            };
            id++;

            self.coopMultiList.splice(0, 0, coopMultiInfos);
            console.log(self.coopMultiList);

            //10件まで表示
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
    onCopy: function (roomId) {
      alert("You just copied: " + roomId.text);
    },
    onError: function () {
      alert("Failed to copy texts");
    },
  },
};
</script>
