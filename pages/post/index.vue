<template>
  <section id="post">
    <v-card
      class="mx-auto"
      max-width="500"
    >
      <v-card-text>
        <v-container fluid>
          <v-row dense>
            <v-col
              v-for="blog in blogs"
              :key="blog.id"
              cols="12"
            >
              <v-card>
                <nuxt-link
                  class="postlink"
                  :to="post/blog.id"
                >
                  <v-img
                    :src="blog.thumbnail"
                    class="white--text align-end"
                    gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)"
                    height="200px"
                  >
                    <!--      <nuxt-link>-->
                    <v-card-title>{{ blog.title }}</v-card-title>
                    <!--                <v-card-text>{{ blog.content }}</v-card-text>-->
                    <v-card-actions>
                      <v-spacer />

                      <v-btn icon>
                        <v-icon>mdi-heart</v-icon>
                      </v-btn>

                      <v-btn icon>
                        <v-icon>mdi-bookmark</v-icon>
                      </v-btn>

                      <v-btn icon>
                        <v-icon>mdi-share-variant</v-icon>
                      </v-btn>
                    </v-card-actions>
                  </v-img>
                </nuxt-link>
              </v-card>
            </v-col>
          </v-row>

        <!--      </nuxt-link>-->
        </v-container>
      </v-card-text>
      <v-card-actions>
        <v-dialog
          v-model="dialog"
          width="500"
        >
          <template v-slot:activator="{ on }">
            <v-btn
              absolute
              dark
              fab
              right
              bottom
              color="primary"
              v-on="on"
            >
              <v-icon>mdi-plus</v-icon>
            </v-btn>
          </template>

          <v-card>
            <v-card-title
              class="headline grey lighten-2"
              primary-title
            >
              New Post
            </v-card-title>

            <v-card-text>
              <v-form>
                <v-text-field
                  v-model="form.title"
                  label="Title"
                />
                <v-textarea
                  v-model="form.content"
                  label="Content"
                />
                <v-file-input
                  v-model="form.thumbnail"
                  label="Thumbnail"
                  prepend-icon="mdi-image"
                  accept="image/png, image/jpeg, image/bmp"
                />
              </v-form>
            </v-card-text>

            <v-divider />

            <v-card-actions>
              <v-spacer />
              <v-btn
                color="primary"
                @click="savePost"
              >
                Save
              </v-btn>
              <v-btn
                color="primary"
                text
                @click="dialog = false"
              >
                Cancel
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-card-actions>
    </v-card>
  </section>
</template>

<script>
export default {
  props: {
  },
  asyncData (context) {
    return context.app.$storyapi.get('cdn/stories', {
      version: context.isDev ? 'draft' : 'published',
      starts_with: 'post/'
    }).then((res) => {
      console.log('res-->', res)
      return {
        blogs: res.data.stories.map((n) => {
          return {
            title: n.content.title,
            content: n.content.content,
            id: n.name,
            thumbnail: n.content.thumbnail
          }
        })
      }
    })
  },
  data: () => {
    return {
      dialog: false,
      form: {
        title: '',
        content: '',
        thumbnail: {}
      }
    }
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
  },
  methods: {
    savePost () {
      console.log('form params -===> ', this.form)
    }
  }
}
</script>
<style scoped>
  .postlink {
    text-decoration: none;
  }

</style>
