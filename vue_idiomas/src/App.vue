<template>
  <div class="h-screen">
    <HeaderComponent />
    <div class="max-w-(90rem) my-0 mx-auto px-24 py-20">
      <h1 class="font-semibold text-3x1 text-white-600">
        Lista dos alunos do curso
      </h1>
      <div class="flex justify-between items-center mt-10">
        <div class="flex flex-col w-[48%]">
          <label class="font-semibold text-base text-grey-300">
            Pesquisar
          </label>
          <input type="text" placeholder="Nome do aluno"
            class="bg-dark-400 rounded-lg mt-2 border-none py-3 px-5 text-white-600 text-base" 
            @key="onSearch"
            v-model="search"
            />
        </div>
        <div class="flex flex-col w-[48%]">
          <label class="font-semibold text-base text-grey-300">
            Nacionalidade
          </label>
          <select name="nacionalidade" id="nacionalidade"
            class="bg-dark-400 rounded-lg mt-2 border-none py-3 px-5 text-white-600 text-base hover:cursor-auto"
            @change="onSearch"
            v-model="searchCountry"
            >
            <option value="">Todas</option>
            <option
            v-for="nat in $store.state.dataUser"
            :key="nat.location.country.pastcode"
            >   
            {{ nat.location.country }}           
            </option>
          </select>
        </div>
      </div>
      <div class="overflow-x-auto relative shadow-md sm:rounded-lg mt-8">
        <table class="w-full text-base text-left text-grey-400 bg-dark-400">
          <thead>
            <tr>
              <th scope="col" class="py-3 px-6">Nome</th>
              <th scope="col" class="py-3 px-6">Sexo</th>
              <th scope="col" class="py-3 px-6">Nacionalidade</th>
              <th scope="col" class="py-3 px-6">Ação</th>
            </tr>
          </thead>
          <tbody>
            <tr class="border-t border-grey-300" v-for="user in onSearch()" :key="user.id.value">
              <th scope="row" class="py-4 px-6 font-medium whitespace-nowrap text-white-400">
                {{ `${user.name.first} ${user.name.last}` }}
              </th>
              <th class="py-4 px-6 font-medium text-white-400">
                {{ user.gender }}
              </th>
              <th class="py-4 px-6 font-medium text-white-400">
                {{ user.location.country }}
              </th>
              <th class="py-4 px-6 font-medium text-white-400">
                <button class="bg-dark-500 rounded py-0.5 px-1.5">
                  Visualizar
                </button>
              </th>
            </tr>
          </tbody>
        </table>
      </div>
      <div>

      </div>
    </div>
  </div>
</template>

<script>
import HeaderComponent from '@/components/headerComponent.vue'

export default {
  name: 'App',
  components: {
    HeaderComponent,
  },
  data() {
    return {
      loading: false,
      search: '',
      searchCountry: ''
    }
  },
  created() {
    this.loading = true;
    this.$store.dispatch('getUsers').finally(() => {
      this.loading = false;
    })
  },
  methods: {
    onSearch() {
      if (this.search.length > 0) {
        return this.$store.state.dataUser.filter(a => (a.name.first.toUpperCase() || a.name.last.toUpperCase()).includes(this.search.toUpperCase()))
      } else if(this.searchCountry.length > 0){
        return this.$store.state.dataUser.filter(a => a.location.country.toUpperCase().includes(this.searchCountry.toUpperCase()))
      } else {
        return this.$store.state.dataUser;
      }
    }
  }
}
</script>

<style>

</style>
