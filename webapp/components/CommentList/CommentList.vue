<template>
  <div id="comments">
    <h3 style="margin-top: -10px;">
      <counter-icon icon="comments" :count="post.comments.length">
        {{ $t('common.comment', null, 0) }}
      </counter-icon>
    </h3>
    <ds-space margin-bottom="large" />
    <div v-if="post.comments && post.comments.length" id="comments" class="comments">
      <comment
        v-for="comment in post.comments"
        :key="comment.id"
        :comment="comment"
        :post="post"
        :routeHash="routeHash"
        @deleteComment="updateCommentList"
        @updateComment="updateCommentList"
        @toggleNewCommentForm="toggleNewCommentForm"
      />
    </div>
  </div>
</template>
<script>
import CounterIcon from '~/components/_new/generic/CounterIcon/CounterIcon'
import Comment from '~/components/Comment/Comment'
import scrollToAnchor from '~/mixins/scrollToAnchor'

export default {
  mixins: [scrollToAnchor],
  components: {
    CounterIcon,
    Comment,
  },
  props: {
    routeHash: { type: String, default: () => '' },
    post: { type: Object, default: () => {} },
  },
  methods: {
    checkAnchor(anchor) {
      return anchor === '#comments'
    },
    updateCommentList(updatedComment) {
      this.post.comments = this.post.comments.map(comment => {
        return comment.id === updatedComment.id ? updatedComment : comment
      })
    },
    toggleNewCommentForm(showNewCommentForm) {
      this.$emit('toggleNewCommentForm', showNewCommentForm)
    },
  },
}
</script>
