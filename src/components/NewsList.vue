<template>

    <form @submit.prevent="searchNews" class="d-flex flexcolumn justify-content-center">
        <div class="input-group mx-sm-3 mb-2">
            <label class="visually-hidden" for="search">Search</label>
            <input type="search" name="search" v-model="searchTerm" id="search" class="form-control mb-2 mr-sm-2" placeholder="Enter search term here" />
            <button class="btn btn-primary mb-2">Search</button>
        </div>
    </form>
    <p>You are searching for {{ searchTerm }}</p>

    <ul class="news__list">
        <li v-for="article in articles" class="news__item">
            <div class="image-container">
                <img class="images" v-bind:src="article.urlToImage" />
            </div>
            <h6 style="padding:15px 15px; font-size: 20px;text-align: left;">{{ article.title }}</h6>
            <p style="padding:0px 15px; font-size: 18px;text-align: left;">{{ article.description }}</p>
        </li>
    </ul>
</template>

<script>
    export default {
        data() {
            return {
                articles: [],
                searchTerm: ''
            }
        },

        methods: {
            searchNews() {
                let self = this;

                fetch('https://newsapi.org/v2/everything?q='+ self.searchTerm + '&language=en', {
                    headers: {'Authorization': `Bearer ${import.meta.env.VITE_NEWSAPI_TOKEN}`,}
                })

                .then(function(response) {
                    return response.json();
                })
                .then(function(data) {
                    console.log(data);
                    self.articles = data.articles;
                });
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