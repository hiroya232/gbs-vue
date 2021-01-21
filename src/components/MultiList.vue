<template>
  <div>
    <GetTweet
      multiType="救援マルチ"
      search-keyword=":参戦ID"
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
// import Twitter from "twitter";

// export default {
//   data: function () {
//     return {
//       multiList: [],
//     };
//   },
//   methods: {
//     getTweets(enemy) {
//       let client = new Twitter({
//         consumer_key: process.env.VUE_APP_CONSUMER_KEY,
//         consumer_secret: process.env.VUE_APP_CONSUMER_SECRET,
//         access_token_key: process.env.VUE_APP_ACCESS_TOKEN_KEY,
//         access_token_secret: process.env.VUE_APP_ACCESS_TOKEN_SECRET,
//       });

//       //ツイート取得
//       let self = this;
//       client.stream(
//         "statuses/filter",
//         { track: `:参戦ID ${enemy}` },
//         function (stream) {
//           stream.on("data", function (tweet) {
//             let splittedTweet = tweet.text.split(/[ \n]/);
//             let i = 0;
//             splittedTweet.forEach(function (value, index) {
//               if (value == ":参戦ID") {
//                 i = index;
//               }
//             });

//             //データ加工
//             let multiInfos = {
//               lv: splittedTweet[i + 2],
//               enemy: splittedTweet[i + 3],
//               id: splittedTweet[i - 1],
//             };

//             console.log("multiInfos : ", multiInfos);

//             if (multiInfos.enemy == enemy) {
//               self.multiList.splice(0, 0, multiInfos);
//             }
//             if (self.multiList.length > 10) {
//               self.multiList.splice(10, 1);
//             }
//           });

//           stream.on("error", function (error) {
//             console.log(error);
//           });
//         }
//       );
//     },
//     onCopy: function (id) {
//       alert("You just copied: " + id.text);
//     },
//     onError: function () {
//       alert("Failed to copy texts");
//     },
//   },
// };
//
//
</script>
