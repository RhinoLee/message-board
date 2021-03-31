<template>
  <div>
    <button @click="showMessageForm = !showMessageForm"
    type="button"
    class="btn btn-info show-message">
    Show Message Form
    </button>
    <form v-if="showMessageForm" @submit.prevent="addMessage">
      <div class="form-group">
        <label for="username">User Name</label>
        <input
          v-model="message.username"
          type="text"
          class="form-control"
          id="username"
          required
        />
      </div>
      <div class="form-group">
        <label for="subject">Subject</label>
        <input
          v-model="message.subject"
          type="text"
          class="form-control"
          id="subject"
          placeholder="Enter a subject"
          required
        />
      </div>
      <div class="form-group">
        <label for="message">Message</label>
        <textarea
        v-model="message.message"
        class="form-control"
        id="message"
        placeholder="Enter a message"
        required>
        </textarea>
      </div>
      <div class="form-group">
        <label for="imageURL">ImageURL</label>
        <input
          v-model="message.imageURL"
          type="url"
          class="form-control"
          id="imageURL"
          placeholder="Put an imageURL"
        />
      </div>
      <button type="submit" class="btn btn-primary">Add message</button>
    </form>
    <ul class="list-unstyled">
      <li class="media" v-for="msg in messages" :key="msg._id">
        <img v-if="msg.imageURL" class="mr-3" :src="msg.imageURL" :alt="msg.subject" />
        <div class="media-body">
          <h4 class="mt-0 mb-1">{{ msg.username }}</h4>
          <h5 class="mt-0 mb-1">{{ msg.subject }}</h5>
          <p>{{ msg.message }}</p>
          <small>{{ msg.created }}</small>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
const API_URL = 'http://localhost:1234/messages';

export default {
  data: () => ({
    showMessageForm: false,
    messages: null,
    message: {
      username: 'Anonymous',
      subject: '',
      message: '',
      imageURL: '',
    },
  }),
  mounted() {
    fetch(API_URL)
      .then((res) => res.json())
      .then((result) => {
        this.messages = result.reverse();
      });
  },
  methods: {
    addMessage() {
      fetch(API_URL, {
        method: 'POST',
        body: JSON.stringify(this.message),
        headers: {
          'content-type': 'application/json',
        },
      }).then((res) => res.json()).then((result) => {
        this.messages.unshift(result);
      });
    },
  },
};
</script>

<style lang="scss">
  li {
    padding: 20px 0;
    & + li {
      border-top: 1px solid #fff;
    }
  }
  img {
    max-width: 300px;
    height: auto;
  }
  .btn.show-message {
    margin: 30px 0;
  }
</style>
