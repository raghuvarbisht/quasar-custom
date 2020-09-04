<template>
    <!-- table header sort icon update
      https://github.com/quasarframework/quasar/issues/5065
      https://quasar.dev/options/quasar-icon-sets#Installing-a-Quasar-Icon-Set
      -->
    <!-- Added selection="multiple" above -->
    <!-- Added :selected.sync above -->
    <div class="q-pa-md qcustom-table">
      <q-table
        v-if="columns.length > 0"
        ref="myTable"
        :data="data"
        :columns="columns"
        row-key="id"
        :hide-bottom="data.length > 0"
        @row-click="onRowClick"
        selection="multiple"
        :selected.sync="selectedRows"
        style="width:900px;">
        <!-- slot for table header section start-->
          <template v-slot:header="props">
            <q-tr :props="props">              
              <q-th v-if="expansionFlag" ></q-th>
              <q-th align-left 
                :class="multiSelectOption" 
                v-if="multiSelectOption || singleSelectOption">
                <q-checkbox v-model="props.selected" v-if="multiSelectOption"/>
              </q-th>
              <q-th 
                v-for="col in props.cols" 
                :class="col.class !== undefined ? col.class : ''" 
                :key="col.name" 
                :props="props" 
                @click="sortData">
                {{ col.label }}
              </q-th>
              <q-th v-if="actionFlag"></q-th>
            </q-tr>
          </template>
        <!-- slot for table header end -->

        <!-- slot for table body/row section start-->
          <template v-slot:body="props">
            <q-tr :props="props">
              <q-td auto-width v-if="expansionFlag">
                <q-icon color="accent" size="1rem" @click="props.expand = !props.expand">
                  <svg v-if="props.expand" xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 16 16">
                      <g fill="none" fill-rule="evenodd">
                        <path d="M0 0H16V16H0z"/>
                        <path fill="#19202D" d="M12.24 11c.676 0 1.014-.853.536-1.35L8.537 5.23c-.297-.308-.777-.308-1.074 0L3.223 9.65C2.747 10.147 3.085 11 3.76 11h8.48z"/>
                      </g>
                  </svg>
                  <svg v-else xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 16 16">
                      <g fill="none" fill-rule="evenodd">
                        <path d="M0 0H16V16H0z"/>
                        <path fill="#19202D" d="M3.76 5c-.676 0-1.014.853-.536 1.35l4.239 4.417c.297.31.777.31 1.074 0l4.24-4.416C13.253 5.853 12.915 5 12.24 5H3.76z"/>
                      </g>
                  </svg>
                </q-icon>
              </q-td>
              <q-td align-center v-if="multiSelectOption || singleSelectOption">
               <q-checkbox v-model="props.selected" dense/>
              </q-td>
              <q-td
                v-for="col in props.cols"
                :key="col.name"
                :props="props">
                <template v-if="editFlag === props.row.id && col.type !== undefined" >
                  <q-input outlined v-model="col.value" v-if="col.type === 'textbox'"/>
                  <q-select filled v-model="col.value" :options="options" v-if="col.type === 'dropdown'" />
                </template>
                <template v-else>
                  {{ col.value }}
                </template>
              </q-td>
              <!-- action column start -->
              <q-td v-if="actionFlag">
                <template v-if="editFlag === props.row.id">
                  <q-btn outline rounded color="primary" label="Save" @click="save(props.row)"/>
                  <q-btn outline rounded color="secondary" label="Cancel" @click="cancel(props.row)"/>
                </template>
                <template v-else>
                  <q-btn outline rounded color="primary" label="Edit" @click="edit(props.row)"/>
                  <q-btn outline rounded color="secondary" label="Delete" @click="delete(props.row)"/>
                </template>
              </q-td>
              <!-- action column end -->
            </q-tr>
            <!-- Expand section start -->
            <q-tr v-show="props.expand" :props="props">
              <q-td colspan="100%">
                <div class="text-left">Exapnded {{ props.row.name }} {{ props.row.id }}.</div>
              </q-td>
            </q-tr>
            <!-- Expand section end -->
        </template>
      <!-- slot for table body/row section end-->

      <!-- slot for table bottom/footer section start-->
        <template v-slot:bottom></template>
      <!-- slot for table bottom/footer section end-->
      </q-table>
      {{selectedRows}}
    </div>
</template>
<script>

export default {
  name: 'QCustomTable',
  props: {
    multiSelectOption: { // multiselect select checkbox
      default: false
    },
    singleSelectOption: { // single select checkbox
      default: false
    },
    expansionFlag: {
      default: false
    },
    actionFlag: {
      default: false
    },
    fieldDetails: {
      default: Array
    },
    dataSetDetails: {
      default: Array
    }

  },
  data () {
    return {
     // options: [{ label: 'Admin' , value: 'Admin' }, { label: 'Analyst' , value: 'Analyst' }],
      options: ['Admin' , 'Analyst'],
      selectedRows: [],
      editFlag: null,
      columns: this.fieldDetails,
      data: this.dataSetDetails
    }
  },
  computed: {

  },
  filters: {
    capitalize (value) {
      if (!value) return ''
      value = value.toString()
      return `${value.charAt(0).toUpperCase()}${value.slice(1)}`
    }
  },

  methods: {
    getSelectedString () {
      return this.selected.length === 0 ? '' : `${this.selected.length} record${this.selected.length > 1 ? 's' : ''} selected of ${this.data.length}`
    },
    edit (row, flag) {
      console.log('==edit==' + JSON.stringify(row))
      this.editFlag = row.id
    },
    cancel (row) {
      this.editFlag = null
      console.log(JSON.stringify(row))
    },
    delete (row) {
      alert('delete row', row.id)
      console.log(JSON.stringify(row))
    },
    save (row) {
      this.editFlag = null
      console.log(JSON.stringify(row))
    },
    onRowClick (evt, row) {
      console.log('clicked on', JSON.stringify(row))
    },
    editClick () {
      console.log('calling editClick')
    },
    sortData () {
      console.log('sort data')
    }

  }
}
</script>
