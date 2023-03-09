<!-- Please remove this file from your project -->
<template>
  <div class="relative flex items-top justify-center min-h-screen bg-gray-100 sm:items-center sm:pt-0">
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.1.2/dist/tailwind.min.css" rel="stylesheet">
    <div class="mx-auto p-8 bg-gray-200">
     
      <div class="m-16">
        <img class="w-24 h-24 mx-auto ..." src="../static/logo.png">
      </div>
      
      <div class="grid grid-cols-3 gap-4 mb-4">
        <div class="col-span-2 ...">
          <input class="appearance-none block w-full border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500" type="text" placeholder="Please type Spec no., Spec issue no., Spec name">
        </div>
      </div>

      <label class="block">
        <span class="block text-sm font-medium text-slate-700">Filter</span>
      </label>
      <div class="grid grid-cols-4 gap-4">
        <div class="...">
          <select class="block appearance-none w-full bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline">
            <option>All Status</option>
            <option>Approve</option>
            <option>Draft</option>
          </select>
        </div>
        <div class="...">
          <select class="block appearance-none w-full bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline">
            <option>All Effective Date</option>
            <option>11 Nov 2011 - 11 Nov 2022</option>
          </select>
        </div>
        <div class="...">
          <input class="appearance-none block w-full border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500" type="text" placeholder="Corss reference no.">
        </div>
        <div class="...">
          <input class="appearance-none block w-full border border-gray-200 rounded py-3 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500" type="text" placeholder="Originator name">
        </div>
      </div>

      <div class="grid grid-cols-4 gap-4">
        <div class="...">
          <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded" @click="search">
            Search
          </button>
        </div>
      </div>

      <div class="relative overflow-x-auto mt-16">
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
      materials: [],
    }
  },

  methods: {
    async search() {
      const res = await this.$axios.get(`${this.$config.apiUrl}/users`)
      this.materials = res?.data || []
    },
  }
}
</script>
