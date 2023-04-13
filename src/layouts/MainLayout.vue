<template>
  <q-layout>
    <div class="q-pa-md" style="min-height: 100vh;">
      <q-card style="border: 2px solid #df091d;">
        <div>&nbsp;</div>
        <q-separator class="bg-red-10" inset></q-separator>
        <div class="text-h6 text-negative" style="padding-left: 15px">
          Obter Contactos para Tratamento
        </div>
        <q-separator class="bg-red-10" inset></q-separator>
        <q-card-section class="pt-4">
          <q-toolbar class="bg-red-10 text-white q-py-md shadow-2">
            <q-btn-dropdown stretch flat label="Selecione">
              <div>
                <q-scroll-area visible :thumb-style="thumbStyles" :bar-style="barStyles"
                  style="min-width:200px ;height: 400px;width: 300px" class="col">
                  <q-list class="q-pa-sm">
                    <q-item-label header class="bg-red-10 text-white">Contactos</q-item-label>
                    <q-item v-for="link in linksList " :key="link.title" clickable v-close-popup tabindex="0"
                      :model-value="firstSelection" @click="changeSelection(link.title)">
                      <q-item-section avatar>
                        <q-avatar square size="40px">
                          <img :src="link.icon">
                        </q-avatar>
                      </q-item-section>
                      <q-item-section>
                        <q-item-label>{{ link.title
                        }}</q-item-label>
                      </q-item-section>
                    </q-item>
                    <q-separator inset spaced />
                    <q-item-label header class="bg-red-10 text-white">Pesquisa de Entidades</q-item-label>
                    <q-item :model-value="firstSelection" @click="changeSelection(dataSearch.title)" clickable
                      v-close-popup tabindex="0">
                      <q-item-section avatar>
                        <q-avatar square size="40px">
                          <img :src="dataSearch.icon">
                        </q-avatar>
                      </q-item-section>
                      <q-item-section>
                        <q-item-label>{{ dataSearch.title }}</q-item-label>
                      </q-item-section>
                    </q-item>
                  </q-list>
                </q-scroll-area>
              </div>
            </q-btn-dropdown>
            <div class="col-10" v-if="componetSearch">
              <q-form>
                <div class="row q-col-gutter-md ">
                  <div class="col-2 ">
                    <q-select v-model="optionSelection" outlined color="black" bg-color="white" :options="options"
                      label="Filtrar Entidade por" @update:model-value="changeSelectionSearch" />
                  </div>
                  <div class="col-8 " v-if="optionSelection != ''">
                    <q-input v-if="optionSelection == 'NIF'" v-model="textSearch" label-color="dark" outlined
                      color="white" bg-color="white" label="Filtro da Entidade" lazy-rules :disable="disable">
                      <template v-slot:prepend>
                        <q-icon name="search"></q-icon>
                      </template>
                    </q-input>
                    <q-input v-model="textSearch" label-color="dark" outlined color="white" bg-color="white"
                      label="Filtro da Entidade" lazy-rules :disable="disable">
                      <template v-slot:prepend>
                        <q-icon name="search"></q-icon>
                      </template>
                    </q-input>
                  </div>
                  <div class="col-2  q-pt-lg">
                    <q-btn color="red" icon="search" label="Pesquisar" size="md" />
                  </div>
                </div>
              </q-form>
            </div>
          </q-toolbar>

          <div class="text-h6 text-negative" v-if="componetTable">
            <board-component :title="firstSelection"></board-component>
          </div>
        </q-card-section>
      </q-card>
      <messages-component v-model:loading="dialogData.loading"
        v-model:deleteConfirmation="dialogData.deleteConfirmation"></messages-component>
    </div>
  </q-layout>
</template>

<script lang="ts">
import { defineComponent, ref, reactive } from 'vue'
import BoardComponent from 'components/BoardComponent.vue'
import MessagesComponent from 'components/MessagesComponent.vue'
export default defineComponent({
  name: 'MainLayout',

  components: {
    BoardComponent,
    MessagesComponent
  },
  data: () => ({
  }),
  setup () {
    const firstSelection = ref('')
    const optionSelection = ref('')
    const linksList = ref([
      { title: 'Contactos a processar', icon: 'src/assets/images/open-book.png' },
      { title: 'Contactos a processar pelo NIF', icon: 'src/assets/images/card-document.png' },
      { title: 'Contactos a processar pelo Email', icon: 'src/assets/images/arroba.png' },
      { title: 'Contactos a processar pelo Contacto Tlf', icon: 'src/assets/images/old-phone.png' },
      { title: 'Contactos My Toyota', icon: 'src/assets/images/toyota.png' },
      { title: 'Contactos a processar pelo Golden Record', icon: 'src/assets/images/gold-card.png' }
    ])
    const dataSearch = { title: 'Aplicar filtro', icon: 'src/assets/images/searching.png' }
    const thumbStyles = {
      right: '4px',
      borderRadius: '7px',
      backgroundColor: 'red',
      width: '4px',
      opacity: '0.75'
    }
    const barStyles = {
      right: '2px',
      borderRadius: '9px',
      backgroundColor: 'red',
      width: '8px',
      opacity: '0.2'
    }
    const textSearch = ref('')
    const componetSearch = ref(false)
    const componetTable = ref(false)
    const options = [
      'NIF', 'Nome', 'Email', 'Contacto Telefónico', 'Matrícula'
    ]
    const dialogData = reactive({
      loading: false,
      deleteConfirmation: false
    })
    const componetProgress = ref(true)
    const changeSelection = (data: any) => {
      firstSelection.value = data
      componetSearch.value = false
      componetTable.value = false
      optionSelection.value = ''
      disable.value = true
      componetProgress.value = true
      dialogData.loading = true
      dialogData.deleteConfirmation = true
      if (data === 'Aplicar filtro') {
        componetSearch.value = true
      } else {
        componetTable.value = true
      }
      setTimeout(() => {
        dialogData.loading = false
      }, 10000)
    }
    const changeSelectionSearch = () => {
      textSearch.value = ''
      disable.value = false
    }

    const disable = ref(true)

    const inputRef = ref(null)
    return {
      firstSelection,
      optionSelection,
      linksList,
      dataSearch,
      thumbStyles,
      barStyles,
      textSearch,
      componetSearch,
      options,
      componetTable,
      dialogData,
      disable,
      componetProgress,
      changeSelection,
      changeSelectionSearch,

      model: ref(''),
      inputRef,

      myRule (val: any) {

        return new Promise((resolve, reject) => {
          setTimeout(() => {
            resolve(!!val || '')
          }, 1000)
        })
      }
    }
  },
  methods: {
  }
})
</script>
