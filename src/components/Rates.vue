<template>
  <div class="rates">
    <h1>{{ msg }}</h1>
    <p>Rates</p>
    <div id="table">
        <div class="mt-3 mb-3" size="sm">
            <b-input v-model="keyword" placeholder="Search" type="text"></b-input>
            <br>
        </div>
        <b-table
                :data="filteredNames"
                :paginated="isPaginated"
                :per-page="perPage"
                :current-page.sync="currentPage"
                :pagination-simple="isPaginationSimple"
                :pagination-position="paginationPosition"
                :default-sort-direction="defaultSortDirection"
                :sort-icon="sortIcon"
                :sort-icon-size="sortIconSize"
                default-sort="user.name"
                aria-next-label="Next page"
                aria-previous-label="Previous page"
                aria-page-label="Page"
                aria-current-label="Current page">

                <template slot-scope="props">
                    <b-table-column field="symbol" label="Symbol" width="40" sortable numeric>
                        {{ props.row.symbol }}
                    </b-table-column>

                    <b-table-column field="name" label="Name" sortable>
                        {{ props.row.name }}
                    </b-table-column>

                    <b-table-column field="rate" label="Rate" sortable>
                        {{ props.row.rate }}
                    </b-table-column>

                    <b-table-column field="" label="Pin">
                        <a v-on:click="move(props.row)">
                            <b-icon
                                v-if="(props.index + 1) == 1"
                                icon="star"
                                size="is-small">
                            </b-icon>
                            <b-icon
                                v-if="(props.index + 1) != 1"
                                icon="star-outline"
                                size="is-small">
                            </b-icon>
                        </a>
                    </b-table-column>
                </template>
            </b-table>
        </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'Rates',
  props: {
    msg: String,
  },
  data() {
    return {
      posts: [],
      columns: [],
      errors: [],
      filterName: '',
      isPaginated: true,
      isPaginationSimple: false,
      paginationPosition: 'bottom',
      defaultSortDirection: 'asc',
      sortIcon: 'arrow-up',
      sortIconSize: 'is-small',
      currentPage: 1,
      perPage: 5,
      keyword: ''
    }
  },
     columns() {
      this.columns = [
                    {
                        field: 'name',
                        label: 'Name',
                        searchable: true,
                    },
                    {
                        field: 'symbol',
                        label: 'Symbol',
                        searchable: true,
                    },
                    {
                        field: 'rate',
                        label: 'Rates',
                        searchable: true,
                    }
                ]
  },
     created() {
        axios.get(`https://r.easycrypto.nz/json/backenddb.json`)
        .then(response => {
            Object.keys(response.data).forEach((key) => {
                // console.log(response.data[key].name);
                this.posts.push({name:response.data[key].name,
                            symbol:response.data[key].symbol,
                            rate:String(response.data[key].rate)});
            })
        }).catch(e => {
            this.errors.push(e)
        })
    },
    methods: {
        move(item){
            var index = this.posts.indexOf(item);
            this.posts.splice(index, 1);
            this.posts.unshift(item);
        }
    },
	computed: {
		filteredNames() {
            return this.posts.filter((item) => {
                return (item.name.includes(this.keyword) || item.symbol.includes(this.keyword) || item.rate.includes(this.keyword));
            })
        }
        
	}
}
</script>
<style>
#table {
  margin: 0 auto;
  width: 500px;
}
#table .input-group-text {
  padding: 0 .5em 0 .5em;
}
#table .input-group-text .fa {
  font-size: 12px;
}
/* .table-mobile-sort{
    display:none;
} */
</style>