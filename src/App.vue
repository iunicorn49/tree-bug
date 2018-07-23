
<template>
  <div>
    <h1>测试代码</h1>
    <div>
      <div v-for="(first, fIndex) in treeData" :key="fIndex">
        <el-checkbox v-model="noLink[fIndex]">{{checkBoxs[fIndex].label}}</el-checkbox>
        <!-- <el-checkbox v-model="checkBoxs[fIndex].checked">{{checkBoxs[fIndex].label}}</el-checkbox> -->
        <div class="tree">
          <div class="tree-item" v-for="(tree, tIndex) in first.children" :key="tIndex">
              <el-tree :data="[tree]" 
								ref="tree"
								default-expand-all
								:expand-on-click-node="false"
								:default-checked-keys="checkedList"
              node-key="id" show-checkbox></el-tree>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>


  export default {
    data() {
      return {
        checkBoxs: [], // 一级 CheckBox 渲染
        treeData: [], // 二级 tree 渲染
        checkedList: [], // tree 初始默认选中项
        noLink: [false, false, false] // 无关联 一级 CheckBox 数据
      }
    },
    created() {
      let base = this._createBaseData()
      
      let checkBoxs = []
      let treeData = []
      base.forEach(first => {
        checkBoxs.push(first)
        treeData.push(first)
        first.children.forEach(s => {
          if (s.checked) {
            this.checkedList.push(s.id)
          }
        })
      })
      this.checkBoxs = checkBoxs
      this.treeData = treeData
      console.log('this.checkBoxs', this.checkBoxs)
      console.log('this.treeData', this.treeData)
      console.log('this.checkedList', this.checkedList)
    },
    methods: {
      _createBaseData() { // 造数据
        let base = []
        for (let i = 0; i < 3; i++) {
          let item = {
            id: i + 1 + 'f',
            label: `第${i}组 - 第一级`,
            checked: i === 1 ? true : false
          }
          let children = []
          for (let j = 0; j < 3; j++) {
            let c = {
              id: `${i + 1}` + j + 's',
              label: `第${i}组 - 第二级 - 第${j}个TREE`,
              checked: i === 1 ? true : false
            }
            children.push(c)
          }
          item.children = children
          base.push(item)
        }
        return base
      }
    }
  }
</script>

<style>
  .tree {
    display: flex;
    margin: 10px 0;
  }
  .tree-item {
    flex: 0 1 200px;
  }
</style>
