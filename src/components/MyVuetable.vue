<template>
  <div class="ui container">
      
    <div class="vuetable-pagination ui basic segment grid">
      <filter-bar></filter-bar>
	  </div>  	
		<vuetable ref="vuetable"
		:api-url="apiUrl"
		:fields="fields"
		pagination-path=""
		:per-page="10"
    :multi-sort="true"
    :sort-order="sortOrder"
    multi-sort-key="ctrl"
		@vuetable:pagination-data="onPaginationData"
    :append-params="appendParams"
    detail-row-component="detailRowComponent"
		>
    <template slot="actions" slot-scope="props">
      <div class="custom-actions">
        <button class="ui basic button"
          @click="onAction('view-item', props.rowData, props.rowIndex)">
          <i class="zoom icon"></i>
        </button>
        <button class="ui basic button"
          @click="onAction('edit-item', props.rowData, props.rowIndex)">
          <i class="edit icon"></i>
        </button>
        <button class="ui basic button"
          @click="onAction('delete-item', props.rowData, props.rowIndex)">
          <i class="delete icon"></i>
        </button>
      </div>
    </template>    

    </vuetable>
		<div class="vuetable-pagination ui basic segment grid">
			<!-- Lección 8 -->
			<vuetable-pagination-info ref="paginationInfo"
			></vuetable-pagination-info>
			<!-- Lección 7 -->
			<vuetable-pagination ref="pagination"
				@vuetable-pagination:change-page="onChangePage"
			></vuetable-pagination>
		</div>
  </div>
</template>

<script>
import accounting from 'accounting'
import moment from 'moment'
import Vue from 'vue'
import VueEvents from 'vue-events'
import Vuetable from 'vuetable-2/src/components/Vuetable'
import VuetablePagination from 'vuetable-2/src/components/VuetablePagination'
import VuetablePaginationInfo from 'vuetable-2/src/components/VuetablePaginationInfo'
import CustomActions from './CustomActions'
import FilterBar from './FilterBar'

Vue.use(VueEvents)
Vue.component('custom-actions', CustomActions)
Vue.component('filter-bar', FilterBar)


export default {
  components: {
    Vuetable,
    VuetablePagination,
    VuetablePaginationInfo
  },
props: {
    apiUrl: {
      type: String,
      required: true
    },
    fields: {
      type: Array,
      required: true
    },
    sortOrder: {
      type: Array,
      default() {
        return []
      }
    },
    appendParams: {
      type: Object,
      default() {
        return {}
      }
    },
    detailRowComponent: {
      type: String
    }
  },  
  data() {
  	return {}
  },
  mounted() {
    this.$events.$on('filter-set', eventData => this.onFilterSet(eventData))
    this.$events.$on('filter-reset', e => this.onFilterReset())
  },  
  methods: {
  	allcap (value) {
  		return value.toUpperCase()
  	},
  	genderLabel (value) {
  		return value === 'M'
  		? '<span class="ui teal label"><i class="large man icon"></i>Male</span>'
  		: '<span class="ui pink label"><i class="large woman icon"></i>Female</span>'
  	},
  	formatNumber (value) {
  		return accounting.formatNumber(value, 2)
  	},
  	formatDate (value, fmt = 'D MMM YYYY') {
  		return (value == null)
  		? ''
  		: moment(value, 'YYYY-MM-DD').format(fmt)
  	},
  	onPaginationData (paginationData) {

  		this.$refs.pagination.setPaginationData(paginationData)
  		this.$refs.paginationInfo.setPaginationData(paginationData)
  	},
  	onChangePage (page) {
  		this.$refs.vuetable.changePage(page)
  	},
    onAction (action, data, index) {
      console.log('slot) action: ' + action, data.name, index)
    },
    onFilterSet (filterText) {
      this.appendParams = {
        'filter': filterText
      }
      Vue.nextTick(() => this.$refs.vuetable.refresh())
      console.log('filter-set', 'filterText')      
    },
    onFilterReset () {
      this.appendParams = {}
      Vue.nextTick(() => this.$refs.vuetable.refresh())
      console.log('filter-reset')
    }
  }
}
</script>