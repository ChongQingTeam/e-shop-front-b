<template>
<div>
    <Table border ref="selection" :columns="columns12" :data="data6">
        <template slot-scope="{ row }" slot="name">
            <strong>{{ row.name }}</strong>
        </template>
        <template slot-scope="{ row, index }" slot="action">
            <Button type="primary" size="small" style="margin-right: 5px" @click="show(index)">View</Button>
            <Button type="error" size="small" @click="remove(index)">Delete</Button>
        </template>
    </Table>
    <Button @click="selectAll(true)" >全选</Button>
    <Button @click="selectAll(false)" >全不选</Button>
    <Button @click="testQuery()">测试查询</Button>

    <Page :total=4 show-sizer   show-elevator></Page>

</div>
</template>
<script>
import { testQuery1 } from '@/api/data'
export default {
  data () {
    return {
      columns12: [
        {
          type: 'selection',
          width: 60,
          align: 'center'
        },
        {
          title: 'adminName',
          key: 'adminName'
          // slot: 'name'
        },
        {
          title: 'mobile',
          key: 'mobile',
          sortable: true
        },
        {
          title: 'Address',
          key: 'address',
          render: (h, params) => {
            return h('div', [
              h('Icon', {
                props: {
                  type: 'md-pin'
                }

              }),
              h('strong', params.row.address)
            ]
            )
          }
        },
        {
          title: 'Action',
          slot: 'action',
          width: 150,
          align: 'center'
        }
      ],
      data6: []
    }
  },
  methods: {
    show (index) {
      this.$Modal.info({
        title: 'User Info',
        content: `Name：${this.data6[index].name}<br>Age：${this.data6[index].age}<br>Address：${this.data6[index].address}`
      })
    },
    remove (index) {
      this.data6.splice(index, 1)
    },
    selectAll (status) {
      this.$refs.selection.selectAll(status)
    },
    testQuery () {
      var that = this
      var tmp = []
      debugger
      testQuery1().then(res => {
        const data = res.data
        if (data.status === '200') {
          tmp.push(data.result)
          that.data6 = tmp
        }
        resolve(res)
      }).catch(err => {
        reject(err)
      })
    }

  }
}
</script>
