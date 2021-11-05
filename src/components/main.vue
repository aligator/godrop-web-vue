<template>
<div>
  <div class="controls">
    <v-row align="center">
  <v-col cols="12"
          sm="6">
    <div class=" buttons">
      <v-btn
      class="mx-2"
      fab
      dark
      color="indigo"
    >
      <v-icon dark>
        mdi-plus
      </v-icon>
    </v-btn><v-btn
      class="mx-2"
      fab
      dark
      color="indigo"
    >
      <v-icon dark>
        mdi-plus
      </v-icon>
    </v-btn><v-btn
      class="mx-2"
      fab
      dark
      color="indigo"
    >
      <v-icon dark>
        mdi-plus
      </v-icon>
    </v-btn>
  </div> 
  
</v-col>
  <v-col cols="12"
          sm="6"
          style="display:flex;align-items:center">
          <div class="pt-5 buttons">
    <v-text-field
    
    color="grey"
    background-color="rgb(59, 67, 73)"
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
    :headers="headers"
    :items="desserts"
    :items-per-page="5"
    class="elevation-1"
  ></v-data-table>

</div>  
</template>

<script>
export default {
data(){
  return {
    icon:true,
    message:""
  }
},

methods:{
log(){
  console.log("aaaaaaa")
  this.icon=false;
  setTimeout(()=>{this.icon=true},50);
  
}
}
}
</script>

<style>
.buttons{
  display: flex;
  justify-content: center;
  align-items: center;
}
.controls{
  display: flex;
  width:100%;
  background-color: rgb(49, 57, 63);
  /* padding: 10px; */
}
</style>