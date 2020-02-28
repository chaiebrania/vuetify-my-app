<template >
  <div >
   <v-data-table
    :headers="headers"
    :items="codes"
    :height="height"
     fixed-header disable-pagination disable-sort hide-default-footer
    class="elevation-1 ml-5"
  >
  
    <template v-slot:top>
    <h1 class="mb-2">liste de suivi des instruments</h1>
    <v-container >
      <v-toolbar flat color="white">
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="900px">
          <template v-slot:activator="{ on }">
            <v-btn color="primary" dark class="mb-2" v-on="on">ajouter piece</v-btn>
          </template>
          <v-card>
            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.code" label="Code Interne"></v-text-field>
                  </v-col>
                   <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.designation" label="Designation"></v-text-field>
                  </v-col>
                   <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.lecture" label="Lecture"></v-text-field>
                  </v-col>
                   <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.precision" label="Precision"></v-text-field>
                  </v-col>
                   <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.type" label="Type"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.nserie" label="N°serie"></v-text-field>
                  </v-col>
                   <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.marque" label="Marque"></v-text-field>
                  </v-col>
                   <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.section" label="Section"></v-text-field>
                  </v-col>
                   <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.postetravail" label="Poste de travail"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.datemisenservice" label="Date mis en service"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.frequencedecalibrage" label="Frequence de calibrage"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.date1calibrage" label="Date de 1er calibrage"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.dateduderniercalibrage" label="Date du dernier calibrage"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.prochaincalibrage" label="Prochain calibrage"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.signateurcontrolleur" label="Signateur controlleur"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.signateurchefdequipe" label="Signateur chef d'equipe"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.postetravail" label="Poste de travail"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.nsemaine" label="N°semaine"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.remarque" label="Remarque"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.ncertificat" label="N°certificat"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.documentation" label="Documentation"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.instructiondecalibrage" label="Instruction de callibrage"></v-text-field>
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
        { text: 'Lecture', value: 'lecture' },
        { text: 'Precision', value: 'presision' },
        { text: 'Type', value: 'type' },
        { text: 'N°serie', value: 'nserie' },
        { text: 'Marque', value: 'marque' },
        { text: 'Section', value: 'section' },
        { text: 'Date mis en service', value: 'datemisenservice' },
        { text: 'Fréquence de calibrage', value: 'frequencedecalibrage' },
        { text: 'Date du 1er calibrage', value: 'date1calibrage' },
        { text: 'Date du dernier calibrage', value: 'datederniercalibrage' },
        { text: 'Prochain calibrage', value: 'prochaincalibrage' },
        { text: 'Signateur controlleur', value: 'signateurcontrolleur' },
        { text: 'Signateur chef d équipe', value: 'signateurchefequipe' },
        { text: 'N°semaine', value: 'nsemaine' },
        { text: 'Remarque', value: 'remarque' },
        { text: 'N°certificat', value: 'ncertificat' },
        { text: 'Documentation', value: 'documentation' },
        { text: 'Instruction de calibrage', value: 'instructiondecalibrage' },
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