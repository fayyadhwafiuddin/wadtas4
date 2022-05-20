<script setup>
import HelloWorld from "./components/HelloWorld.vue";
import TheWelcome from "./components/TheWelcome.vue";
</script>

<template>
  <div id="app">
      <table class="pt-8 border-collapse w-128 h-48 m-auto rounded-2xl m-auto mt-32 text-sm text-left text-gray-500 bg-gray-200">
        <tr class="text-xs text-gray-700">
        <th class="font-medium p-6">
          Name
        </th>
        <th class="font-medium p-6">
          Title
        </th>
        <th class="font-medium p-6">
          User ID
        </th>
        <th class="font-medium p-6">
          ID
        </th>
        </tr>
        
        <tr v-for="bloguser in blogusers" :key="bloguser.id" class="bg-zinc-100 p-6">
          <td class="bg-white-400 p-6">
            {{ bloguser.username }}
          </td>
          <td class="text-blue-400 underline p-6">
            {{ bloguser.title }}
          </td>
          <td class="p-6">
            {{ bloguser.studentid }}
          </td>
          <td class="p-6 w-24">
            {{ bloguser.pid }}
          </td>
        </tr>

        <tr>
          <td class="h-8"></td>
        </tr>
      </table>

      <form id="form" v-on:submit="handleSubmit">
      <table class="pt-8 border-collapse w-128 h-48 m-auto rounded-2xl m-auto mt-32 text-sm text-left text-black bg-gray-300">
        <tr>
          <th class="text-lg p-6 font-bold">Post</th>
        </tr>

        <tr>
        <th class="font-medium p-6">
          Title
        </th>
        <td>
          <input class="border border-black" id="title" name="title" v-model="modifiedData.title" type="text">
        </td>
        </tr>
        <tr>
        <th class="font-medium p-6">
          content
        </th>
        <td>
          <textarea class="border border-black" id="content" name="content" v-model="modifiedData.content" cols="30" rows="10"></textarea>
        </td>
        </tr>
        <tr>
          <td></td>
          <td><input class="border border-black rounded-lg w-24" type="submit" value="Submit"></td>
        </tr>
        <tr class="h-8">

        </tr>
      </table>
      </form>
  </div>
</template>

<script>
posts: [];
blogusers: [];
export default {
  name: "App",
  data() {
    return {
        modifiedData: {
        id: '',
        title: null,
        content: null,
        userId:'',
        pid: '', 
      },
      posts: [],
      blogusers: [],
    };
  },
  mounted() {
    fetch("http://localhost:8082/api/blogusers", {
      method: "GET",
    })
      .then((response) => response.json())
      .then((data) => {
        var data1 = [];
        var pushdata1 = [];
        data1 = data.data;
        data1.forEach((element) => {
          pushdata1.push(element.attributes);
        });
        this.blogusers = pushdata1;
      });
    fetch("http://localhost:8082/api/posts", {
      method: "GET",
    })
      .then((response) => response.json())
      .then((data) => {
        var data2 = [];
        var pushdata2 = [];
        data2 = data.data;
        data2.forEach((element) => {
          pushdata2.push(element.attributes);
        });

        this.posts = pushdata2;
        this.blogusers.forEach((array1) => {
          this.posts.forEach((array2) => {
            if (array1.studentid == array2.userId) {
              array1.title = array2.title;
              array1.pid = array2.pid;
            }
          });
        });
      });
},

handleSubmit: async function(e) {
      e.preventDefault();

      try {
        const response = await fetch('http://localhost:1337/api/posts', {
            method: 'POST',
            headers: {'Authorization': 'Bearer 765946dbf095b85f788eeb09c42f8eac310db3c5a8df28866e113d658623666787f631adf8b5a1a6a8b36a94692522b51948fe090c4902a5ce3bf6b27ae06ec757fa3d96170113a3b5eeb685a94d2d12610c35dcbfa6870f62b4fafe71397cbd0ef39472bcf03576f27b34b749b2092c599396bad0d7b6dd9640882a4bb575f1'},
            body: JSON.stringify(this.modifiedData)
          }).then(this.checkStatus)
            .then(this.parseJSON);
            console.log(response);
      } catch (error) {
        this.error = error
      }
    }
}
</script>

<style>
@import "./assets/base.css";

/* #app {
  max-width: 1280px;
  font-weight: normal;
}

header {
  line-height: 1.5;
}


a,
.green {
  text-decoration: none;
  color: hsla(160, 100%, 37%, 1);
  transition: 0.4s;
}

@media (hover: hover) {
  a:hover {
    background-color: hsla(160, 100%, 37%, 0.2);
  }
}

@media (min-width: 1024px) {

  #app {
    display: grid;
    grid-template-columns: 1fr 1fr;
    padding: 0 2rem;
  }

  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  .logo {
    margin: 0 2rem 0 0;
  }
} */
</style>
