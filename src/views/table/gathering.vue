<template>
  <section>
    
    <el-row>
      <el-col :span="24">
        <div class="block">
            <!--查询表单-->
            <el-form :inline="true" class="demo-form-inline">
                <el-form-item label="活动名称">
                <el-input v-model="searchMap.name" placeholder="活动名称"></el-input>
                </el-form-item>
                <el-form-item label="活动日期" >
                <el-date-picker  type="date" placeholder="选择开始日期" v-model="searchMap.starttime_1" ></el-date-picker>
                <el-date-picker  type="date" placeholder="选择截止日期" v-model="searchMap.starttime_2" ></el-date-picker>
                </el-form-item>
                <el-button type="primary" @click="fetchData()">查询</el-button>
            </el-form>
            <!--表格-->
            <el-table
            :data="list"
            border
            style="width: 100%">
            <el-table-column type="selection">
            </el-table-column>
            <el-table-column
                prop="id"
                label="ID"
                width="80">
            </el-table-column>
            <el-table-column
                prop="name"
                label="活动名称"
                width="180">
            </el-table-column>
            <el-table-column
                prop="address"
                label="活动地址">
            </el-table-column>
            <el-table-column
                prop="starttime"
                label="开始日期">
            </el-table-column>
            <el-table-column
                prop="endtime"
                label="截止日期">
            </el-table-column>
            <el-table-column label="操作">
            </el-table-column>
            </el-table>

          <el-pagination
            @current-change="fetchData"
            :current-page="currentPage"
            :page-size="10"
            layout="prev, pager, next, jumper"
            :total="total">
          </el-pagination>
        </div>
      </el-col>
    </el-row>
  </section>
</template>

<script>
  import gathering from '@/api/gathering'

  export default {
    data() {
      return {
        list: null,
        listLoading: true,
        total: 0,
        currentPage: 1,
        searchMap: {}
      }
    },
    created() {
      this.fetchData()
    },
    methods: {
      fetchData() {
        this.listLoading = true
        gathering.search(this.currentPage,10,this.searchMap).then(response => {
          if(response.flag === true){
            this.list = response.data.rows
            this.total = response.data.total
          }
          this.listLoading = false
        })
      }
    }
  }
</script>