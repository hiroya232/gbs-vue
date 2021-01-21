<template>
  <div>
    <button @click="getTweets(searchKeyword, enemy)">{{ multiType }}</button>
    <div
      class="coop-multi-list"
      v-for="MultiInfo in MultiList"
      :key="MultiInfo.id"
      v-clipboard:copy="MultiInfo.roomId"
      v-clipboard:success="onCopy"
      v-clipboard:error="onError"
    >
      <div>{{ MultiInfo.enemyInfo }} {{ MultiInfo.roomId }}</div>
    </div>
  </div>
</template>

<script>
import Twitter from "twitter";

export default {
  data: function () {
    return {
      MultiList: [],
    };
  },
  props: ["multiType", "searchKeyword", "enemy"],
  methods: {
    getTweets(searchKeyword, ...enemy) {
      console.log("検索開始");
      console.log(searchKeyword);
      console.log(enemy);
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
        { track: `${searchKeyword} + ${enemy}` },
        function (stream) {
          stream.on("data", function (tweet) {
            let splittedTweet = tweet.text.split(/[ \n]/);
            console.log(splittedTweet);
            let roomId = splittedTweet.slice(0, 2)[0].split("：")[0];
            console.log(roomId);

            //ルームidが同じ時は無視
            if (
              self.MultiList.map((obj) => obj.roomId).includes(roomId) ||
              self.MultiList === null
            ) {
              return;
            }

            let enemyInfo;
            let MultiInfos;
            if (searchKeyword === ":参戦ID") {
              enemyInfo = splittedTweet.slice(-2)[0];
            } else {
              enemyInfo = splittedTweet.slice(-1)[0];
            }
            console.log(enemyInfo);

            //データ加工
            MultiInfos = {
              id: id,
              roomId: roomId,
              enemyInfo: enemyInfo,
            };

            id++;

            self.MultiList.splice(0, 0, MultiInfos);
            console.log(self.MultiList);

            //10件まで表示
            if (self.MultiList.length > 10) {
              self.MultiList.splice(10, 1);
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
