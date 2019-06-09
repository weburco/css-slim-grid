<template>
  <div id="app">
    <div class="wrap title">
      <h1>Bootsrap Slim Grid 1.0</h1>
      <a href>
        <div class="d-flex">
          <h5>Github</h5>
          <div class="icon">
            <img src="/github.svg" alt>
          </div>
        </div>
      </a>
    </div>
    <div class="w-container" v-for="(blok,index) in bloks" :key="index">
      <div class="w-top">
        <div class="delete-wrap" @click="deleteContainer(index)">
          <div class="icon">
            <img src="/delete.svg" alt>
          </div>
        </div>
        <div class="wrap-settings">
          <div class="options-wrap">
            <select name id v-model="blok.colNum">
              <option :value="ind+1" v-for="(option,ind) in 12" :key="ind">{{option}}</option>
            </select>
          </div>
          <div class="add-col" @click="addCol(index)">Add Col</div>
        </div>
      </div>
      <div class="wrap" :ref="`wrap-element-${index}`">
        <div :class="[`col-${item.itemColNum}`]" v-for="(item,i) in blok.items" :key="i">
          <div class="col-item">
            <select
              name
              id
              v-model="item.itemColNum"
              class="option-col"
              @change="item.itemColNum === 'delete'? blok.items.splice(i,1):''"
            >
              <option value="delete">Delete</option>
              <option :value="ind+1" v-for="(option,ind) in 12" :key="ind">{{option}}</option>
            </select>
            <div class>item-{{i+1}}</div>
          </div>
        </div>
      </div>
    </div>
    <div class="wrap add-bloks" @click="addContainer">Add Wrap
      <div class="icon">
        <img src="/add-icon.svg" alt>
      </div>
    </div>
    <div class="wrap">
      <div class="bg-output col-6 col-sm-10">
        <div class="d-flex" style="justify-content:space-between;">
          <h4>Output</h4>
          <div style="cursor:pointer" @click="copyOutput" ref="copyElement">Copy</div>
        </div>
        <div class="output-color">
          <div v-for="(container,index) in outerHtml" :key="index" ref="outputElement">
            <div>{{container.wrap}}</div>
            <div class="ml-10">
              <div v-for="(item,ind) in container.items" :key="ind">{{item.col}}</div>
            </div>
            <div>{{container.wrapEnd}}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      bloks: [
        {
          name: "component",
          colNum: 12,
          items: []
        }
      ]
    };
  },
  computed: {
    outerHtml() {
      let html = [];
      for (let index = 0; index < this.bloks.length; index++) {
        html.push({
          wrap: `<div class='wrap'>`,
          items: []
        });
        for (let ind = 0; ind < this.bloks[index].items.length; ind++) {
          html[index].items.push({
            col: `<div class='col-${
              this.bloks[index].items[ind].itemColNum
            }'>item-${ind + 1}</div>`
          });
        }
        html[index].wrapEnd = "</div>";
      }
      return html;
    }
  },
  methods: {
    addContainer() {
      const container = {
        name: "component",
        colNum: 12,
        items: []
      };
      this.bloks.push(container);
    },
    addCol(index) {
      this.bloks[index].items.push({ itemColNum: this.bloks[index].colNum });
    },
    deleteContainer(index) {
      this.bloks.splice(index, 1);
    },
    delete(containerIndex, colIndex) {
      this.bloks[containerIndex].items.splice(colIndex, 1);
    },
    copyOutput() {
      let copyElement = '';
      for (let index = 0; index < this.$refs.outputElement.length; index++) {
        for (let i = 0; i < this.$refs.outputElement[index].children.length; i++) {
          copyElement +=this.$refs.outputElement[index].children[i].innerText;
        }
      }
      console.log(copyElement)
      const el = document.createElement('textarea');
      el.value = copyElement;
      document.body.appendChild(el);
      el.select();
      document.execCommand('copy');
      this.$refs.copyElement.innerText='Copied'
        document.body.removeChild(el);
      setTimeout(() => {
        this.$refs.copyElement.innerText='Copy'
      }, 2000);
    }
  }
};
</script>

<style>
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
a {
  color: inherit;
  text-decoration: none;
}
.ml-10 {
  margin: 5px 0 5px 20px;
}
.bg-output {
  padding: 10px 25px;
  background: #eee;
  margin: 30px 0;
}
.output-color {
  color: #313131;
  margin-top: 10px;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  width: 90%;
  margin: 0 auto;
}
.d-flex {
  display: flex;
  align-items: center;
}
.title {
  padding: 30px 0;
  justify-content: space-between;
}
.w-container {
  margin-bottom: 20px;
}
.w-top {
  display: flex;
  justify-content: space-between;
}
.add-col,
.delete-wrap {
  padding: 8px 15px;
  color: #000;
  cursor: pointer;
}
.add-col {
  background-color: lightblue;
  border: 1px dashed #54b3d3;
  border-bottom: none;
}
.wrap-settings {
  display: flex;
  align-items: center;
}
.options-wrap {
  margin-right: 10px;
  position: relative;
  cursor: pointer;
}

.delete-wrap {
  border: 1px dashed lightcoral;
  border-bottom: none;
  background: lightpink;
}
.delete-wrap .icon {
  margin-left: 0;
}
.w-container .wrap {
  background-color: #eee;
  padding: 15px;
  border: 2px dashed #d4d4d4;
}

.btn-delete {
  position: fixed;
  right: 30px;
  cursor: pointer;
}
.option-col {
  position: absolute;
  display: none;
  top: 5px;
  right: 3px;
  padding: 5px 8px;
}
.col-item:hover .option-col {
  display: block;
}
.col-item {
  position: relative;
  padding: 20px;
  margin: 5px 10px;
  background-color: lightcoral;
  border-radius: 0.25rem;
  color: #fff;
}
.add-bloks {
  align-items: center;
  border: 3px dotted #2196F3;
  height: 50px;
  margin-top: 30px;
  justify-content: center;
  font-size: 22px;
  color: #2196F3;
  border-radius: 0.25rem;
  cursor: pointer;
}
.icon {
  width: 22px;
  height: 22px;
  margin-left: 5px;
}
.icon img {
  width: 100%;
  height: 100%;
}
</style>
