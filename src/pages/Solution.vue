<template>
  <q-page >
    <div class="col">
        <q-input @keyup.esc="searchField=''" filled bottom-slots v-model="searchField" v-select-all label="Search">
        <template v-slot:append>
          <q-icon v-if="searchField !== ''" name="close" @click="close" class="cursor-pointer" />
          <q-icon @keypress.enter="search" name="search" @click="search"/>
        </template>
        </q-input>
    </div>
    <div class="text-center constrain-more bg-green-3">
      <div class="q-pa-xl full-width text-center">
        <q-list separator bordered>
          <q-item v-for="plant in pal" :key="plant">
            <q-item-section
              ><q-btn flat @click="alert(plant.solution)" class="full-width"
                >{{ plant.name }}
              </q-btn></q-item-section
            >
          </q-item>
        </q-list>
      </div>
    </div>
  </q-page>
</template>

<script>
import plantdata from "../assets/data.json";
export default {
  data() {
    return {
      searchField:'',
      pal: plantdata,
      temp: plantdata
    };
  },
  methods: {
    search(){
      var fil={}
      var t=this.pal
      var s = this.searchField
      Object.keys(t).forEach(function(key){
        let plant = t[key]
        var pname = plant.name.toLowerCase()
        var slc = s.toLowerCase()
        if (pname.includes(slc)){
          fil[key]=plant
        }
      })
      this.pal=fil
    },
    close(){
      this.searchField=''
      this.pal=this.temp
    },
    alert(data) {
      console.log(plantdata)
      var i, len, text;
      for (
        i = 0, len = data.length, text = '<ul class="text-subtitle1">';
        i < len;
        i++
      ) {
        text += "<li>" + data[i] + "</li>";
      }
      text += "</ul>";
      console.log("Text : - ", text);
      this.$q
        .dialog({
          title: "Treatment Details ",
          message: text,
          html: true
        })
        .onOk(() => {});
    }
  }
};
</script>

<style lang="sass">
.constrain-more
  max-width: 700px
  margin: 0 auto
</style>
