<template>
  <Layout>
    <TopSection />





    <div class="overflow-x-hidden">
      <div
        class="projects container-inner mx-auto text-xl border-t border-gray-500 border-b py-16 mb-8 relative"
      >
        <h2 class="font-bold mb-6" id="projects">Recent Blog Posts:</h2>

        <div class="absolute right-0" style="top: 50px; transform: translate(100%) rotate(180deg)">
          <svg width="170px" height="170px">
            <use xlink:href="#dots-triangle" />
          </svg>
        </div>

        <ul class="text-lg sm:text-xl">
          <li v-for="post in $page.posts.edges" :key="post.id" class="checkmark mb-6">
            <div>
              {{ post.node.section }} -
              <g-link :to="post.node.path" class="text-copy-primary">{{ post.node.title }}</g-link>
            </div>
            <div class="text-lg text-gray-600">{{ post.node.summary }}</div>
          </li>
        </ul>
      </div>
      <!-- end projects -->
    </div>

    
    <div class="overflow-x-hidden border-gray-200 border-b">
      <div class="get-to-know-me container-inner mx-auto text-xl pb-16 relative">
        <h2 class="font-bold mb-6" id="about">Get to know me:</h2>

        <div class="absolute left-0" style="top: 50px; transform: translateX(-100%)">
          <svg width="170px" height="170px">
            <use xlink:href="#dots-triangle" />
          </svg>
        </div>

        <div class="flex flex-col sm:flex-row justify-between items-center mb-16">
          <div>
            <g-image
              src="../../static/profile.jpg"
              alt="avatar"
              class="w-32 h-32 rounded-full mb-8 lg:mb-0"
            />
          </div>
          <div
            class="flex-1 text-lg sm:text-xl ml-6"
          >I am a freelance web developer, photographer, content creator and writer, who is on a mission to help businesses and individuals build solutions for their online world.</div>
        </div>
      </div>
      <!-- end get-to-know me -->
    </div>

    <div class="overflow-x-hidden">
      <div class="contact-me bg-background-secondary pt-16">
        <div class="container-inner mx-auto text-xl pb-4 relative">
          <h2 class="font-bold mb-6" id="contact">Contact me:</h2>

          <div class="absolute right-0 top-0" style="transform: translate(100%) rotate(180deg)">
            <svg width="170px" height="170px">
              <use xlink:href="#dots-triangle" />
            </svg>
          </div>

          <div class="text-lg sm:text-lg mb-16">
            <form
              name="contact"
              class="mb-12"
              method="post"
              action="/success/"
              v-on:submit.prevent="handleSubmit"
              data-netlify="true"
              data-netlify-honeypot="bot-field"
            >
              <input type="hidden" name="form-name" value="contact" />
              <p hidden>
                <label>
                  Don’t fill this out:
                  <input name="bot-field" />
                </label>
              </p>
              <div class="flex flex-wrap mb-6 -mx-4">
                <div class="w-full md:w-1/2 mb-6 md:mb-0 px-4">
                  <label class="block mb-2 text-copy-primary" for="name">Name</label>

                  <input
                    type="text"
                    name="name"
                    id="name"
                    placeholder="Joe Bloggs"
                    class="block w-full bg-background-form border border-border-color-primary shadow rounded outline-none focus:border-green-700 mb-2 p-4"
                    v-model="formData.name"
                    required
                  />
                </div>

                <div class="w-full px-4 md:w-1/2">
                  <label class="block text-copy-primary mb-2" for="email">Email Address</label>

                  <input
                    type="email"
                    name="email"
                    id="email"
                    placeholder="email@example.com"
                    class="block w-full bg-background-form border border-border-color-primary shadow rounded outline-none focus:border-green-700 mb-2 p-4"
                    v-model="formData.email"
                    required
                  />
                </div>
              </div>

              <div class="w-full mb-12">
                <label class="block text-copy-primary mb-2" for="message">Message</label>

                <textarea
                  id="message"
                  rows="5"
                  name="message"
                  class="block w-full bg-background-form border border-border-color-primary shadow rounded outline-none appearance-none focus:border-green-700 mb-2 px-4 py-4"
                  placeholder="Enter your message here."
                  v-model="formData.message"
                  required
                ></textarea>
              </div>

              <div class="flex justify-end w-full">
                <button
                  type="submit"
                  value="submit"
                  class="block bg-green-700 hover:bg-green-800 text-white text-sm font-semibold tracking-wide uppercase shadow rounded cursor-pointer px-6 py-3"
                >Submit</button>
              </div>
            </form>
          </div>
        </div>
      </div>
      <!-- end contact-me -->
    </div>
  </Layout>
</template>

<page-query>
query HomePosts {
  metadata {
    sanityOptions {
      projectId
      dataset
    }
  }
  photos: allSanityPost (sortBy: "publishedAt", order: DESC, limit: 6) {
    totalCount
    pageInfo {
      totalPages
      currentPage
    }
    edges {
      node {
        id
        title
        publishedAt (format: "MMMM D, Y")      
        _rawExcerpt
        mainImage {
          asset {
            _id
            url
          }
          caption
          alt
          hotspot {
            x
            y
            height
            width
          }
          crop {
            top
            bottom
            left
            right
          }
        }  
        slug {
          current
        }
      }
    }
  }
  posts: allPost (limit: 4, sortBy: "date", order: DESC, filter: { published: { eq: true }}) {
    edges {
      node {
        id
        title
        published
        section
        date (format: "MMMM D, Y")
        summary
        
        path
      }
    }
  }
}
</page-query>

<script>
import Vue from 'vue'
import TopSection from '../components/Home/TopSection'
import VueMasonry from 'vue-masonry-css'

Vue.use(VueMasonry)

export default {
  components: {
    TopSection,
    VueMasonry
  },
  data() {
    return {
      formData: {}
    }
  },
  methods: {
    encode(data) {
      return Object.keys(data)
        .map(key => encodeURIComponent(key) + '=' + encodeURIComponent(data[key]))
        .join('&')
    },
    handleSubmit(e) {
      fetch('/', {
        method: 'POST',
        headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
        body: this.encode({
          'form-name': e.target.getAttribute('name'),
          ...this.formData
        })
      })
        .then(() => this.$router.push('/success'))
        .catch(error => alert(error))
    }
  },
  metaInfo: {
    title: 'HeadForCode'
  }
}
</script>

