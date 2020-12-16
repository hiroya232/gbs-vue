<template>
  <div>
    <button @click="getTweets">ツイート取得</button>
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
            multiInfo:
              splittedTweet[i + 2] +
              " " +
              splittedTweet[i + 3] +
              " " +
              splittedTweet[i - 1],
          };
          console.log("multiInfos : ", multiInfos.multiInfo);

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
  },
};
</script>
