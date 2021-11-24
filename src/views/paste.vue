
  <div class="post-show">
    <h2>{{ post.blurb }}</h2>
    <p>{{ post.blog }}</p>
    <span v-if="post.image_url"
      ><img v-bind:src="post.image_url" v-bind:alt="post.image_url"
    /></span>
    <p>{{ post.user }}</p>
    <div v-if="$parent.getUserId() == post.user_id">
      <router-link :to="`/posts/${post.id}/edit`">Edit</router-link>
    </div>
    <form v-on:submit.prevent="createNewComment()">
      <div>
        <label>Body</label> <br />
        <input type="text" v-model="newCommentParams.body" />
      </div>
      <div>
        <label>Image</label> <br />
        <input type="text" v-model="newCommentParams.image_url" />
      </div>
      <button>Post Comment!</button>
    </form>
    <p>{{ newCommentParams }}</p>
    <h3>Comments</h3>

    <div v-for="comment in post.comments" v-bind:key="comment.id">
      <div v-if="$parent.getUserId() == comment.user.id">
        <form v-on:submit.prevent="updateComment(comment)">
          <h1>Update Comment</h1>
          <ul>
            <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
          </ul>
          <div>
            <label>Body:</label>
            <input type="text" v-model="comment.body" />
          </div>
          <div>
            <label>Image:</label>
            <input type="text" v-model="comment.image_url" />
          </div>
          {{ comment.id }}

          {{ comment.user.id }}
          <input type="submit" class="btn btn-primary" value="Submit" />
          <button v-on:click="destroyComment(comment)">Delete Comment</button>
        </form>
      </div>
      User: {{ comment.user.name }} <br />
      <img v-bind:src="comment.user.image_url" v-bind:alt="comment.user" />
      Comment: {{ comment.body }} {{ comment.image_url }}
    </div>
  </div>





<template>
  

<div class="board-show">
    <h1>{{ board.name }}</h1>
    <div v-for="post_board in board.post_boards" v-bind:key="post_board.id">
      <br />

      <p>{{ post_board.post.blog }}</p>
      <br />
      <p>{{ post_board.post.blurb }}</p>
      <img
        v-bind:src="post_board.post.image_url"
        v-bind:alt="post_board.post.image_url"
      />
      <br />
      <button v-on:click="unsavePost(post_board)">Unsave Post</button>
    </div>
    <form v-on:submit.prevent="updateBoard()">
      <h1>Update Board</h1>
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <div>
        <label>Name:</label>
        <input type="text" v-model="board.name" />
      </div>
      <input type="submit" value="Update" />
    </form>

    <button v-on:click="destroyBoard()">Delete Board</button>
  </div>
  </template>




   <form v-on:submit.prevent="updateBoard()">
          <h1>Update Board</h1>
          <ul>
            <li v-for="error in errors" v-bind:key="error">
              {{ error }}
            </li>
          </ul>
          <div>
            <label>Name:</label>
            <input type="text" v-model="board.name" />
          </div>
          <div class="">
            <input type="submit" value="Update" />
          </div>
          <div class="">
            <button v-on:click="destroyBoard()">Delete Board</button>
          </div>
        </form>