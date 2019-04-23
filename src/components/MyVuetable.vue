<template>
  <div class="ui container">
	    <div class="vuetable-pagination ui basic segment grid">
		  	<!-- Lección 8 -->
		  	<vuetable-pagination-info ref="paginationInfoTop"
		  	></vuetable-pagination-info>
		  	<!-- Lección 7 -->
		  	<vuetable-pagination ref="paginationTop"
		  		@vuetable-pagination:change-page="onChangePage"
		  	></vuetable-pagination>
		</div>  	
		<vuetable ref="vuetable"
		api-url="https://vuetable.ratiw.net/api/users"
		:fields=fields
		pagination-path=""
		:per-page="20"
    :multi-sort="true"
    multi-sort-key="ctrl"
		@vuetable:pagination-data="onPaginationData"
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
import Vuetable from 'vuetable-2/src/components/Vuetable'
import VuetablePagination from 'vuetable-2/src/components/VuetablePagination'
import VuetablePaginationInfo from 'vuetable-2/src/components/VuetablePaginationInfo'
import Vue from 'vue'
import CustomActions from './CustomActions'

Vue.component('custom-actions', CustomActions)

export default {
  components: {
    Vuetable,
    VuetablePagination,
    VuetablePaginationInfo
  },
  data() {
  	return {
      css: {
          ascendingIcon: 'glyphicon glyphicon-chevron-up',
          descendingIcon: 'glyphicon glyphicon-chevron-down'
      },
  		fields: [/*{
        name: '__sequence', // Mustra el número de secuencia de los registros en función de la información de paginación
        title: '#',
        titleClass: 'center aligned',
        dataClass: 'right aligned'
      }*/
      /*{
        name: '__handle', // Este fue creado específicamente porque necesitamos hacer un icono de manejador para Sortable.js
        dataClass: 'center aligned'
      }*/
      /*{
        name: '__checkbox',
        titleClass: 'center aligned',
        dataClass: 'center aligned'
      }*/
      {
  			name: 'name',
        sortField: 'name'
      }, { 
        name: 'email',
        sortField: 'email',
        direction: 'asc'
      }, {
  				name: 'birthdate',
          sortField: 'birthdate',
  				titleClass: 'center aligned',
  				dataClass: 'center aligned',
  				callback: 'formatDate|DD-MM-YYYY'
  			}, {
  				name: 'nickname',
          sortField: 'nickname',
  				callback: 'allcap'
  			}, {
  				name: 'gender',
          sortField: 'gender',
  				titleClass: 'center aligned',
  				dataClass: 'center aligned',
  				callback: 'genderLabel'
  			}, {
  				name: 'salary',
          sortField: 'salary',
  				titleClass: 'center aligned',
  				dataClass: 'right aligned',
  				callback: 'formatNumber',
          visible: false
  			}, {          
          //name: '__component:custom-actions', // DE esta manera obtenemos las acciones desde el componente que creamos.
          name: '__slot:actions',
          title: 'Actions',
          titleClass: 'center aligned',
          dataClass: 'center aligned'
        } 


  			/* {
  				name: 'address.line1',
  				title: 'Address 1',
  			}, {
  				name: 'address.line2',
  				title: 'Address 2',
  			}, {
  				name: 'address.zipcode',
  				title: 'Zipcode'
  			}*/
  		]
  	}
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
		this.$refs.paginationTop.setPaginationData(paginationData)
		this.$refs.paginationInfoTop.setPaginationData(paginationData)

  		this.$refs.pagination.setPaginationData(paginationData)
  		this.$refs.paginationInfo.setPaginationData(paginationData)
  	},
  	onChangePage (page) {
  		this.$refs.vuetable.changePage(page)
  	},
    onAction (action, data, index) {
      console.log('slot) action: ' + action, data.name, index)
    }
  }
}
</script>