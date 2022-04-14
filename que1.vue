<template>
  <b-container fluid>
    <!-- User Interface controls -->

    <b-form-input v-model="text" placeholder="Enter country" required>
    </b-form-input>

    <b-button variant="primary" @click="getData">Get Data</b-button>

    <b-row>
      <b-col lg="6" class="my-1">
        <b-form-group
          label="Filter"
          label-for="filter-input"
          label-cols-sm="3"
          label-align-sm="right"
          label-size="sm"
          class="mb-0"
        >
          <b-input-group size="sm">
            <b-form-input
              id="filter-input"
              v-model="filter"
              type="search"
              placeholder="Type to Search"
            ></b-form-input>

            <b-input-group-append>
              <b-button :disabled="!filter" @click="filter = ''"
                >Clear</b-button
              >
            </b-input-group-append>
          </b-input-group>
        </b-form-group>
      </b-col>

      <b-col lg="6" class="my-1">
        <b-form-group
          v-model="sortDirection"
          label="Filter On"
          description="Leave all unchecked to filter on all data"
          label-cols-sm="3"
          label-align-sm="right"
          label-size="sm"
          class="mb-0"
          v-slot="{ ariaDescribedby }"
        >
          <b-form-checkbox-group
            v-model="filterOn"
            :aria-describedby="ariaDescribedby"
            class="mt-1"
          >
            <b-form-checkbox value="name">Name</b-form-checkbox>

            <b-form-checkbox value="domains">Domains</b-form-checkbox>

            <b-form-checkbox value="state-province"
              >state_province</b-form-checkbox
            >

            <b-form-checkbox value="web_pages">web_pages</b-form-checkbox>
          </b-form-checkbox-group>
        </b-form-group>
      </b-col>
    </b-row>

    <!-- Main table element -->
    <b-container class="bv-example-row">
    <b-card>
  <b-card-text v-for="data1 in posts" :key=data1.id>
  <b-card>
<b-row>
<b-col>University name:{{data1.name}}</b-col>
</b-row>
<div class="w-100"></div>
<b-row>
<b-col>Domain:{{data1.domains}}</b-col>
<b-col>State-province:{{data1.state-province}}</b-col>
<b-col><a :href='data1.web_pages' @click="redirect(data1.web_pages)">website_url:--{{ data1.web_pages }}</a></b-col>
<!--<b-col>Web-pages{{data1.web_pages}}</b-col>-->
</b-row>
</b-card>
</b-card-text>
</b-card>
</b-container>-->

    <b-table

:items="posts"

:fields="fields"

:filter="filter"

:filter-included-fields="filterOn"

stacked="md"

show-empty

small

@filtered="onFiltered"

>

<template #cell(actions)="row">

<b-button size="sm" @click="info(row.item, row.index, $event.target)" class="mr-1">

Info modal

</b-button>

<b-button size="sm" @click="row.toggleDetails">

{{ row.detailsShowing ? 'Hide' : 'Show' }} Details

</b-button>

</template>


<template #row-details="row">

<b-card>

<ul>

<li v-for="(value, key) in row.item" :key="key">{{ key }}: {{ value }}</li>

</ul>

</b-card>

</template>

</b-table>

    <!-- <b-container class="bv-example-row" >


<b-card>


<b-card-text :items="posts"

:fields="fields"

:filter="filter"

:filter-included-fields="filterOn"

stacked="md"

show-empty

small

@filtered="onFiltered" >




<b-card>


<b-row >


<b-col cols="2.5">University name:{{data1.name}}</b-col>


</b-row>


<div class="w-100"></div>


<b-row >


<b-col cols="3.5">Domain:{{data1.domains}}</b-col>




<b-col cols="4">Web-pages{{data1.web_pages}}</b-col>


</b-row>


</b-card>


</b-card-text>


</b-card>


</b-container>-->

    <!-- Info modal -->

    <b-modal
      :id="infoModal.id"
      :title="infoModal.title"
      ok-only
      @hide="resetInfoModal"
    >
      <pre>{{ infoModal.content }}</pre>
    </b-modal>
  </b-container>
</template>


<script>
export default {
  name: "QuE1",

  data() {
    return {
      posts: "",

      fields: ["name", "domains", "state-province", "web_pages", "country"],

      totalRows: 1,

      filter: null,

      filterOn: [],

      infoModal: {
        id: "info-modal",

        title: "",

        content: "",
      },
    };
  },

  computed: {},

  mounted() {
    // Set the initial number of items

    this.totalRows = this.items.length;
  },

  methods: {
    /*info(item, index, button) {

this.infoModal.title = `Row index: ${index}`

this.infoModal.content = JSON.stringify(item, null, 2)

this.$root.$emit('bv::show::modal', this.infoModal.id, button)

},

resetInfoModal() {

this.infoModal.title = ''

this.infoModal.content = ''

},*/

    onFiltered(filteredItems) {
      // Trigger pagination to update the number of buttons/pages due to filtering

      this.totalRows = filteredItems.length;

      this.currentPage = 1;
    },

    async getData() {
      try {
        let response = await fetch(
          "http://universities.hipolabs.com/search?country=" + this.text
        );

        this.posts = await response.json();
      } catch (error) {
        console.log(error);
      }
    },

    created() {
      this.getData();
    },
  },
};
</script>