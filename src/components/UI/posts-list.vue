<template>
  <div>
    <div v-if="showLoader" class="d-flex align-center">
      <v-progress-circular
        v-if="showLoader"
        class="mx-auto"
        indeterminate
        color="green"
      ></v-progress-circular>
    </div>
    <div v-else-if="posts.length == 0" class="d-flex align-center">
      <span class="mx-auto" v-if="type == 'main'">There are no posts yet</span>
       <span class="mx-auto" v-if="type == 'profile'">You don't have any posts yet</span>
    </div>
    <v-row v-else style="justify: space-between">
      <v-col cols="6" lg="3" v-for="post in posts" :key="post._id">
        <v-card evalution="10" @click="getDetailPost(post._id)">
          <v-card-title class="title">{{ post.title }}</v-card-title>
          <v-card-text
            ><span class="text">{{ post.text }}</span>
          </v-card-text>

          <v-card-subtitle class="px-5">
            <v-row justify="end">
              <span>
                {{ post.date_of_create }}
              </span>
            </v-row>
            <v-row justify="start"
              ><span> Author: {{ post.author_username }} </span>
            </v-row>
          </v-card-subtitle>
        </v-card>
      </v-col>
      <post-detail-modal
        :post="postDetail"
        :visible="visibleMainModal"
        @close="visibleMainModal = !visibleMainModal"
      />
      <add-new-post-modal
        isEdit="true"
        :post="postDetail"
        :visible="visibleProfileModal"
        @close="visibleProfileModal = !visibleProfileModal"
        @getPosts="$emit('getPosts', (visibleProfileModal = false))"
      />
    </v-row>
  </div>
</template>

<script>
import postDetailModal from "@/components/Main/post-detail-modal";
import addNewPostModal from "@/components/Profile/add-new-post-modal";
import postsService from "@/request/requests/postsService";

export default {
  components: {
    postDetailModal,
    addNewPostModal,
  },
  data: () => ({
    visibleMainModal: false,
    visibleProfileModal: false,
    postDetail: {},
  }),
  props: {
    posts: {
      require: true,
    },
    type: {
      require: true,
    },
    showLoader: {
      require: true,
    },
  },
  methods: {
    async getDetailPost(id) {
      const response = await postsService.getPostById(id);
      this.postDetail = response.result;
      console.log(this.postDetail);
      if (this.type === "main") {
        this.visibleMainModal = true;
      }
      if (this.type === "profile") {
        this.visibleProfileModal = true;
      }
    },
  },
};
</script>

<style>
.text {
  width: 100%;
  overflow: hidden;
  line-height: 2rem;
  max-height: 4rem;
  -webkit-box-orient: vertical;
  display: block;
  display: -webkit-box;
  overflow: hidden !important;
  text-overflow: ellipsis;
  -webkit-line-clamp: 2;
}
.title {
  width: 100%;
  overflow: hidden;
  line-height: 3rem;
  max-height: 3rem;
  -webkit-box-orient: vertical;
  display: block;
  display: -webkit-box;
  overflow: hidden !important;
  text-overflow: ellipsis;
  -webkit-line-clamp: 2;
}
</style>