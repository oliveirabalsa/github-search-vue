<template>
  <q-card class="user__card">
    <q-card-section>
      <div class="row no-wrap items-center">
        <div class="col text-h6 ellipsis">
          {{ repositoryName }}
        </div>
        <div
          class="col-auto text-grey text-caption q-pt-md row no-wrap items-center"
        >
          <q-icon name="person" />
          {{ owner }}
        </div>
      </div>

      <q-rating v-model="stars" size="26px" />
      {{ stars }}
    </q-card-section>
    <q-separator />
  </q-card>
</template>

<script>
export default {
  name: 'UserCard',
  data() {
    return {};
  },
  props: {
    repositoryName: {
      type: String,
      default: '',
      required: true,
    },

    stars: {
      type: Number,
      default: 0,
      required: true,
    },

    owner: {
      type: String,
      default: '',
    },
  },
};
</script>

<style lang="scss">
.user__card {
  width: 60%;
  margin-top: 5px;
  max-width: 526px;
}

@media (max-width: 430px) {
  .user__card {
    width: 100%;
  }
}
</style>
