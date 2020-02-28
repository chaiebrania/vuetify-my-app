<template >
  <div >
   <v-data-table
    :headers="headers"
    :items="codes"
    
    class="elevation-2 ml-5"
  >
  
    <template v-slot:top>
    <h1> Instrument de mesure</h1>
    <v-container >
      <v-toolbar flat color="white">
        
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="900px">
          <template v-slot:activator="{ on }">
            <v-btn color="primary" dark class="mb-2" v-on="on">ajouter piece</v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="headline">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.name" label="Code Interne"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.designation" label="Designation"></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="close">annuler</v-btn>
              <v-btn color="blue darken-1" text @click="save">enregistre</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
      </v-container>
    </template>
    <template v-slot:item.action="{ item }">
      <v-icon
        small
        class="mr-2"
        @click="editItem(item)"
      >
        mdi-pencil
      </v-icon>
      <v-icon
        small
        @click="deleteItem(item)"
      >
        mdi-delete
      </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn color="primary" @click="initialize">Reinitialiser</v-btn>
    </template>
  </v-data-table>
  </div>
</template>

<script>
  export default {
    data: () => ({
      dialog: false,
      headers: [
        {
          text: 'Code Interne',
          align: 'start',
          sortable: false,
          value: 'name',
        },
        { text: 'Designation', value: 'designation' },
        { text: 'Actions', value: 'action', sortable: false },
      ],
      codes: [],
      editedIndex: -1,
      editedItem: {
        name: '',
        designation: 0,
      },
      defaultItem: {
        name: '',
        designation: 0,
      },
    }),

    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'ajouter piece' : 'supprimer piece '
      },
    },

    watch: {
      dialog (val) {
        val || this.close()
      },
    },

    created () {
      this.initialize()
    },

    methods: {
      initialize () {
        this.codes = [
          {
            name: 101,
           designation: 'Pieds à coulisse',
          },
        
        
          ]
      },

      editItem (item) {
        this.editedIndex = this.codes.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },

      deleteItem (item) {
        const index = this.codes.indexOf(item)
        confirm('vous étes sur de supprimer cet element !!') && this.codes.splice(index, 1)
      },

      close () {
        this.dialog = false
        setTimeout(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        }, 300)
      },

      save () {
        if (this.editedIndex > -1) {
          Object.assign(this.codes[this.editedIndex], this.editedItem)
        } else {
          this.codes.push(this.editedItem)
        }
        this.close()
      },
    },
  }
</script>