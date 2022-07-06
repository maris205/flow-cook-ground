<template>
  <div class="app-container">

    <el-form :inline="true"  class="demo-form-inline">
      <div  v-for="item in item_prob">
        <el-form-item label="Item name">
          <el-input v-model="item.name" disabled placeholder="name like ipad" />
        </el-form-item>
        <el-form-item label="Item prob">
          <el-input v-model="item.prob" disabled placeholder="prob like 0.1, [0,1]" />
        </el-form-item>

        <br/>
      </div>

    </el-form>

    <hr/>

    <el-form ref="form" label-width="120px">

      <el-form-item label="Contract name">
        <el-input v-model="name"
                  placeholder="please input contract name，the NFT collection Name"
        />
      </el-form-item>

      <el-form-item label="Code type">
        <el-select v-model="es_index"  @change="change_index" placeholder="">
          <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
      </el-form-item>


      <el-form-item>
        <el-button type="primary" @click="analyze()">
          Generate code
        </el-button>
      </el-form-item>
    </el-form>

    <el-form ref="form1"  label-width="120px">
      <el-form-item label="Code content">
        <el-input v-model="code_text"
                  placeholder="Main  code"
                  :rows="10" type="textarea"/>
      </el-form-item>

    </el-form>


  </div>
</template>

<script>

import { code } from '@/api/es_code'

export default {
  data() {
    return {
      formInline:{},
      item_prob:[{"name":"纸巾","prob":0.5},
        {"name":"鼠标","prob":0.3},
        {"name":"键盘","prob":0.14},
        {"name":"iPad","prob":0.05},
        {"name":"Macbook","prob":0.01}],
      name:"",//输入的行业名称
      code_text:"",//结果
      select_index:"",
      es_index:"",
      options: [],//索引列表
    }
  },
  created() {
    this.get_index()
  },
  methods: {
    filterNode(value, data) {
      if (!value) return true
      return data.label.indexOf(value) !== -1
    },
    //获得类型列表
    get_index() {
      this.options.push({value:'cadence',label:'cadence'})
      this.options.push({value:'vue.js',label:'vue.js'})

      this.es_index = 'cadence' //默认一个index
      this.select_index = "cadence"
    },

    change_index(es_index) {
      this.select_index = es_index
      console.log("select index", es_index)
    },

    //获得合约
    analyze() {
      let data = {
        "name": this.name,
        "code_type": this.select_index
      }
      code(data).then(response => {
        console.log(response)
        this.code_text = response.results[0].contract
      })
    },

  }
}
</script>
