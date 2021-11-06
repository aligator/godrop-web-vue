<template>
<div>
  <div class="controls">
    <v-row align="center">
  <v-col cols="12"
          sm="6">
    <div class=" buttons">
      <v-btn
      @click="back"
      class="mx-2"
      fab
      small
      dark
      color="rgba(10,68,68,0.8)"
    >
      <v-icon dark>
        mdi-arrow-left-circle
      </v-icon>
    </v-btn>
  </div> 
  
</v-col>
  <v-col cols="12"
          sm="6"
          style="display:flex;justify-content:flex-end">
          <div class="pa-2 input">
            <v-btn
      class="mx-2"
      fab
      small
      dark
      color="rgba(88,58,48,0.5)"
    >
      <v-icon dark>
        mdi-plus
      </v-icon>
    </v-btn>
    <v-text-field
    dense
    dark
    hide-details="true"
    color="grey"
    
    background-color="rgba(59, 67, 73, .3)"
            v-model="message"
            :append-outer-icon="icon ? 'mdi-folder-plus' : 'mdi-folder-open' "
            type="text"
            outlined
            @click:append-outer="log()"
    ></v-text-field>
          </div>
    </v-col>
  </v-row>
  </div>

    <v-data-table
    style="background-color:rgba(8,50,28,0.3)"
    :loading="loading"
    :headers="headers"
    :items="desserts"
    :items-per-page="5"
    class="elevation-1"
    @click:row="handleClick"
  >
    <template v-slot:top>
      <v-toolbar
      style="background-color:rgba(8,50,28,0.2)"
        flat
      >
        <v-toolbar-title>My CRUD</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer></v-spacer>
        <v-dialog
          v-model="dialog"
          max-width="500px"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              color="primary"
              dark
              class="mb-2"
              v-bind="attrs"
              v-on="on"
            >
              New Item
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.name"
                      label="Dessert name"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.id"
                      label="ID"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.mimeType"
                      label="Type"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="blue darken-1"
                text
                @click="close"
              >
                Cancel
              </v-btn>
              <v-btn
                color="blue darken-1"
                text
                @click="save"
              >
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="text-h5">Are you sure you want to delete this item?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeDelete">Cancel</v-btn>
              <v-btn color="blue darken-1" text @click="deleteItemConfirm">OK</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
      
      
      <v-icon
        small
        class="mr-2"
        @click="editItem(item)"
      >
        mdi-pencil
      </v-icon>
      <v-icon
      class="mr-3"
        small
        @click="deleteItem(item)"
      >
        mdi-delete
      </v-icon>
    <v-icon
      v-if="item.mimeType!=null"
        small
        class="mr-2"
        @click="editItem(item)"
      >
        mdi-download
      </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn
        color="primary"
        @click="initialize"
      >
        Reset
      </v-btn>
    </template>
  </v-data-table>

</div>  
</template>

<script>
import gql from 'graphql-tag';
export default {
  apollo:{
    getFileNode:{
      query:gql`
    fragment FileNode on FileNode {
    id,
    name,
    description,
    isFolder,
    state,
    mimeType,
    size,
}

fragment FileNodeWithChildren on FileNode {
    ...FileNode
    children {
        ...FileNode
    }
}
    query GetFileNode($path: String!) {
      getFileNode(path: $path) {
        ...FileNodeWithChildren
      }
    }`,
      variables: {
      path: '\\',
    },
    },


  },
data(){
  return {
    path:"",
    loading:true,
    getFileNode:[],
         dialog: false,
      dialogDelete: false,
    icon:true,
    desserts:[],
    message:"",
    headers:[
          {
            text: 'ETC',
            align: 'start',
            sortable: false,
            value: 'name',
          },
          { text: 'Pfad', value: 'id' },
          { text: 'Type', value: 'mimeType' },
        { text: 'Actions', value: 'actions', sortable: false },
        ],

         editedIndex: -1,
      editedItem: {
        name: '',
        id: 0,
        isFolder: 0,
      },
      defaultItem: {
        name: '',
        id: 0,
        isFolder: 0,
      },
  }
},

  watch: {
      dialog (val) {
        val || this.close()
      },
      dialogDelete (val) {
        val || this.closeDelete()
      },
      getFileNode() {
        this.path=this.getFileNode.id;
        console.log(this.getFileNode);
        this.desserts=this.getFileNode.children||[];
        this.loading=false;
      }
    },
 computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
      },
    },

     created () {
      this.initialize()
    },


methods:{
  back(){
       
    const arr = this.path.split("\\");
    
    arr.pop();
    
    this.path = arr.join("\\");

    if(arr.length<=1)this.path="\\";
    this.refetch(this.path);
    
  },
  handleClick(row){
    if(row.isFolder) this.refetch(row.id);
  },
  initialize () {

        this.refetch("\\");
      },
       editItem (item) {
        this.editedIndex = this.desserts.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },
        deleteItem (item) {
        this.editedIndex = this.desserts.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialogDelete = true
      },
         deleteItemConfirm () {
        this.desserts.splice(this.editedIndex, 1)
        this.closeDelete()
      },
         close () {
        this.dialog = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },
        closeDelete () {
        this.dialogDelete = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },
      save () {
        if (this.editedIndex > -1) {
          Object.assign(this.desserts[this.editedIndex], this.editedItem)
        } else {
          this.desserts.push(this.editedItem)
        }
        this.close()
      },
log(){
  this.$apollo.queries.getFileNode.refetch({
  path: '/test'
})
  
  this.icon=false;
  setTimeout(()=>{this.icon=true},50);
  
},
refetch(path){
    this.$apollo.queries.getFileNode.refetch({
  path: path
})
}
}
}
</script>

<style>
.input{
   display: flex;
  justify-content: flex-end;
}
.buttons{
  display: flex;
  justify-content: flex-start;
}
.controls{
  display: flex;
  width:100%;
  background-color: rgb(2, 131, 109);
  /* padding: 10px; */
}

 tr:hover { background-color: rgba(0,0,22,0.3) !important; }
</style>