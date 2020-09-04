<template>
  <q-layout view="lHh Lpr lFf">
    <QCustomTable
      :multiSelectOption="multiSelectOption"
      :singleSelectOption="singleSelectOption"
      :expansionFlag="expansionFlag"
      :actionFlag="actionFlag"
      :fieldDetails="fieldDetails"
      :dataSetDetails="dataSetDetails"/>

  </q-layout>
</template>

<script>
import QCustomTable from 'components/q-custom-table.vue'

export default {
  name: 'MainLayout',
  components: { QCustomTable },
  data () {
    return {
      tabSelected: '',
      tabItems: [],
      multiSelectOption: true,
      singleSelectOption: true,
      expansionFlag: true,
      actionFlag: true,
      fieldDetails: [
        {
          name: 'name',
          required: true,
          label: 'User name',
          align: 'left',
          field: row => row.name,
          /*format: val => `${val}`,*/
          sortable: true,
          type: 'textbox',
          class: ''
        },
        { name: 'usertype', align: 'right', label: 'UserType', field: 'usertype', sortable: true, type: 'dropdown' },
        { name: 'age', align: 'right', label: 'Age', field: 'age', sortable: true, type: 'textbox' }
      ],
      dataSetDetails: [
        {
          id: 1,
          name: 'Amit Kumar',
          usertype: 'Analyst',
          age: 32
        },
        {
          id: 2,
          name: 'Sumit Singh',
          usertype: 'Admin',
          age: 28
        },
        {
          id: 3,
          name: 'John doe',
          usertype: 'Analyst',
          age: 42
        },
        {
          id: 4,
          name: 'Jay Singh',
          usertype: 'Analyst',
          age: 22
        },
        {
          id: 5,
          name: 'Rohit Singh',
          usertype: 'Admin',
          age: 23
        }
      ],
      selection: 'ACTIVE/All',
      studies: []
    }
  }
}
</script>
