<template>
  <div class="home">
    Test name is <input v-model="name">
    Test age is <input v-model="age">
    Test button is<br/>
    <!-- <button @click="persist">Save</button> -->
    <br/>
    <br/>
    <br/>
    <br/>
    <br/>
    <br/>
    <h2>Cats</h2>
    <div v-for="(cat, n) in cats" :key="n">
      <p>
        <span class="cat">{{ cat }}</span>
        <button @click="removeCat(n)">Remove</button>
      </p>
    </div>
    <p>
      <input v-model="newCat">
      <button @click="addCat">Add Cat</button>
    </p>
    <br/>
    <br/>
    <br/>
    <br/>
    <br/>
    <br/>
    <el-form :model="autoform"
             ref="autoform"
             label-width="80px">
      <el-form-item label="活动名称" prop="name">
        <el-input v-model="autoform.name"></el-input>
      </el-form-item>
      <el-form-item label="活动形式" prop="desc">
        <el-input type="textarea"
                  v-model="autoform.desc"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary"
                   @click="onSubmit('autoform')">立即创建</el-button>
        <el-button @click="resetForm('autoform')">重置</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>

export default {
  name: 'Home',
  data () {
    return {
      cats: [],
      newCat: null,
      name: '',
      age: 0,
      autoform: {
        name: '',
        desc: ''
      }
    }
  },
  created () {
    this.formInit()
  },
  watch: {
    autoform: {
      handler (res) {
        if (res) {
          const tempData = []
          tempData.push(this.autoform) // 值变化时保存在tempData
          localStorage.setItem('formTempData', JSON.stringify(tempData)) // 本地存储存储的是字符串
        }
      },
      deep: true
    }
  },
  mounted () {
    if (localStorage.getItem('cats')) {
      try {
        this.cats = JSON.parse(localStorage.getItem('cats'))
      } catch (e) {
        localStorage.removeItem('cats')
      }
    }
  },
  methods: {
    formInit () {
      if (localStorage.getItem('formTempData')) { // 对象不为null时，加载数据
        let arr = []
        arr = JSON.parse(localStorage.getItem('formTempData'))
        console.log('初始化数据：', arr[0])
        this.autoform = arr[0]
      }
    },
    addCat () {
      // 确保他们输入了一些东西
      if (!this.newCat) {
        return false
      }

      this.cats.push(this.newCat)
      this.newCat = ''
      this.savecats()
    },
    removeCat (x) {
      this.cats.splice(x, 1)
      this.savecats()
    },
    savecats () {
      const parsed = JSON.stringify(this.cats)
      localStorage.setItem('cats', parsed)
    },
    onSubmit (formName) {
      localStorage.removeItem('formTempData') // 提交成功时清除localStorage中的数据
      this.$refs[formName].resetFields()
      console.log(this.autoform)
    },
    resetForm (formName) {
      // 因为create有数据 所以需要清空
      this.autoform = {
        name: '',
        desc: ''
      }
      // 还要去掉localStorage
      localStorage.removeItem('formTempData')
      this.$refs[formName].resetFields()
    }
  }
}
</script>
<style lang="scss" scoped>
  .home{
    max-width: 960px;
    margin: 0 auto;
  }
</style>
