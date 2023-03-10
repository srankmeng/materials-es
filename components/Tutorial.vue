<!-- Please remove this file from your project -->
<template>
  <div class="relative min-h-screen bg-gray-100 p-8">
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.1.2/dist/tailwind.min.css" rel="stylesheet">
    <div class="container p-8 mx-auto bg-gray-200">
     
      <div class="m-16">
        <img class="w-24 h-24 mx-auto ..." src="../static/logo.png">
      </div>
      
      <div class="grid grid-cols-3 gap-4 mb-4">
        <div class="col-span-2 ...">
          <input v-model="searchText" class="appearance-none block w-full border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500" type="text" placeholder="Please type Spec no., Spec issue no., Spec name">
        </div>
      </div>

      <label class="block">
        <span class="block text-sm font-medium text-slate-700">Filter</span>
      </label>
      <div class="grid grid-cols-4 gap-4">
        <div class="...">
          <select v-model="filter.status" class="block appearance-none w-full bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline">
            <option value="">All Status</option>
            <option value="Approved">Approved</option>
            <option value="Draft">Draft</option>
            <option value="Review">Review</option>
            <option value="Archived">Archived</option>
          </select>
        </div>
        <div class="...">
          <select v-model="filter.effectiveDate" class="block appearance-none w-full bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline">
            <option value="">All Effective Date</option>
            <option value="1">11 Nov 2021 - 11 Nov 2022</option>
            <option value="2">11 Nov 2023 - 11 Nov 2024</option>
          </select>
        </div>
        <div class="...">
          <input v-model="filter.crossReference" class="appearance-none block w-full border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500" type="text" placeholder="Corss reference no.">
        </div>
        <div class="...">
          <input v-model="filter.originator" class="appearance-none block w-full border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500" type="text" placeholder="Originator name">
        </div>
      </div>

      <div class="grid grid-cols-4 gap-4">
        <div class="...">
          <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 mr-4 rounded" @click="search">
            Search
          </button>
          <button class="bg-gray-500 hover:bg-gray-700 text-white font-bold py-2 px-4 rounded" @click="clear">
            Clear
          </button>
        </div>
      </div>

      <div class="relative overflow-x-auto mt-4">
        <label class="block text-right mb-4">
          <span class="block text-sm">{{ count.toLocaleString() }} item(s)</span>
        </label>
        <table class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
          <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
            <tr>
              <th scope="col" class="px-6 py-3">
                Spec no.
              </th>
              <th scope="col" class="px-6 py-3">
                Spec issue no.
              </th>
              <th scope="col" class="px-6 py-3">
                Spec name
              </th>
              <th scope="col" class="px-6 py-3">
                Status
              </th>
              <th scope="col" class="px-6 py-3">
                Effective date
              </th>
              <th scope="col" class="px-6 py-3">
                Cross reference
              </th>
              <th scope="col" class="px-6 py-3">
                Originator
              </th>
            </tr>
          </thead>
          <tbody>
            <tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700" v-for="m in materials">
              <td class="px-6 py-4">
                {{ m.spec_no }}
              </td>
              <td class="px-6 py-4">
                {{  m.spec_issue_no }}
              </td>
              <td class="px-6 py-4">
                {{  m.spec_name }}
              </td>
              <td class="px-6 py-4">
                {{  m.status }}
              </td>
              <td class="px-6 py-4">
                {{  m.effective_date }}
              </td>
              <td class="px-6 py-4">
                {{  m.cross_reference }}
              </td>
              <td class="px-6 py-4">
                {{  m.originator }}
              </td>
            </tr>
            <tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700" v-if="!materials.length">
              <td class="px-6 py-4" colspan="7">
                There is no data
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'NuxtTutorial',

  data() {
    return {
      searchText: '',
      filter: {
        status: '',
        effectiveDate: '',
        crossReference: '',
        originator: '',
      },
      count: 0,
      materials: [],
    }
  },

  async fetch() {
    this.search()
  },

  methods: {
    async search() {

      let payloadShould = []
      if (this.searchText) {
        payloadShould = [
          {
            "term": {
              "spec_no": this.searchText.toLowerCase()
            }
          },
          {
            "match_phrase": {
              "spec_issue_no": this.searchText.toLowerCase()
            }
          },
          {
            "term": {
              "spec_name": this.searchText.toLowerCase()
            }
          },
        ]
      }

      let payloadFilter = []
      if (this.filter.status) {
        payloadFilter = [
          ...payloadFilter,
          {
            term: {
              status: this.filter.status
            }
          }
        ]
      }
      if (this.filter.effectiveDate === '1') {
        payloadFilter = [
          ...payloadFilter,
          {
            range: {
              effective_date: {
                gte: "01-Nov-21 12.00.00 AM",
                lte: "01-Nov-22 12.00.00 AM"
              }
            }
          }
        ]
      }
      if (this.filter.effectiveDate === '2') {
        payloadFilter = [
          ...payloadFilter,
          {
            range: {
              effective_date: {
                gte: "01-Nov-23 12.00.00 AM",
                lte: "01-Nov-24 12.00.00 AM"
              }
            }
          }
        ]
      }
      if (this.filter.crossReference) {
        payloadFilter = [
          ...payloadFilter,
          {
            term: {
              cross_reference: this.filter.crossReference
            }
          }
        ]
      }
      if (this.filter.originator) {
        payloadFilter = [
          ...payloadFilter,
          {
            term: {
              originator: this.filter.originator.toLowerCase()
            }
          }
        ]
      }

      // prepare payload
      let payload = {
        query: {
          bool: {}
        }
      }
      if (payloadShould.length) {
        payload.query.bool = {
          ...payload.query.bool,
          should: payloadShould,
          minimum_should_match: 1,
        }
      }
      if (payloadFilter.length) {
        payload.query.bool = {
          ...payload.query.bool,
          filter: payloadFilter
        }
      }

      // const res = await this.$axios.post(`http://localhost:8080/${this.$config.apiUrl}/api/console/proxy?path=materials%2F_search&method=GET`,
      const res = await this.$axios.post(`${this.$config.apiUrl}/materials/_search`,
        {
          from: 0, 
          size: 100,
          ...payload
        }, 
        {
          auth: {
            username: this.$config.elasticUsername,
            password: this.$config.elasticPassword,
          }
          // headers: {
          //   Authorization: `ApiKey ${this.$config.apiKey}`,
          //   'kbn-xsrf': 'true'
          // }
        })
      this.materials = res?.data.hits.hits.map(item => {
        const obj = item._source
        return {
          ...obj,
        }
      }) || []

      // get total count result
      // const resCount = await this.$axios.post(`http://localhost:8080/${this.$config.apiUrl}/api/console/proxy?path=materials%2F_count&method=GET`,
      const resCount = await this.$axios.post(`${this.$config.apiUrl}/materials/_count`,
        {
          ...payload
        }, 
        {
          auth: {
            username: this.$config.elasticUsername,
            password: this.$config.elasticPassword,
          }
          // headers: {
          //   Authorization: `ApiKey ${this.$config.apiKey}`,
          //   'kbn-xsrf': 'true'
          // }
        })
      this.count = resCount?.data.count || 0
    },

    async clear() {
      this.searchText = ''
      this.filter = {
        status: '',
        effectiveDate: '',
        crossReference: '',
        originator: '',
      }
      await this.search()
    }
  }
}
</script>