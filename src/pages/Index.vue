<template>
  <q-page class="flex flex-center column">
    <div class="p-5 container__search">
      <div v-if="!repositories.length" class="input__container">
        <img class="github__image" src="../assets/github.png" />
        <h5>Pesquise repositórios digitando o nome de usuário no GitHub.</h5>
        <q-form @submit="getReposByName(text)">
          <q-input
            outlined
            v-model="text"
            label="Digite o nome do usuário"
            :rules="[val => !!val || 'Por favor, digite um nome de usuário']"
          />
          <q-btn
            type="submit"
            class="btn__style"
            color="secondary"
            icon-right="send"
            label="Buscar"
          />
        </q-form>
      </div>
      <q-btn
        v-if="repositories.length"
        type="reset"
        class="btn__style"
        color="negative"
        icon-right="delete"
        label="Limpar"
        @click="cleanRepositories"
      />
    </div>
    <user-card
      v-for="repository of repositories"
      :key="repository.id"
      :repositoryName="repository.name"
      :owner="repository.owner.login"
      :stars="repository.stargazers_count"
    />
  </q-page>
</template>

<script>
import UserCard from 'src/components/UserCard.vue';
import { http } from '../services/api';

export default {
  name: 'PageIndex',
  components: {
    UserCard,
  },
  data() {
    return {
      text: '',
      repositories: [],
    };
  },
  methods: {
    async getReposByName(username) {
      this.$q.loading.show();
      try {
        this.repositories = [];
        const response = await http.get(`users/${username}/repos`);
        this.$q.loading.hide();
        this.repositories = response.data;
      } catch (error) {
        this.$q.loading.hide();
        this.$q.notify({
          type: 'negative',
          message: error.message.includes('404')
            ? 'Usuário não encontrado'
            : error.message,
        });
      }
    },
    cleanRepositories() {
      this.repositories = [];
      this.text = '';
    },
  },
};
</script>
<style lang="scss">
h5 {
  text-align: center;
}
.container__search {
  padding: 50px;
  border: 2px solid rgb(185, 161, 161);
  border-radius: 5px;
  width: 60%;
  max-width: 526px;
}

.github__image {
  width: 80%;
  margin: 0 10%;
}

.input__container {
  width: 80%;
  margin: 0 auto;
  align-items: center;
}

.btn__style {
  width: 100%;
  margin-top: 6%;
}
</style>
