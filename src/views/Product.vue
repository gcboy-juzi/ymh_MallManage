<template>
  <div style="padding: 10px;">
    <div style="margin: 10px 0;display: flex;">
      <el-input v-model="search" placeholder="请输入" style="width: 20%;" clearable/>
      <el-button type="primary" style="margin-left: 5px" @click="lode">查询</el-button>
      <el-button type="primary" @click="add">新增</el-button>
      <el-popconfirm title="确认删除吗?" @confirm="deleteBatch">
        <template #reference>
          <el-button type="danger">批量删除</el-button>
        </template>
      </el-popconfirm>
    </div>
        <el-dialog title="" v-model="dialogVisible">
          <el-form
              ref="fruitRules"
              :model="admins"
              label-width="120px">
            <el-form-item label="商品图片" prop="productImage">
              <el-upload
                  ref="uplode"
                  action="http://localhost:8888/files/uplode"
                  :on-success="filesUplodeSeccess">
                <el-button size="small" type="primary">点击上传</el-button>
                <div solt="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
              </el-upload>

            </el-form-item>
            <el-form-item label="商品名称" prop="productName">
              <el-input v-model="admins.productName"></el-input>
            </el-form-item>

            <el-form-item label="商品数量" prop="productMnum">
              <el-input v-model="admins.productMnum"></el-input>
            </el-form-item>

            <el-form-item label="商品颜色1" prop="productCs">
              <el-input v-model="admins.productCs"></el-input>
            </el-form-item>

            <el-form-item label="商品颜色2" prop="productCd">
              <el-input v-model="admins.productCd"></el-input>
            </el-form-item>

            <el-form-item label="商品颜色3" prop="productCf">
              <el-input v-model="admins.productCf"></el-input>
            </el-form-item>
            <el-form-item label="商品颜色4" prop="productCg">
              <el-input v-model="admins.productCg"></el-input>
            </el-form-item>
            <el-form-item label="商品副图1" prop="productIm">
              <el-upload
                  ref="uplode"
                  action="http://localhost:8888/files/uplode"
                  :on-success="filesUplodeSeccessd">
                <el-button size="small" type="primary">点击上传</el-button>
                <div solt="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
              </el-upload>
            </el-form-item>
            <el-form-item label="商品副图2" prop="productImd">
              <el-upload
                  ref="uplode"
                  action="http://localhost:8888/files/uplode"
                  :on-success="filesUplodeSeccessf">
                <el-button size="small" type="primary">点击上传</el-button>
                <div solt="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
              </el-upload>
            </el-form-item>
            <el-form-item label="商品副图3" prop="productImf">
              <el-upload
                  ref="uplode"
                  action="http://localhost:8888/files/uplode"
                  :on-success="filesUplodeSeccessg">
                <el-button size="small" type="primary">点击上传</el-button>
                <div solt="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
              </el-upload>
            </el-form-item>
            <el-form-item label="商品副图4" prop="productImg">
              <el-upload
                  ref="uplode"
                  action="http://localhost:8888/files/uplode"
                  :on-success="filesUplodeSeccessh">
                <el-button size="small" type="primary">点击上传</el-button>
                <div solt="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
              </el-upload>
            </el-form-item>
            <el-form-item label="价格" prop="productPrice">
              <el-input v-model="admins.productPrice"></el-input>
            </el-form-item>
            <el-form-item label="重量" prop="weight">
              <el-input v-model="admins.weight"></el-input>
            </el-form-item>
            <el-form-item label="一级分类ID" prop="oneCategoryId">
              <el-input v-model="admins.oneCategoryId"></el-input>
            </el-form-item>
            <el-form-item label="二级分类ID" prop="twoCategoryId">
              <el-input v-model="admins.twoCategoryId"></el-input>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="onSubmit">确定</el-button>
              <el-button @click="dialogVisible = false">取消</el-button>
            </el-form-item>
          </el-form>
        </el-dialog>

    <!-- 检索结果 -->
    <el-row :gutter="0">
      <el-col :span="20">
        <el-table :data="admin" border stripe style="width: 100%" @selection-change="handleSelectionChange">
          <el-table-column type="selection" width="40" />
          <el-table-column  prop="productId" label="ID" width="60" sortable/>
          <el-table-column prop="productImage" label="商品图片" width="120">
            <template #default="scope">
              <el-image style="width: 100px; height: 100px" :src="scope.row.productImage" :preview-src-list="[scope.row.productImage]">
              </el-image>
            </template>
          </el-table-column>
          <el-table-column prop="productName" label="商品名称" width="200"/>
          <el-table-column prop="productCs" label="商品颜色1" width="80"/>
          <el-table-column prop="productCd" label="商品颜色2" width="80"/>
          <el-table-column prop="productCf" label="商品颜色3" width="80"/>
          <el-table-column prop="productCg" label="商品颜色4" width="80"/>
          <el-table-column prop="productPrice" label="商品价格" width="80"/>
          <el-table-column prop="productMnum" label="商品数量" width="80"/>
          <el-table-column prop="oneCategoryId" label="一级分类ID" width="90"/>
          <el-table-column prop="twoCategoryId" label="二级分类ID" width="90"/>
          <el-table-column prop="createTime" label="创建时间" width="150"/>
          <el-table-column prop="updateTime" label="最后修改时间" width="150"/>
          <el-table-column fixed="right" label="操作" align="center" width="180">
            <template #default="scope">
<!--              <el-button-->
<!--                  type="primary"-->
<!--                  @click="buy(scope.row.productId)">-->
<!--                购买-->
<!--              </el-button>-->
              <el-button
                  type="primary"
                  icon="el-icon-edit"
                  circle
                  @click="edit(scope.row)">
              </el-button>
              <el-button
                  type="danger"
                  icon="el-icon-delete"
                  circle
                  @click="del(scope.row)">
              </el-button>
            </template>
          </el-table-column>
        </el-table>
      </el-col>
    </el-row>

    <!-- 分页 -->
    <div style="text-align: center;margin: 20px 0">
        <el-pagination v-model:currentPage="currentPage" :page-sizes="[5, 10, 20, 30]" :page-size="pageSize"
                       layout="total, sizes, prev, pager, next, jumper" :total="total" @size-change="handleSizeChange"
                       @current-change="handleCurrentChange">
        </el-pagination>
    </div>


  </div>
</template>

<script>
import request from "@/utils/request.js"
import {ElMessage} from 'element-plus'
export default {
  name: 'Product',

  created() {
    this.lode()

  },
  methods: {

    filesUplodeSeccess(res){
      console.log(res)
      this.admins.productImage = res.data

    },
    filesUplodeSeccessd(res){
      console.log(res)
      this.admins.productIm = res.data

    },
    filesUplodeSeccessf(res){
      console.log(res)
      this.admins.productImd = res.data

    },
    filesUplodeSeccessg(res){
      console.log(res)
      this.admins.productImf = res.data

    },
    filesUplodeSeccessh(res){
      console.log(res)
      this.admins.productImg = res.data

    },
    // buy(productId){
    //   request.get("/master/buy/" + productId).then(res => {
    //     // 请求成功跳转沙箱支付的页面
    //     window.open(res.data)
    //   })
    // },

    onSubmit() {
      if (this.admins.productId) {
        request.put("/info/update", this.admins).then(res => {
          console.log(res);
          if (res.code === 200) {
            ElMessage({
              type: 'success',
              message: '修改成功',
            })
          } else {
            ElMessage({
              type: 'error',
              message: res.msg
            })
          }
          this.lode()
          this.dialogVisible = false
        })
      } else {
        request.post("/info/add", this.admins).then(res => {
          console.log(res);
          if (res.code === 200) {
            ElMessage({
              type: 'success',
              message: '添加成功',
            })
          } else {
            ElMessage({
              type: 'error',
              message: res.msg
            })
          }
          this.lode()
          this.dialogVisible = false
        })
      }
    },

    lode() {
      request.get("/info/page", {
        params: {
          pageNum: this.currentPage,
          pageSize: this.pageSize,
          search: this.search,
        }
      }).then(res => {
        console.log(res);
        this.admin = res.data.records
        this.total = res.data.total
      })

    },
    add() {
      this.dialogVisible = true
      this.admins = {}
      if (this.$refs['upload']) {
        this.$refs['upload'].clearFiles()  // 清除历史文件列表
      }
    },

    //修改
    edit(row) {
      this.admins = JSON.parse(JSON.stringify(row))
      this.dialogVisible = true
      this.$nextTick(() =>{
        this.$refs['uplode'].clearFiles()
      })
    },
    //删除
    del(row) {
      this.$confirm("是否确定要删除" , {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      }).then(() => {
            request
                .delete("/info/delete/" + row.productId)
                .then(res => {
                  if (res.code === 200) {
                    ElMessage({
                      type: 'success',
                      message: '删除成功',
                    })
                  } else {
                    ElMessage({
                      type: 'error',
                      message: res.msg
                    })
                  }
                  this.lode()
                });
          })
          .catch(() => {});
    },
    deleteBatch(){
      if (!this.ids.length){
        ElMessage({
          type:"warning",
          message:"请先进行选择"
        })
        return
      }
      request.post("/info/deleteBatch",this.ids).then(res => {
        if (res.code === 200) {
          ElMessage({
            type: 'success',
            message: '批量删除成功',
          })
          this.lode()
        } else {
          ElMessage({
            type: 'error',
            message: res.msg
          })
        }
      })
    },
    handleSelectionChange(val){
      this.ids = val.map(v => v.productId)
    },

    handleSizeChange(pageSize) {//改变每页的个数触发
      this.pageSize = pageSize
      this.lode()

    },
    handleCurrentChange(pageNum) {//改变当前页码触发
      this.currentPage = pageNum
      this.lode()

    },
  },

  data() {
    return {
      dialogVisible: false,
      admins:{},
      search: '',
      currentPage: 1,
      pageSize: 5,
      total: 0,
      tableData: [],
      admin: [],
      ids: [],
    };
  },

};
</script>

<style scoped>
.userindex {
  width: 100%;
  min-height: 100%;
  padding: 10px;
}
/* 搜索 */
.userindex-queryInfo {
  margin-bottom: 10px;
}

.userindex-queryInfo-li {
  width: 100%;
  height: auto;
}
/* 列表 */
.userindex-list {
  width: 100%;
  height: auto;
  margin-bottom: 30px;
}
/* 分页 */
.userindex-page-box {
  width: 100%;
  height: auto;
  display: flex;
  justify-content: center;
}
</style>
