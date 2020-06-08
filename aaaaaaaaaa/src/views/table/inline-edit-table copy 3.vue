<template>
  <div class="app-container">
    <el-table v-loading="listLoading" :data="list" border fit highlight-current-row style="width: 100%" @row-dblclick="rowDblclick">
      <el-table-column align="center" label="ID" width="80">
        <template slot-scope="scope">
          <el-input
            v-if="scope.row.idFlag && scope.row.idfocus"
            v-model="scope.row.id"
            @focus="focusId(scope.row)"
            @blur="onItemBlurId(scope.row)"
          />
          <span v-else>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column width="180px" align="center" label="Date">
        <template slot-scope="scope">
          <el-input
            v-if="scope.row.timestampFlag && scope.row.tmfocus"
            v-model="scope.row.timestamp"
            @focus="focustimestamp(scope.row)"
            @blur="onItemBlurtimestamp(scope.row)"
          />
          <span v-else>{{ scope.row.timestamp | parseTime('{y}-{m}-{d} {h}:{i}') }}</span>
        </template>
      </el-table-column>

      <el-table-column width="120px" align="center" label="Author">
        <template slot-scope="scope">
          <el-input
            v-if="scope.row.authorFlag && scope.row.authorfocus"
            v-model="scope.row.author"
            @focus="focusauthor(scope.row)"
            @blur="onItemBlurauthor(scope.row)"
          />
          <span v-else>{{ scope.row.author }}</span>
        </template>
      </el-table-column>
      <el-table-column class-name="status-col" label="Status" width="110">
        <template slot-scope="{row}">
          {{ row.status }}
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import { fetchList } from '@/api/article'

export default {
  name: 'InlineEditTable',
  filters: {
    statusFilter(status) {
      const statusMap = {
        published: 'success',
        draft: 'info',
        deleted: 'danger'
      }
      return statusMap[status]
    }
  },
  data() {
    return {
      list: null,
      listLoading: true,
      listQuery: {
        page: 1,
        limit: 10
      }
    }
  },
  created() {
    this.getList()
  },
  methods: {
    async getList() {
      this.listLoading = true
      const { data } = await fetchList(this.listQuery)
      const items = data.items
      console.log(items)
      this.list = items.map(v => {
        this.$set(v, 'edit', false) // https://vuejs.org/v2/guide/reactivity.html
        this.$set(v, 'idFlag', false) // https://vuejs.org/v2/guide/reactivity.html
        this.$set(v, 'timestampFlag', false) // https://vuejs.org/v2/guide/reactivity.html
        this.$set(v, 'authorFlag', false) // https://vuejs.org/v2/guide/reactivity.html
        this.$set(v, 'idfocus', false)
        this.$set(v, 'tmfocus', false)
        this.$set(v, 'autherfocus', false)
        v.originalTitle = v.title //  will be used when user click the cancel botton
        return v
      })
      this.listLoading = false
    },
    cancelEdit(row) {
      row.title = row.originalTitle
      row.edit = false
      this.$message({
        message: 'The title has been restored to the original value',
        type: 'warning'
      })
    },
    confirmEdit(row) {
      row.edit = false
      row.originalTitle = row.title
      this.$message({
        message: 'The title has been edited',
        type: 'success'
      })
    },
    rowDblclick(row) {
      console.log(row)
      row.edit = true
      row.idFlag = true
      row.timestampFlag = true
      row.authorFlag = true
      row.idfocus = true
      row.tmfocus = true
      row.authorfocus = true
    },
    onItemBlurId(row) {
      console.log('1')
      row.idFlag = false
    },
    onItemBlurtimestamp(row) {
      console.log('2')
      row.timestampFlag = false
    },
    onItemBlurauthor(row) {
      console.log('3')
      row.authorFlag = false
    },
    focusId(row) {
      console.log('4')
      row.edit = true
      // row.idfocus = true
      // row.tmfocus = true
      row.idfocus = true

      // row.idFlag = true
      // row.timestampFlag = true
      // row.authorFlag = true
    },
    focusauthor(row) {
      row.edit = true
      console.log('5')
      // row.tmfocus = true
      // row.authorfocus = true
      // row.authorfocus = true

      // row.idFlag = true
      // row.timestampFlag = true
      // row.authorFlag = true
    },
    focustimestamp(row) {
      row.edit = true
      console.log('6')
      // row.authorfocus = true
      // row.tmfocus = true
      // row.idFlag = true
      // row.timestampFlag = true
      // row.authorFlag = true
    }
  }
}
</script>

<style scoped>
.edit-input {
  padding-right: 100px;
}
.cancel-btn {
  position: absolute;
  right: 15px;
  top: 10px;
}
</style>
