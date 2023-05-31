<script>
import {defineComponent} from 'vue'
import request from "../utils/request";
import Cookies from 'js-cookie'
import index from "./Index.vue";


const item = {
  product_id: '',
  name: '',
  price: '',
  quantity: '',
}

const header = {
  prop: "key",
  label: "自定义",
  editable: false,
  type: "input",
}

export default defineComponent({
  name: "Prodect",
  data() {
    return {
      user: Cookies.get('user') ? JSON.parse(Cookies.get('user')) : {},
      tableHeader: [
        {
          prop: "product_id",
          label: "ID",
          editable: true,
          type: "input",
        },
        {
          prop: "name",
          label: "商品名",
          editable: true,
          type: "input"
        },
        {
          prop: "price",
          label: "价格",
          editable: true,
          type: "input"
        },
        {
          prop: "quantity",
          label: "数量",
          editable: true,
          type: "input"
        },
      ],
      tableData: [{
        product_id: '1',
        name: '冬枣',
        price: '30.0',
        quantity: '100',
      }]
    }
  },
  created() {
    this.productInquire();
  },
  methods: {
    logout() {
      Cookies.remove('user');
      this.$router.push('/index')
    },
    productInquire() {
      request.get('/tproduct/productInquire').then(res => {
        if (res.code === '200') {
          this.tableData = res.data;
        }
      })
    },
    handleEdit(row) {
      // row.editable = true;
      console.log(row);

    },
    updateProduct(index) {
      console.log(index);
      let updateProduct = this.tableData[index];
      console.log(updateProduct);
      request.put('/tproduct/productChange',updateProduct).then( res =>  {
        if (res.code === '200') {
          this.$notify.success("更新成功");
          this.productInquire();
        } else  {
          this.$notify.success("更新失败");
          this.productInquire();
        }
      })
    },
    handleDelete(index) {
      console.log("删除操作", index + 1)

      let deleteId = this.tableData[index].product_id;
      console.log(deleteId)
      const url = '/tproduct/productDeleteById/' + deleteId ;
      request.delete(url).then(res => {
        if (res.code === '200') {
          this.$notify.success("删除成功");
          this.productInquire();
        } else {
          this.$notify.success("删除失败");
          this.productInquire();
        }
      })
    },

  }

})
</script>

<template>
  <!-- 头部栏 -->
  <div class="top_column">
    <table class="top_table">
      <tr>
        <td>
          <router-link to="/index">首页</router-link>
        </td>
        <td>
          <router-link to="/product">浏览商品</router-link>
        </td>
        <td>
          <router-link to="/AddProduct">添加商品</router-link>
        </td>
      </tr>
    </table>

    <table class="top_table">

      <tr>
        <td><h2>《云计算应用开发》 课程设计</h2></td>
      </tr>
    </table>

    <table class="top_table">
      <tr>
        <td>
          <router-link to="/index">欢迎：{{ user.name }}</router-link>
        </td>
        <td>
          <router-link to="/index" @click="logout()">退出</router-link>
        </td>
      </tr>
    </table>
  </div>

  <!-- 中部主体 -->
  <div class="mid_column">
    <ul></ul>
    <div style="padding-top: 50px">
      <!--      表头-->
      <el-table :data="tableData" style="width: 120vh; background-color: white">
        <el-table-column
            :prop="item.prop"
            :label="item.label"
            v-for="(item, index) in tableHeader"
            :key="item.prop"
        >
          <template #default="scope">
            <div v-show="item.editable || scope.row.editable"
                 class="editable-row">
              <template v-if="item.type === 'input'">
                <el-input
                    size="small"
                    v-model="scope.row[item.prop]"
                    @change="handleEdit(scope.$index, scope.row)"/>
              </template>
            </div>
          </template>

        </el-table-column>

        <el-table-column label="操作">
          <template #default="scope">
            <el-button
                size="small"
                @click="updateProduct(scope.$index)"
            >更新
            </el-button>

            <el-button
                size="small"
                type="danger"
                @click="handleDelete(scope.$index)"
            >删除
            </el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
  </div>

  <!-- 底部脚标 -->
  <div class="bottom_column">
    <h2>焦梓豪</h2>
  </div>
</template>


<style src="../style/product.css" scoped>

</style>