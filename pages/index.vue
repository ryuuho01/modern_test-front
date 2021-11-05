<template>
  <div class="homecontainer">
    <p class="main-title">独り言App</p>
    <p>つぶやく内容を入力してください</p>
    <input v-model="newTweet" type="text">
    <button @click="insertTweet">つぶやく</button>
    <p class="sub-title">つぶやき一覧</p>
    <!-- 呟き枠 -->
    <ul>
      <li class="tweets" v-for="item in tweetCurrent" :key="item.id">
        <!-- 赤丸とツイート内容 -->
        <div class="left">
          <div class="redcircle"></div>
          <p class="tweet">{{item.tweet}}</p>
        </div>
        <!-- 削除ボタン -->
        <button class="delete" @click="deleteButton(item.id)">削除</button>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";
export default {
    data() {
    return {
      newTweet: "",
      tweetCurrent: [],

    }
  },

    mounted() {
    this.getTweet();
    },

    methods: {
    async getTweet() {
      let res = [];
      const tweetresData = await this.$axios.get("http://127.0.0.1:8000/api/tweet/");
      for(let i=tweetresData.data.data.length-1; i+1 > tweetresData.data.data.length-7 ; i--) {
      // for(let i=0; i<tweetresData.data.data.length ; i++) {
        if( i < 0 ){
          break
        } else {
          res.push(tweetresData.data.data[i]);
        }
      }
      this.tweetCurrent = res;
    },

    async insertTweet() {
      const sendpostData = {
        tweet: this.newTweet,
      };
      await this.$axios.post("http://127.0.0.1:8000/api/tweet/", sendpostData);
      this.newTweet = '';

      this.getTweet();
    
    },

    deleteButton: async function(item_id) {
        const deletepath = "http://127.0.0.1:8000/api/tweet/"+item_id;
        await axios
        .delete(deletepath);
        this.getTweet();
    },

  }
}
</script>

<style>
/* ----------------------リセット---------------------------- */
html, body, div, span, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
abbr, address, cite, code,
del, dfn, em, img, ins, kbd, q, samp,
small, strong, sub, sup, var,
b, i,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, figcaption, figure,
footer, header, hgroup, menu, nav, section, summary,
time, mark, audio, video {
    margin:0;
    padding:0;
    border:0;
    outline:0;
    font-size:100%;
    vertical-align:baseline;
    background:transparent;
}

body {
    line-height:1;
}

article,aside,details,figcaption,figure,
footer,header,hgroup,menu,nav,section {
    display:block;
}

nav ul {
    list-style:none;
}

blockquote, q {
    quotes:none;
}

blockquote:before, blockquote:after,
q:before, q:after {
    content:'';
    content:none;
}

a {
    margin:0;
    padding:0;
    font-size:100%;
    vertical-align:baseline;
    background:transparent;
}

/* change colours to suit your needs */
ins {
    background-color:#ff9;
    color:#000;
    text-decoration:none;
}

/* change colours to suit your needs */
mark {
    background-color:#ff9;
    color:#000;
    font-style:italic;
    font-weight:bold;
}

del {
    text-decoration: line-through;
}

abbr[title], dfn[title] {
    border-bottom:1px dotted;
    cursor:help;
}

table {
    border-collapse:collapse;
    border-spacing:0;
}

/* change border colour to suit your needs */
hr {
    display:block;
    height:1px;
    border:0;
    border-top:1px solid #cccccc;
    margin:1em 0;
    padding:0;
}

input, select {
    vertical-align:middle;
}
/* -------------------------------------------------- */

.homecontainer {
  text-align: center;
  margin-top: 50px;
}

.main-title {
  margin: 20px;
  margin-bottom: 50px;
  font-size: 30px;
  font-weight: bold;
}

.sub-title {
  margin: 50px;
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 20px;
}

.tweets:first-child {
  border-top: 1px solid grey;
}
.tweets {
  border: 1px solid grey;
  border-top: none;
  border-bottom: none;
  width: 400px;
  display: flex;
  margin: auto;
  padding: 10px 20px;
}
.tweets:last-child {
  border-bottom: 1px solid grey;
  padding-bottom: 10px;
}

.left {
  display: flex;
}

.redcircle {
  height: 40px;
  width: 40px;
  border-radius: 50%;
  background: red;
  margin: auto;
  margin-right: 5px;
}

.tweet {
  margin: auto;
  text-align: left;

}

.delete {
  height: 30px;
  margin: auto 0 auto auto;
  border-radius: 5px;
  border: 1px solid;
}

</style>