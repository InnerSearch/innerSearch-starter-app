<template>
  <div id='defaultForm'>
  </div>
</template>

<script>
  /*
      Simple way to use InnerSearch
  */

  import Vue from 'vue';
  import {Searchbox,
    SearchDatalist,
    RefinementListFilter,
    Paginate,
    SearchButton,
    ResetButton,
    Hits,Generics,NumericListFilter} from 'vue-innersearch/src/innerSearch';



      Vue.component('searchbox', Searchbox);
      Vue.component('search-datalist', SearchDatalist);
      Vue.component('refinement-list-filter', RefinementListFilter);
      Vue.component('paginate', Paginate);
      Vue.component('search-button', SearchButton);
      Vue.component('reset-button', ResetButton);
      Vue.component('hits', Hits);
      Vue.component('numeric-list-filter', NumericListFilter);

      Vue.mixin(Generics);

  window.addEventListener('load', function () {
    new Vue({
      el: '#defaultForm',

      created : function () {
        // ES server configuration
        this.setHost({
            host: "es.yinyan.fr",
            port : 80,
            protocol : 'http'
        });
        this.setIndex('bank');
        this.setType('account');
      },

      template : `
                <section>
                    <h1 class='is-title'>innerSearch.js</h1>

                    <hr class='is-line' />

                    <div class="is-columns">
                        <div class="is-column is-one-fifth">
                            <div>
                                <refinement-list-filter :field="'state'" :search="true" :title="'State : '" :size="100"  orderKey="_count" orderDirection="desc" operator="AND">
                                    <template slot="label" slot-scope="{ displayCount,clickOnLabel,clickOnItem,items,checkedItems }">
                                        <select name="" id="test" v-model="checkedItems"  @change="clickOnItem(checkedItems)">
                                            <option selected="selected"></option>
                                            <option v-for="(item, index) in items" :value="item.key">
                                                    <label v-if="displayCount" :for="item.key" v-on:click='clickOnLabel(item.key)'>{{ item.key }} ( {{ item.doc_count }} )</label>
                                            </option>
                                        </select>
                                    </template>
                                    <template slot="viewmore"></template>
                                </refinement-list-filter>
                                <refinement-list-filter :field="'age'" :search="true" :title="'Age : '" :size="19"  orderKey="_count" orderDirection="desc" operator="OR">
                                    <template slot="title" slot-scope="{ title }">
                                        <h3 class="is-refinement-menu-title" style="width: fit-content;display: inline-block;margin-right: 120px;">{{title}}</h3>
                                    </template>
                                    <template slot="label" slot-scope="{ displayCount,clickOnLabel,clickOnItem,items,checkedItems }">
                                        <div  v-for="(item, index) in items" :key="index" class="is-item is-refinement-list">
                                            <input
                                            type="radio"
                                            name="age"
                                            :value="item.key"
                                            v-model="checkedItems"
                                            @change="clickOnItem(checkedItems)">
                                            <label v-if="displayCount" :for="item.key" v-on:click='clickOnLabel(item.key)'>{{ item.key }} years old ( {{ item.doc_count }} )</label>
                                            <label v-else :for="item.key" v-on:click='clickOnLabel(item.key)'>{{ item.key }}</label>
                                        </div>
                                    </template>
                                    <template slot="uncheck_all" slot-scope="{ uncheckAll }">
                                        <a href="#" v-on:click='uncheckAll()' style="font-size:0.8em">Clear</a>
                                    </template>
                                </refinement-list-filter>
                                <refinement-list-filter :field="'gender'" :search="true" :size="100" :title="'Gender : '" :displayCount="true" operator="OR" ></refinement-list-filter>
                            </div>
                        </div>
                        <div class="is-column">
                            <div>
                                <searchbox :autofocus="true" :realtime="true" :timeout="200" :field="'firstname'" :placeholder="'Search by firstname'"></searchbox>

                                <search-datalist :realtime="true" :field="'lastname'" :suggestion="['firstname', 'lastname']">
                                    <template slot="items" slot-scope="{ item }">
                                        {{ item._source.firstname }} {{ item._source.lastname }} (<strong>{{ item._source.gender }}</strong>)
                                    </template>

                                    <template slot="nosuggestion" slot-scope="{ value }">
                                        Sorry, "{{ value }}" doesn't exist... :(
                                    </template>

                                    <template slot="suggestions" slot-scope="{ suggestion }">
                                        <span v-html="suggestion.highlight.firstname ? suggestion.highlight.firstname[0] : suggestion._source.firstname"></span>
                                        <span v-html="suggestion.highlight.lastname ? suggestion.highlight.lastname[0] : suggestion._source.lastname"></span>
                                    </template>
                                </search-datalist>
                                <numeric-list-filter :field="'balance'">
                                    <template slot="header">
                                        <h3 class="is-nlf-title">Balance : </h3>
                                    </template>
                                </numeric-list-filter>
                                <div style="margin: 20px auto;width: 90%">
                                    <search-button></search-button>
                                    <reset-button></reset-button>
                                </div>
                                <hits>
                                    <template slot="hits" slot-scope="{ hits }">
                                        <div class="is-score is-hits">
                                            <strong v-if="hits.score === 0">No result found</strong>
                                            <strong v-else-if="hits.score === 1">1 result found</strong>
                                            <strong v-else-if="hits.score > 1">{{ hits.score }} results found</strong>
                                        </div>
                                        <div v-for="item in hits.items" :item="item">
                                            <div><strong>Identity (firstname, lastname) :</strong> {{ item._source.firstname }} {{ item._source.lastname }} ({{ item._source.state }}, {{ item._source.gender }} , {{item._source.balance}})</div>
                                        </div>
                                    </template>
                                </hits>

                                <paginate :previousText="'&#x2B9C; Previous page'" :nextText="'Next page &#x2B9E;'" :size="10"></paginate>
                            </div>
                         </div>
                    </div>
                </section>
            `
    });
  });

  export default {};
</script>

<style>
  @import url(https://unpkg.com/vue-innersearch@0.0.10/default-innersearch-theme.min.css)
</style>

