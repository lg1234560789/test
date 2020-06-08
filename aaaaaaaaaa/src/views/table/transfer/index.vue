<template>
  <div class="app-container" @click.self="original">
    <el-row :gutter="20">
      <el-col :span="10">
        <div class="table-title">
          <span class="spanheader">biao</span>
          <el-input class="input" />
        </div>

        <el-table
          ref="staffTable"
          :key="tableKey"
          v-loading="listLoading"
          height="400px"
          :data="staffList"
          border
          fit
          highlight-current-row
          @selection-change="handleStaffChange"
        >
          <el-table-column type="selection" :reserve-selection="true" width="55" />
          <el-table-column label="手机" align="center">
            <template slot-scope="{row}">
              <span>{{ row.phone }}</span>
            </template>
          </el-table-column>

          <el-table-column label="昵称" align="center">
            <template slot-scope="{row}">
              <span>{{ row.nickName }}</span>
            </template>
          </el-table-column>
        </el-table>
      </el-col>
      <el-col :span="2" style="text-align:center;margin:160px auto">
        <el-button
          type="primary"
          :disabled="!staffData.length"
          icon="el-icon-arrow-right"
          circle
          @click="addStaff"
        />
        <el-button
          type="primary"
          :disabled="!selectedStaffData.length"
          icon="el-icon-arrow-left"
          circle
          style="margin-left: 0;margin-top: 10px;"
          @click="removeStaff"
        />
      </el-col>
      <el-col :span="10">
        <div>
          <span class="spanheader">biao</span>
          <el-input class="input" />
        </div>
        <el-table
          ref="selectedStaffTable"
          :key="tableKey"
          v-loading="listLoading"
          height="400px"
          :data="selectedStaffList"
          border
          fit
          highlight-current-row
          @selection-change="handleSelectedStaffChange"
        >
          <el-table-column type="selection" :reserve-selection="true" width="55" />
          <el-table-column label="手机" align="center">
            <template slot-scope="{row}">
              <span>{{ row.phone }}</span>
            </template>
          </el-table-column>

          <el-table-column label="昵称" align="center">
            <template slot-scope="{row}">
              <span>{{ row.nickName }}</span>
            </template>
          </el-table-column>
        </el-table>
      </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  // name: 'Trasfer',
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
      listLoading: false,
      staffTemp: {
        phone: '',
        nickName: '',
        staffTypeId: ''
      },
      staffList: [{ 'phone': 1, nickName: 'liuguan' }, { 'phone': 2, nickName: 'liuguan2' }],
      selectedStaffList: [],
      staffData: [],
      selectedStaffData: [],
      tableKey: 0,
      rowKey: 'rowKey',
      staffOptions: [
        { key: 28, display_name: '补货员' },
        { key: 29, display_name: '测试员' }
      ]
    }
  },
  methods: {
    // 从后台获取左边表格的数据
    getStaffList() {
    //   fetchStaffList(this.staffTemp).then(res => {
    //     if (res.value.staff.length === 0) {
    //       alert('查无此人')
    //     }
    //     this.staffList = res.value.staff
    //   })
      this.staffList = [{ 'phone': 1, nickName: 'liuguan' }, { 'phone': 2, nickName: 'liuguan2' }]
    },
    // 将左边表格选择项存入staffData中
    handleStaffChange(rows) {
      this.staffData = []
      if (rows) {
        rows.forEach(row => {
          if (row) {
            this.staffData.push(row)
          }
        })
      }
    },
    // 左边表格选择项移到右边
    addStaff() {
      setTimeout(() => {
        this.$refs['staffTable'].clearSelection()
        this.$refs['selectedStaffTable'].clearSelection()
      }, 0)
      let repeat = false
      this.selectedStaffList.forEach(item => {
        if (this.staffData[0] && item.phone === this.staffData[0].phone) {
          repeat = true
          alert('此员工已添加')
          return
        }
      })
      if (repeat === false) {
        this.staffData.forEach(item => {
          this.selectedStaffList.push(item)
        })
        for (let i = 0; i < this.staffList.length; i++) {
          for (let j = 0; j < this.staffData.length; j++) {
            if (
              this.staffList[i] &&
              this.staffData[j] &&
              this.staffList[i].phone === this.staffData[j].phone
            ) {
              this.staffList.splice(i, 1)
            }
          }
        }
      }
    },
    // 右边表格选择项移到左边
    removeStaff() {
      setTimeout(() => {
        this.$refs['staffTable'].clearSelection()
        this.$refs['selectedStaffTable'].clearSelection()
      }, 0)
      this.selectedStaffData.forEach(item => {
        this.staffList.push(item)
      })
      for (let i = 0; i < this.selectedStaffList.length; i++) {
        for (let j = 0; j < this.selectedStaffData.length; j++) {
          if (
            this.selectedStaffList[i] &&
            this.selectedStaffData[j] &&
            this.selectedStaffList[i].phone === this.selectedStaffData[j].phone
          ) {
            this.selectedStaffList.splice(i, 1)
          }
        }
      }
    },
    // 将右边表格选择项存入selectedStaffData中
    handleSelectedStaffChange(rows) {
      this.selectedStaffData = []
      if (rows) {
        rows.forEach(row => {
          if (row) {
            this.selectedStaffData.push(row)
          }
        })
      }
    }
    // 提交
    // modifyStaff() {
    //   let isEmpty = false
    //   this.selectedStaffList.forEach(item => {
    //     if (!item.staffTypeId) {
    //       alert('请选择类型')
    //       isEmpty = true
    //       return
    //     }
    //   })
    //   if (isEmpty === false) {
    //     editStaff(this.selectedStaffList, this.deviceQuery.id).then(res => {
    //       this.staffListVisible = false
    //       this.$notify({
    //         title: '成功',
    //         message: '修改成功',
    //         type: 'success',
    //         duration: 2000
    //       })
    //     })
    //   }
    // }
  }
}
</script>

<style scoped>
@import './style.scss'
/* .edit-input {
  padding-right: 100px;
}
.cancel-btn {
  position: absolute;
  right: 15px;
  top: 10px;
}
.spanheader{
    height: 40px;
    /* line-height: 40px; */
    /* background: #f5f7fa;
    margin: 0;
    padding-left: 15px;
    border-bottom: 1px solid #ebeef5;
    box-sizing: border-box;
    color: #000;
} */
</style>
