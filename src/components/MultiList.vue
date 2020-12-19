<template>
  <div>
    <button @click="getTweets">ツイート取得</button>
    <div
      class="multi-list"
      v-for="multi in multiList"
      :key="multi.multiInfo"
      v-clipboard:copy="multi.id"
      v-clipboard:success="onCopy"
      v-clipboard:error="onError"
    >
      {{ multi.lv }}
      {{ multi.enemy }}
      {{ multi.id }}
    </div>
  </div>
</template>

<script>
import Twitter from "twitter";

export default {
  data: function () {
    return {
      multiList: [],
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
      client.stream("statuses/filter", { track: ":参戦ID" }, function (stream) {
        stream.on("data", function (tweet) {
          let splittedTweet = tweet.text.split(/[ \n]/);
          let i = 0;
          splittedTweet.forEach(function (value, index) {
            if (value == ":参戦ID") {
              i = index;
            }
          });

          //データ加工
          let multiInfos = {
            lv: splittedTweet[i + 2],
            enemy: splittedTweet[i + 3],
            id: splittedTweet[i - 1],
          };
          // console.log("multiInfos : ", multiInfos);

          self.multiList.push(multiInfos);
          if (self.multiList.length > 10) {
            self.multiList.shift();
          }
        });

        stream.on("error", function (error) {
          console.log(error);
        });
      });
    },
    onCopy: function (id) {
      console.log("You just copied: " + id.text);
    },
    onError: function () {
      alert("Failed to copy texts");
    },
  },
};
</script>
