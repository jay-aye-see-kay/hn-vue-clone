<template>
    <div class="card comment">
        <div class="card-body">
            <span class="by">by {{ by }}</span>
            <span class="comment-time">{{ timeSince }} ago</span>
            <br>
            <span class="comment-text" v-html="text"></span>

            <button v-on:click="viewComments = true" class="" type="button" data-toggle="collapse" v-bind:data-target="`#comment-block-${commentId}`" aria-expanded="false" aria-controls="collapseExample">
                View {{ numKids }} replies
            </button>

            <div class="comments collapse" v-bind:id="`comment-block-${commentId}`">
                <Comment
                    v-for="commentId in kids"
                    v-bind:key="commentId"
                    v-bind:commentId="commentId"
                    v-bind:parentId="commentId"
                    v-bind:getReplies="viewComments">
                </Comment>
            </div>

        </div>
    </div>
</template>

<script>
// import Comment from './Comment.vue'
import axios from 'axios'
import { cacheAdapterEnhancer } from 'axios-extensions';

// enable cache for axios (see: https://github.com/kuitos/axios-extensions)
const cachedAxios = axios.create({
	baseURL: '/',
	headers: { 'Cache-Control': 'no-cache' },
	adapter: cacheAdapterEnhancer(axios.defaults.adapter, true)
});

export default {
    name: 'Comment',
    data() {
        return {
            text: '',
            by: '',
            time: 0,
            kids: [],
            numKids: 0,
            type: '',
            viewComments: false,
        }
    },
    props: ['commentId', 'parentId', 'getReplies'],
    // mounted() {
    //     this.getComment()
    // },
    watch: {
        commentId: function() {
            this.getComment()
        },
        getReplies: function() {
            this.getComment()
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
    },
    methods: {
        getComment() {
            cachedAxios.get(`https://hacker-news.firebaseio.com/v0/item/${this.commentId}.json`)
            .then(function(response) {
                // update comment details
                this.text = response.data.text
                this.by = response.data.by
                this.time =response.data.time
                this.type = response.data.type
                
                // only get replies if comment is active
                this.kids = response.data.kids
                this.numKids = this.kids.length
            }.bind(this))
            .catch(function (error) { console.log(error) })
        }
    }
}
</script>
