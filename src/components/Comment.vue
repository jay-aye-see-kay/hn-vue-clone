<template>
    <div class="card comment">
        <div class="card-body">
            <span class="comment-text" v-html="text"></span>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data() {
        return {
            text: '',
            by: '',
            time: 0,
            kids: [],
            numKids: 0,
            type: '',
        }
    },
    props: ['commentId', 'parentId'],
    mounted() {
        this.getComment()
    },
    watch: {
        commentId: function() {
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
            axios.get(`https://hacker-news.firebaseio.com/v0/item/${this.commentId}.json`)
            .then(function(response) {
                // update comment details
                this.text = response.data.text
                this.by = response.data.by
                this.time =response.data.time
                this.kids = response.data.kids
                this.type = response.data.type

                this.numKids = this.kids.length
            }.bind(this))
            .catch(function (error) { console.log(error) })
        }
    }
}
</script>
