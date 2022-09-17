<template>
  <div class="autocomplete">
    <div class="input" 
         @click="toggleVisible()" 
         v-text="selectedItem ? selectedItem.name : ''"></div>

    <div class="placeholder" 
         v-if="selectedItem == null" v-text="title">
    </div>

    <div class="popaver" v-if="visible">
      <input type="text" 
             ref="input"
             v-model="query" 
             @keydown.up="up" 
             @keydown.down="down"  
             @keydown.enter="selectItem" 
             placeholder="Search">

      <div class="options" ref="optionsList">
        <ul>
          <li v-for="(matche, index) in matches" 
              :key="matche.id" 
              @click="itemClicked(index)" 
              :class="{'selected' : selected == index }"
              v-text="matche.name">
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'autoComplete',
  props: {
      items: { 
        type: Array,
        default: () => []
      },
      filterby: {
        type: String
      },
      title: {
        default: 'Select One...',
        type: String
      },
      shouldReset: {
        type: Boolean,
        default: true
      }
    },
  data() {
    return {
      query: '',
      visible: false,
      selected: 0,
      selectedItem: null,
      itemHeight: 38,
    }
  },
  methods: {
    toggleVisible() {
      this.visible = !this.visible;

      setTimeout(()=>{
        this.$refs.input.focus();
      }, 50)
    },

    itemClicked(index) {
      this.selected = index;
      this.selectItem();
    },

    selectItem(){
      if(!this.matches.length){
        return;
      }

      this.selectedItem = this.matches[this.selected];
      this.visible = false;

      if (this.shouldReset) {
          this.query = '';
          this.selected = 0;
      }

      this.$emit('selectedItem', JSON.parse(JSON.stringify(this.selectedItem)));
    },

    up(){
      if(this.selected == 0){
        return;
      }

      this.selected -= 1;
      this.scrollToItem();
    },

    down(){
      if(this.selected >= this.matches.length - 1){
        return;
      }

      this.selected += 1;
      this.scrollToItem();
    },

    scrollToItem(){
      this.$refs.optionsList.scrollTop = this.selected * this.itemHeight; 
    }
  },
  computed: {
    matches() {
      this.$emit('change', this.query);

      if (this.query == '') {
        return [];
      }

      return this.items.filter((item) => item.name.toLowerCase().includes(this.query.toLowerCase()));
    }
  },
}
</script>

<style scoped>
.autocomplete {
  width: 100%;
  height: 100vh;
  position: relative;
}

.input {
  height: 40px;
  border-radius: 3px;
  border: 2px solid lightgray;
  box-shadow: 0 0 10px #eceaea;
  font-size: 25px;
  padding-left: 10px;
  padding-top: 10px;
  cursor: text;
}
.placeholder{
  position: absolute;
  top: 10px;
  left: 10px;
  font-size: 25px;
  color: #d0d0d0;
  pointer-events: none;
}
.popaver{
  min-height: 50px;
  border: 2px solid lightgray;
  position: absolute;
  top: 46px;
  left: 0;
  right: 0;
  background: #fff;
  border-radius: 3px;
  text-align: center;
}
.popaver input{
  width: 95%;
  margin-top: 5px;
  height: 40px;
  font-size: 16px;
  border-radius: 1px solid lightgray;
  padding-left: 8px;
}
.options{
  max-height: 150px;
  overflow-y: scroll;
  margin-top: 5px;
}
.options ul{
  list-style-type: none;
  text-align: left;
  padding-left: 0;
}
.options ul li{
  border-bottom: 1px solid lightgray;
  padding: 10px;
  cursor: pointer;
  background: #f1f1f1;
}
.options ul li.selected{
  background: #21c00f;
  color: #fff;
  font-weight: 600;
}
</style>
