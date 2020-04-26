<template>
  <div v-editable="blok" class="_postid">
    <h1>{{ title }}</h1>
    <p> {{ content }}</p>
  </div>
</template>

<script>
export default {
  props: {
  },
  asyncData (context) {
    console.log(context)
    return context.app.$storyapi.get('cdn/stories/post/' + context.params.postid, {
      version: context.isDev ? 'draft' : 'published'
    }).then((res) => {
      console.log('res---->', res)
      return {
        blok: res.data.story.content,
        title: res.data.story.content.title,
        content: res.data.story.content.content
      }
    })
  },
  mounted () {
    this.$storybridge.on(['input', 'published', 'change'], (event) => {
      if (event.action === 'input') {
        if (event.story.id === this.story.id) {
          this.story.content = event.story.content
        }
      } else {
        location.reload(true)
      }
    })
  }
}
</script>
<style scoped>
  ._postid p {
    white-space: pre-line;
  }

</style>
