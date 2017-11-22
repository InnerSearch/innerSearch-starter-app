<template>
  <section>
    <h1 class='is-title'>InnerSearch.js</h1>
    <hr class='is-line' />
    <div>
      <searchbox :autofocus="true" :realtime="true" :queries="['label']" :placeholder="'Search by Label'"></searchbox>
      <refinement-list-filter :field="'administration_routes.label'" :size="20"></refinement-list-filter>
    </div>
    <hits>
      <template slot="hits" slot-scope="{ hits }">
        <div class="is-score is-hits">
          <strong v-if="hits.score === 0">No result found</strong>
          <strong v-else-if="hits.score === 1">1 result found</strong>
          <strong v-else-if="hits.score > 1">{{ hits.score }} results found</strong>
        </div>
        <div v-for="item in hits.items" class="is-item is-hits">
          <div><strong>Label :</strong> {{ item._source.label }}</div>
          <div><strong>Route(s) :</strong>
            <ul><li v-for="voies in item._source.administration_routes">{{ voies.label }}</li></ul>
          </div>
          <div v-if="item._source.is_commercialized === 1" style="margin-top : 20px;">
            <strong style="color : #0F4166;">Commercialized</strong>
          </div>
        </div>
      </template>
    </hits>

  </section>
</template>

<script>
  import InnerSearch, {
    searchbox,
    hits,
    refinementListFilter,
    Generics
  } from "innersearch";
  export default {
    created : function () {
      this.SetHost("https://qlap.limics.fr/search");
      this.SetIndex("qlap");
      this.SetType("specialities");
    },

    components : {
      'refinement-list-filter' : refinementListFilter,
      'searchbox' : searchbox,
      'hits' : hits
    }
  }
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
    background-image : url('./../node_modules/innersearch/src/images/search.svg');
    opacity : .4;
  }

  .is-field.is-searchbox {
    width : 100%;
    border : 0;
    color : #2C2C2C;
  }


  /*
      SearchBox
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
</style>
