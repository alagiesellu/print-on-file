<template>
  <div id="app">
    <div class="dont_print">
      <input type="file" @change="onFileChange">
      <button v-on:click="print_list()">Print</button>
    </div>
    <div class="container" v-for="(office, i) in list" :key="i">
      <div class="item">
        <div v-for="line in office" :key="line" v-html="line"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      list: [],
    }
  },
  methods: {
    print_list: function() {
      window.print();
    },
    onFileChange: function(e){
      this.new_el();
      this.createImage(e.target.files[0]);
    },
    createImage(file) {
      let vm = this;
      let reader = new FileReader();
      reader.onload = (e) => {

        for (let el of e.target.result.split("\n"))
          if (el === '')
            vm.new_el();
          else
          {
            if (el.startsWith('*'))
              el = '<strong>'+el.substr(1)+'</strong>';
            vm.list[vm.list.length-1].push(el)
          }

      };
      reader.readAsText(file)
    },
    new_el: function () {
      this.list.push([]);
    },
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-transform: uppercase;
  color: black;
  font-size: 20px;
}
.container{
  width: 100%;
  display: flex;
  height: 100vh;
  justify-content: center;
  background: linear-gradient(rgba(255,255,255,0.95), rgba(255,255,255,0.95)), url("/logo.png") no-repeat;
  background-size: contain;
  background-position: center;
}
.item{
  align-self: center;
}

  /*.file{*/
  /*  text-transform: uppercase;*/
  /*  align-content: center;*/
  /*  color: black;*/
  /*  font-size: 20px;*/
  /*  display: table;*/
  /*  margin: 27% auto;*/
  /*}*/

@media print {
  /*.pagebreak {*/
  /*  clear: both;*/
  /*  page-break-after: always;*/
  /*}*/
  .dont_print {
    display: none;
  }
}

</style>
