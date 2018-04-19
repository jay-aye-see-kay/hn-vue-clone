<template>

    <article class="card">
        <div class="card-body">
            <div class="story-title">
                <strong><a target="_blank" v-bind:href="url">{{ title }}</a></strong> ({{ extractedHostname }})
            </div>
            <div class="story-details">
                <span class="score">{{ score }} {{ score==1 ? 'point' : 'points' }} by</span>
                <span class="by">{{ by }}</span>
                <span class="time-since">{{ timeSince }} ago</span>
            </div>
        
            <button v-on:click="viewComments = true" class="" type="button" data-toggle="collapse" v-bind:data-target="`#comment-block-${storyid}`" aria-expanded="false" aria-controls="collapseExample">
                View all {{ numKids }} comments ({{ descendants }} total replies)
            </button>

            <div class="comments collapse" v-bind:id="`comment-block-${storyid}`">
                <Comment
                    v-for="commentId in kids"
                    v-bind:key="commentId"
                    v-bind:commentId="commentId"
                    v-bind:parentId="storyid"
                    v-bind:getReplies="viewComments">
                </Comment>
            </div>
            
        </div>
    </article>

</template>


<script>
import Comment from './Comment.vue'
import axios from 'axios'
import { cacheAdapterEnhancer } from 'axios-extensions';

// enable cache for axios (see: https://github.com/kuitos/axios-extensions)
const cachedAxios = axios.create({
	baseURL: '/',
	headers: { 'Cache-Control': 'no-cache' },
	adapter: cacheAdapterEnhancer(axios.defaults.adapter, true)
});

export default {
    name: 'story',
    components: {
        Comment
    },
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
            viewComments: false,
        }
    },
    props: ['storynum', 'storyid'],
    mounted() {
        this.getStory()
    },
    watch: {
        storyid: function() {
            this.getStory()
        }
    },
    computed: {
        timeSince() {
            var storyTime = new Date(this.time * 1000)
            var seconds = Math.floor((new Date() - storyTime) / 1000);
            var interval = Math.floor(seconds / 31536000);
            if (interval > 1) {
                return interval + " years";
            }
            interval = Math.floor(seconds / 2592000);
            if (interval > 1) {
                return interval + " months";
            }
            interval = Math.floor(seconds / 86400);
            if (interval > 1) {
                return interval + " days";
            }
            interval = Math.floor(seconds / 3600);
            if (interval > 1) {
                return interval + " hours";
            }
            interval = Math.floor(seconds / 60);
            if (interval > 1) {
                return interval + " minutes";
            }
            return Math.floor(seconds) + " seconds";
        },
        extractedHostname() {
            // only calculate hostname if url != null
            if (!this.url) { return null }
            var hostname;
            var url = this.url
            //find & remove protocol (http, ftp, etc.) and get hostname
            if (url.indexOf("://") > -1) {
                hostname = url.split('/')[2];
            }
            else {
                hostname = url.split('/')[0];
            }
            //find & remove port number
            hostname = hostname.split(':')[0];
            //find & remove "?"
            hostname = hostname.split('?')[0];
            return hostname;
        }
    },
    methods: {
        getStory() {
            cachedAxios.get(`https://hacker-news.firebaseio.com/v0/item/${this.storyid}.json`)
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
