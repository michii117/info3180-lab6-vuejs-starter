<template>
    <ul class="news__list">
        <li v-for="article in articles" class="news__item">
            <div class="image-container">
                <img class="images" v-bind:src= article.urlToImage />
            </div>
            <h6 style="padding:15px 10px; font-size: 20px;">{{ article.title }}</h6>
            <p style="padding:0px 10px; font-size: 18px;">{{ article.description }}</p>
        </li>
    </ul>
</template>

<script>
export default {
 data() {
 return {
 articles: []
 }
 },
 created() {
 let self = this;
 fetch('https://newsapi.org/v2/top-headlines?country=us',
{
 headers: {
 'Authorization': `Bearer ${import.meta.env.VITE_NEWSAPI_TOKEN}`
 }
})
 .then(function(response) {
 return response.json();
 })
 .then(function(data) {
 console.log(data);
 self.articles = data.articles;
 });
 }
}
</script>

<style>

li.news__item {
    width: 29%;
    filter: drop-shadow(2px 4px 6px #00000033);
    background-color: white;
    margin: 1%;
    display: inline-block;
    height: 500px;
    vertical-align: middle;
    border-bottom: solid 7px #0d6efd;
}

.images{
    width: 100%;
    height: inherit;
}

.image-container{
    height:200px;
}

ul {
    list-style-type: none;
    margin: 0;
  padding: 0;
  overflow: hidden;
}
</style>