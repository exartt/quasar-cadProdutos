<template>
  <q-page class="bg-grey-3">
    <q-page class="q-pa-xl">
      <q-table
        :grid="$q.screen.xs"
        title="Cadastro de produtos"
        :filter="filtroProdutos"
        :data="novosProdutos"
        :columns="columns"
        row-key="name"
        :selected-rows-label="getSelectedString"
        selection="multiple"
        :selected.sync="selected"
      >
        <template v-slot:top-right>
          <q-input
            debounce="300"
            bg-color="white"
            dense
            rounded
            outlined
            placeholder="pesquisar"
            v-model="filtroProdutos"
          >
            <template v-slot:append>
              <q-icon name="search" />
            </template>
          </q-input>
        </template>
        <template>
          <q-btn icon="delete" @click="deletarProduto"/>
        </template>
      </q-table>
      <q-page>
        {{filtroProdutos}}
      </q-page>
    </q-page>
    <q-page-sticky position="bottom-right">
      <q-btn round color="grey" icon="add" class="q-mb-lg q-mr-lg" @click="dialog = true" />
      <q-dialog v-model="dialog">
        <q-card style="height: 300px;width: 700px;max-height: 35vh;max-width: 70vw">
            <q-card-section>
              <div class="row justify-between">
                <div class="col-6 text-h6">Cadastrar produtos</div>
                  <q-btn clickable v-close-popup icon="close" rounded flat/>
              </div>
            </q-card-section>
            <div class="line q-my-sm"/>
            <q-card-section>
                <div class="row justify-between rounded-borders ">
                    <div class="col-5" >
                      <q-input
                        bg-color="white"
                        dense
                        outlined
                        v-model="novoCodigo"
                        label="Código"
                        hint="Código do produto"
                        type="number"
                        :rules="[ val => val !== null && val !== '' || 'Digite o código do produto',
                         val => val.typeof !== 'number' || 'O código só permite valores numéricos'
                        ]"
                      >
                      </q-input>
                    </div>
                    <div class="col-6">
                      <q-input
                        bg-color="white"
                        dense
                        outlined
                        v-model="novaDescricao"
                        label="Descrição"
                        hint="Descreva sobre o produto"
                        :rules="[ val => val && val.length >0 || 'Digite a descrição do produto']"
                      />
                    </div>
                </div>
            </q-card-section>
            <div class="line q-my-sm"/>
            <q-card-actions class="card" align="right" position="bottom-right">
                  <q-btn class="button" @click="newProduct" >
                    <q-icon name="save" size="xs"/>
                    Salvar
                  </q-btn>
            </q-card-actions>
          </q-card>
      </q-dialog>
    </q-page-sticky>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      novaDescricao: '',
      novoCodigo: '',
      filtroProdutos: '',
      selected: [],
      dialog: false,
      columns: [
        {
          name: 'name',
          required: true,
          label: 'Código',
          align: 'left',
          field: row => row.name,
          format: val => `${val}`,
          sortable: true
        },
        {
          name: 'descricao',
          align: 'left',
          label: 'Descrição',
          field: 'descricao',
          sortable: true
        }
      ],
      novosProdutos:
        [
          {
            name: 'Granola',
            descricao: 'Cereal nutritivo'
          },
          {
            name: 'Banana',
            descricao: 'Fruta nutritiva'
          },
          {
            name: 'Barra energética',
            descricao: 'Super nutritiva'
          }
        ]
    }
  },
  methods: {
    newProduct () {
      if (this.novoCodigo !== '' || this.novaDescricao !== '') {
        this.novosProdutos.push({
          name: this.novoCodigo,
          descricao: this.novaDescricao
        })
        this.novoCodigo = ''
        this.novaDescricao = ''
      } else {
        alert('Por favor preencha todos os campos!')
      }
    },
    deletarProduto (p) {
      const index = this.novosProdutos.indexOf(p)
      confirm('Você tem certeza de que quer excluir esse produto?') && this.data.splice(index, 1)
    }
  }
}

</script>

<style>
.line{
  border-top: 1px solid #DCDCDC;
}
.card{
  height:35%;
}
.button{
  right: 2%;
  top: 28%;
}
</style>
