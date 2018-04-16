<template>

    <article class="media">
        <div class="media-content">
            <div class="content">
                <p>
                    <strong><a target="_blank" v-bind:href="url">{{ title }}</a></strong> <small>@{{ by }}</small> <small class="is-pulled-right">{{ readableDate }}</small>
                    <br>
                    Comments: {{ numKids }}({{ descendants }}) <span class="is-pulled-right">Score: {{ score }}</span>
                </p>
            </div>
        </div>
    </article>

</template>


<script>
import axios from 'axios'

export default {
    data() {
        return {
            title: '',
            url: '',
            by: '',
            time: 0,
            descendants: 0,
            kids: [],
            numKids: 0,
            score: 0,
            type: '',
        }
    },
    props: ['storynum', 'storyid'],
    mounted() {
        this.getStory()
    },
    updated() {
        this.getStory()
    },
    watch: {
        storyid: function(newVal, oldVal) {
            console.log('Prop changed: ', newVal, ' | was: ', oldVal)
            this.getStory()
        }
    },
    computed: {
        readableDate() {
            var d = new Date(this.time * 1000)
            var months = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
            var year = d.getFullYear();
            var month = months[d.getMonth()];
            var date = d.getDate();
            var hour = d.getHours();
            var min = d.getMinutes();
            var sec = d.getSeconds();
            var time = date + ' ' + month + ' ' + year + ' ' + hour + ':' + min + ':' + sec ;
            return `${date} ${month} ${year} - ${hour}:${min}:${sec}`
        }
    },
    methods: {
        getStory() {
            axios.get(`https://hacker-news.firebaseio.com/v0/item/${this.storyid}.json`)
            .then(function(response) {
                // update story details
                this.title = response.data.title
                this.url = response.data.url
                this.by = response.data.by
                this.time =response.data.time
                this.descendants =response.data.descendants
                this.kids = response.data.kids
                this.score =response.data.score
                this.type = response.data.type

                this.numKids = this.kids.length
            }.bind(this))
            .catch(function (error) { console.log(error) })
        }
    }
}
</script>

<style scoped>

</style>
