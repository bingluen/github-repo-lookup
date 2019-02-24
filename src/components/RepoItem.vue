<template>
  <div class="px-2 py-3 col-12 col-md-6 col-lg-3">
    <b-card
    style="min-height: 100%"
    >
      <div slot="header">
        <b-img class="owner-avatar" :src="repo.owner.avatar_url" thumbnail="" />
        <span class="ml-2 font-weight-bold">{{ repo.owner.login }}</span>
      </div>
      <b-card-title>
        <a :href="repo.html_url" target="_blank">{{ repo.full_name }}</a>
      </b-card-title>
      <b-card-text>
        {{ repo.description }}
      </b-card-text>
    </b-card>
  </div>
</template>

<script>
export default {
  name: 'RepoItem',
  props: {
    repo: {
      type: Object,
    },
  },
};
</script>

<style lang="stylus">
.owner-avatar
  max-width: 50px !important
.card-header
  padding .375rem !important
</style>
