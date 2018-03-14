<template>
  <div id='defaultForm'>
  </div>
</template>

<script>
  /*
      Simple way to use InnerSearch
  */

  import Vue from 'vue';
  import InnerSearch from '../node_modules/innersearch/InnerSearch';

  Vue.use(InnerSearch);

  window.addEventListener('load', function () {
    new Vue({
      el: '#defaultForm',

      created : function () {
        // ES server configuration
        this.setHost('http://es.yinyan.fr');
        this.setIndex('bank');
        this.setType('account');
      },

      template : `
                <section>
                    <h1 class='is-title'>InnerSearch.js</h1>

                    <hr class='is-line' />

                    <div>
                        <searchbox :autofocus="true" :realtime="true" :timeout="2000" :field="['firstname']" :placeholder="'Search by firstname'"></searchbox>
                        <!--<searchbox :realtime="true" :field="['firstname', 'lastname']" :pattern="'.*{v}.*'" :operator="'OR'" :placeholder="'Search by firstname and lastname (prefix)'" :suggestionbox="true">
                            <template slot="suggestions" slot-scope="{ suggestion }">
                                <div style="display : inline-block; width : 60%;" v-html="">
                                    {{ suggestion._source.firstname }} {{ suggestion._source.lastname }}
                                </div>
                                <div style="display : inline-block; width : 39%; text-align : right; opacity : 0.35;">
                                    <strong>{{ suggestion._source.gender }}</strong>
                                </div>
                            </template>
                        </searchbox>
                        <searchbox :realtime="true" :field="['firstname', 'lastname']" :pattern="'.*{v}.*'" :operator="'OR'" :placeholder="'Search by firstname and lastname'" :suggestionbox="true">
                            <template slot="suggestions" slot-scope="{ suggestion }">
                                <div>{{ suggestion._source.firstname }} {{ suggestion._source.lastname }}</div>
                            </template>
                        </searchbox>-->
                        <refinement-list-filter :field="'state'" :title="'State: '" :dynamic="false" orderKey="_count" orderDirection="asc" operator="OR"></refinement-list-filter>
                        <refinement-list-filter :field="'gender'" :size="100" :title="'Gender: '" :displayCount="true" operator="OR"></refinement-list-filter>
                        <search-button></search-button>
                    </div>

                    <hits>
                        <template slot="hits" slot-scope="{ hits }">
                            <div class="is-score is-hits">
                                <strong v-if="hits.score === 0">No result found</strong>
                                <strong v-else-if="hits.score === 1">1 result found</strong>
                                <strong v-else-if="hits.score > 1">{{ hits.score }} results found</strong>
                            </div>
                            <div v-for="item in hits.items" :item="item">
                                <div><strong>Identity (firstname, lastname) :</strong> {{ item._source.firstname }} {{ item._source.lastname }} ({{ item._source.state }}, {{ item._source.gender }})</div>
                            </div>
                        </template>
                    </hits>

                    <paginate :previousText="'Previous page'" :nextText="'Next page'" :size="10"></paginate>
                </section>
            `
    });
  });

  export default {};
</script>


<style>
  /*
     Global fields
 */

  .is-component {
    border : 1px solid #A9A9A9;
    font-family : Calibri, sans-serif;
  }

  .is-icon {
    background-repeat : no-repeat;
    background-position : center center;
    background-size : contain;
  }

  .is-field {
    font-size : 2em;
    padding : 5px;
  }

  .is-button {
    padding : 15px;
    border : 1px solid transparent;
    border-radius : 4px;
    font-size : 1.25em;
    cursor : pointer;
    user-select : none;
  }

  .is-field:focus, .is-field:focus{
    outline: none;
  }


  /*
      Assets
  */

  .is-title {
    font-size : 4em;
    color : #3C434B;
    font-family : Calibri, sans-serif;
    margin : 20px 0 10px 5%;
  }

  .is-line {
    width : 90%;
    margin: auto;
  }


  /*
      SearchBox
  */

  .is-component.is-searchbox {
    display : flex;
    width : 90%;
    margin : 20px auto;
  }

  .is-icon.is-searchbox {
    width : 50px;
    height : 50px;
    margin : 5px;
    background-image : url('./assets/search.svg');
    opacity : .4;
  }

  .is-field.is-searchbox {
    width : 100%;
    border : 0;
    color : #2C2C2C;
  }


  /*
      Refinement list
  */

  .is-component.is-refinement-list {
    width : 90%;
    margin : 20px auto;
    padding : 15px;
    font-size : 1.25em;
    box-sizing : border-box;
  }

  .is-item.is-refinement-list {
    display : inline-block;
    width : 20%;
  }


  /*
      Search Button
  */

  .is-component.is-search-button {
    display : inline-block;
    margin : 0 25px 0 5%;
    border : 0;
  }

  .is-button.is-search-button {
    color : #fff;
    background-color : #337ab7;
    border-color : #2e6da4;
  }

  .is-button.is-search-button:hover {
    color : #fff;
    background-color : #286090;
    border-color : #204d74;
  }

  .is-button.is-search-button:active {
    color : #fff;
    background-color : #286090;
    border-color : #204d74;
  }

  .is-button.is-search-button:focus {
    color : #fff;
    background-color : #286090;
    border-color : #122b40;
  }


  /*
      Hits
  */

  .is-component.is-hits {
    width : 90%;
    margin : 20px auto;
    padding : 15px;
    font-size : 1.25em;
    box-sizing : border-box;
  }

  .is-score.is-hits {
    margin : 10px 0 20px 50px;
    font-size : 1.75em;
    font-weight : bolder;
    font-variant : small-caps;
  }

  .is-item.is-hits {
    width : 90%;
    margin : 20px auto;
    padding : 20px;
    background : #EFEFEF;
  }

  .is-item.is-hits ul {
    margin : 0;
  }


  /*
      Pagination
  */


  /*
      Suggestion Box
  */

  .is-component.is-suggestion-box {
    width : 90%;
    margin : 0 auto;
    border : 2px solid #C4C4C4;
    padding : 10px;
  }
</style>
