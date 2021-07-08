<template>
  <div>
    <h2>Homepage</h2>
    <item v-for="story in stories" :key="story.data.id" :story="story"/>
  </div>
</template>

<script>
import axios from 'axios'
import Item from '@/components/Item'

export default {
  name: 'Homepage',
  components: {
    'item': Item
  },
  data: function () {
    return {
      err: '',
      stories: []
    }
  },
  created: function () {
    // https://hacker-news.firebaseio.com/v0/topstories.json
    axios.get('https://hacker-news.firebaseio.com/v0/topstories.json')
      .then((response) => {
        let results = response.data.slice(0, 11)
        results.forEach(id => {
          axios.get('https://hacker-news.firebaseio.com/v0/item/' + id + '.json')
            .then((response) => {
              this.stories.push(response)
            })
        })
      })
      .catch((err) => {
        this.err = err
      })
  }
}
</script>

<style scoped>
  .story {
    background-color: #fff;
    padding: 20px 30px 20px 80px;
    border-bottom: 1px solid #eee;
    position: relative;
    line-height: 20px;
  }
  .score {
    color: #f60;
    font-size: 1.1em;
    font-weight: 700;
    position: absolute;
    top: 50%;
    left: 0;
    width: 80px;
    text-align: center;
    margin-top: -10px;
  }
  .story a {
    color: #34495e;
    font-weight: 600;
    text-decoration: none;
  }
  .story a span {
    font-size: 0.85em;
    margin-left: 10px;
    color: #828282;
  }
  .story .meta {
    font-size: 0.85em;
    color: #828282;
  }
</style>
