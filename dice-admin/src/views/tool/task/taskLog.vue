<template>
  <div class="app-container">
    <div class="block">
      <el-row>
        <el-col :span="24">
          <el-button icon="el-icon-back" @click.native="back">返回</el-button>
        </el-col>
      </el-row>
    </div>
    <el-table :data="list" element-loading-text="Loading" border fit highlight-current-row>

      <el-table-column label="任务名">
        <template slot-scope="scope">
          {{ scope.row.name }}
        </template>
      </el-table-column>
      <el-table-column label="执行时间">
        <template slot-scope="scope">
          {{ scope.row.execAt }}
        </template>
      </el-table-column>
      <el-table-column label="执行结果">
        <template slot-scope="scope">
          {{ scope.row.execSuccess === 1 ? '成功' : '失败' }}
        </template>
      </el-table-column>
      <el-table-column label="异常信息">
        <template slot-scope="scope">
          {{ scope.row.jobExecption }}
        </template>
      </el-table-column>

    </el-table>

    <el-pagination
      background
      layout="total, sizes, prev, pager, next, jumper"
      :page-sizes="[10, 20, 50, 100,500]"
      :page-size="listQuery.limit"
      :total="total"
      @size-change="changeSize"
      @current-change="fetchPage"
      @prev-click="fetchPrev"
      @next-click="fetchNext"
    >
    </el-pagination>
  </div>
</template>

<script>
import { logList } from '@/api/task'

export default {
  data() {
    return {
      taskId: '',
      listQuery: {
        page: 1,
        limit: 20,
        taskId: undefined
      },
      total: 0,
      list: null,
      selRow: {}
    }
  },
  created() {
    this.init()
  },
  methods: {
    init() {
      this.listQuery.taskId = this.$route.query.taskId
      this.fetchData()
    },
    fetchData() {
      logList(this.listQuery).then(response => {
        this.list = response.data.list
        this.total = response.data.total
      })
    },
    fetchNext() {
      this.listQuery.page = this.listQuery.page + 1
      this.fetchData()
    },
    fetchPrev() {
      this.listQuery.page = this.listQuery.page - 1
      this.fetchData()
    },
    fetchPage(page) {
      this.listQuery.page = page
      this.fetchData()
    },
    changeSize(limit) {
      this.listQuery.limit = limit
      this.fetchData()
    },
    back() {
      this.$router.go(-1)
    }

  }
}

</script>

