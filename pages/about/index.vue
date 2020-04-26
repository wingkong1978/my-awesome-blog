<template>
  <section id="about-page">
    <h1>{{ title }}</h1>
    <p>{{ content }}</p>
  </section>
</template>

<script>
export default {
  props: {
  },
  asyncData (context) {
    return context.app.$storyapi.get('cdn/stories/about', {
      version: context.isDev ? 'draft' : 'published'
    }).then((res) => {
      console.log('res-->', res)
      return {
        title: res.data.story.content.Title,
        content: res.data.story.content.Content
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
        window.location.reload(true)
      }
    })
  }
}
</script>
