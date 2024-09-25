<template>
    <div class="commenter__comment py-4 border-t border-40">
        <div class="font-light text-80 text-sm">
            <template v-if="hasCommenter">
                <a class="link-default" :href="commenterUrl" v-text="commenter"></a>

                said
            </template>

            <template v-else>
                Written
            </template>

            {{ date }}
        </div>

        <div class="mt-2" v-html="commentString"></div>
    </div>
</template>

<script>

import find from 'lodash.find';

export default {
    props: {
        comment: {
            type: Object,
            required: true
        }
    },

    computed: {
        commentString() {
            return find(this.comment.fields, { attribute: 'comment' }).value;
        },

        commenter() {
            return find(this.comment.fields, { attribute: 'commenter' }).value;
        },

        commenterUrl() {
            let commenterId = find(this.comment.fields, { attribute: 'commenter' }).belongsToId;

            return `${Nova.config("base")}/resources/users/${commenterId}`;
        },

        date() {
            let date = find(this.comment.fields, { attribute: 'created_at' });

            return `on ${date.value}`;
        },

        hasCommenter() {
            return Boolean(this.commenter);
        }
    }
}
</script>
