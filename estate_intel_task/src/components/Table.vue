<template>
  <div class="tableComponent">
    <div class="heading">
      <p>15 Projects</p>
      <div>
        <span @click="prevData" v-if="i > 1"
          ><i class="fa fa-angle-left"></i
        ></span>
        <span>{{ from }} - {{ to }} of 45</span>
        <span @click="nextData" v-if="posts.length !== 0 && (i <= 1 || i < 3)">
          <i class="fa fa-angle-right"></i>
        </span>
      </div>
      <p>
        Dispaly Map
        <label class="switch">
          <input type="checkbox" />
          <span class="slider round"></span>
        </label>
      </p>
    </div>
    <div class="table">
      <table>
        <colgroup>
          <col span="1" style="width: 14%;" />
          <col span="1" style="width: 12%;" />
          <col span="1" style="width: 12%;" />
          <col span="1" style="width: 12%;" />
          <col span="1" style="width: 12%;" />
          <col span="1" style="width: 12%;" />
          <col span="1" style="width: 12%;" />
          <col span="1" style="width: 12%;" />
        </colgroup>
        <tr>
          <th>Project Name</th>
          <th>Developer</th>
          <th>Main Contractor</th>
          <th>Area</th>
          <th>State</th>
          <th>Status</th>
          <th>Sector</th>
        </tr>
        <tr v-for="(post, index) of posts" :key="index">
          <td>{{ post.project_name }}</td>
          <td>{{ post.developer }}</td>
          <td>{{ post.main_contractor }}</td>
          <td>{{ post.address }}</td>
          <td>{{ post.state }}</td>

          <td>
            <span>{{ post.status }}</span>
          </td>

          <td>{{ post.sector }}</td>
        </tr>
      </table>

      <p v-if="posts.length === 0" class="error">{{ errorMessage }}</p>
      <div v-if="posts.length === 0" class="loader"></div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { URL } from "../secret.js";
export default {
  data() {
    return {
      posts: [],
      i: 1,
      errorMessage: "",
      from: 0,
      to: 15,
    };
  },
  methods: {
    async prevData() {
      this.i--;
      this.from -= 15;
      this.to -= 15;
      this.getPost();
    },

    async nextData() {
      this.i++;
      this.from += 15;
      this.to += 15;
      this.getPost();
    },

    async getPost() {
      try {
        const res = (await axios.get(`${URL}/?page=${this.i}`)).data;
        this.posts = res.data;
      } catch (error) {
        setTimeout(() => (this.errorMessage = `${error.message}`), 5000);
      }
    },
  },
  created() {
    this.getPost();
  },
};
</script>

<style scoped lang="scss">
@import "../../public/styles/app";
</style>
