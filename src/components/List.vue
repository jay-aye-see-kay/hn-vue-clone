<template>
    <div>
        <Story 
            v-for="num in currentStoryNums" 
            v-bind:key="num" 
            v-bind:storynum="num"
            v-bind:storyid="currentStoryIds[num]">
        </Story>
    </div>
</template>


<script>
import Story from './Story.vue'
import axios from 'axios'

export default {
    name: 'list',
    components: {
        Story,
    },
    props: ['orderby'],
    data() {
        return {
            firstStoryNum: 0,
            lastStoryNum: 29,
            currentStoryNums: [],
            currentStoryIds: [],
            allStoryIds: []
        }
    },
    mounted() {
        // get the current story ids (which updates the stories on completion)
        this.fetchStoryIds(this.orderby);
    },
    watch: {
        orderby: function() {
            this.fetchStoryIds(this.orderby);
        }
    },
    methods: {
        fetchStoryIds(orderby) {
            axios.get(`https://hacker-news.firebaseio.com/v0/${orderby}.json`)
                .then(function(response) {
                    // on completion save story ids and update current story ids
                    this.allStoryIds = response.data
                    this.updateCurrentStoryIds()
                }.bind(this))
                .catch(function (error) { console.log(error) })
        },
        // update current story ids
        updateCurrentStoryIds() {
            this.currentStoryNums = []
            this.currentStoryIds = []
            for (var i = this.firstStoryNum; i <= this.lastStoryNum; i++) {
                this.currentStoryNums.push(i)
                this.currentStoryIds.push(this.allStoryIds[i])
            }
        }
    },

}
</script>

<style scoped>

</style>
