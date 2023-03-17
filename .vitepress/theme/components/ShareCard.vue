<template>
  <div class="shareCard">
    <h1 class="title">
      <span>Alex Darby's Stuff</span>
    </h1>
    <p class="description">
      Here is a selfie I took in a lift because it had weird lighting and made me look serious. In fact, a member of my team was once trapped in this lift for 40 minutes. The reception staff refused to help us get him out and advised we call the Fire Brigade. I'm glad we don't work in that office anymore.

Originally from Cornwall, living and working in Cardiff. Also spent some time living in Sheffield and Bristol.
    </p>
  </div>
</template>

<script lang="ts" setup></script>
<style scoped>
.shareCard {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 0 15px;
  text-align: center;
}
.title {
  display: flex;
  align-items: center;
  font-size: 2.2em;
  font-weight: bold;
}
.description {
  margin: 1em 0;
  width: 60%;
  text-align: left;
}
@media (max-width: 768px) {
  .description {
    width: 90%;
  }
}
</style>
