<template>
  <div id="app" class="container-fluid">
    <div class="jumbotron header row"
         style="background-color: #0199ae; padding: 60px 40px; text-align: center; color: #fff;">
      <h1 style="font-size: 36px; font-weight: 600; text-transform: uppercase;">Weekly links</h1>

      <h3>View the awesome knowledge shared by awesome people.</h3>
    </div>
    <div class="content" style="max-width: 1000px; margin: 0 auto; padding: 60px 20px;">
      <div class="filter">
        <form class="form form-inline" @submit="getLinks($event)">
          <div class="form-group label-floating">
            <label class="control-label">Start Date</label>
            <input type="date" name="startDate" class="form-control">
          </div>
          <div class="form-group label-floating">
            <label class="control-label">End Date</label>
            <input type="date" name="endDate" class="form-control">
          </div>
          <button type="submit" class="btn btn-primary btn-raised">Submit</button>
        </form>
      </div>

      <div class="links row">
        <div class="list-group">
          <div v-for="(row,index) in links" :key="index" class="list-group-item col-md-4" style="height: 400px;">

            <div class="panel panel-primary">
              <div class="panel-heading">
                <a :href="row.link" target="_blank"><h3 class="title">{{ row.title }}</h3></a>
              </div>
              <div class="panel-body">
                <p class="list-group-item-text">{{ row.description }}</p>
                <span class="list-group-item-text" style="font-weight: bold;">{{ row.author }}</span> <span style="font-weight: bold;">{{ row.date }}</span>
                <div class="list-group-separator"></div>
              </div>

              <div class="panel-footer">
                <input type="checkbox" v-model="selected[index]" class="form-control">
              </div>
            </div>
          </div>

        </div>
      </div>
      <div class="pull-right">
        <button class="btn btn-success btn-raised hidden" data-toggle="modal" data-target="#preview">Preview</button>
        <button class="pull-right btn btn-primary btn-raised" @click="copy">Copy</button>
      </div>
    </div>

    <div class="footer row">
      <div class="col-md-6">

      </div>
      <div class="col-md-6">
        <p style="color: white" class="pull-right">Prepared by BsB (Saroj Bista, Biju Nakarmi , Bishal Timilsina)</p>
      </div>
    </div>

    <div class="modal" id="preview">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <button type="button" class="close" data-dismiss="modal" aria-hidden="true">×</button>
            <h4 class="modal-title">View Links</h4>
          </div>
          <div class="modal-body">
            <p>{{ getSelected() }}</p>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
            <button type="button" class="btn btn-primary">Save changes</button>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<script>

  import axios from 'axios';
  import clipboard from 'clipboard-js';

  export default {
    name: 'app',
    data() {
      return {
        selected: [],
        links: [],
      };
    },
    created: function () {
      this.getLinks();
    },
    methods: {
      getSelected(){
        let clip = '';
        this.selected.forEach((value, index) => {
          if (value) {
            clip += `${this.links[index].link}\n`;
          }
        });
        return clip;
      },
      copy(){
        clipboard.copy(getSelected());
      },
      getLinks(e){
        if (e) {
          e.preventDefault();
        }

        let params = $('form').serialize();

        console.log(params);
        axios({
          headers: {
            'Access-Control-Allow-Origin': '*'
          },
          method: 'GET',
          url: 'http://localhost:3000?' + params,
          crossDomain: true,
        }).then((result) => {
          this.links = result.data;
        }).catch(function (err) {
          console.log(err);
        })
      }
    },
    watch: {}
  }

  import 'bootstrap/dist/css/bootstrap.css';
  import 'bootstrap-material-design/dist/css/bootstrap-material-design.css';

  import "bootstrap-material-design/dist/css/ripples.css";
  import "bootstrap-material-design/dist/js/material.js";
  import 'bootstrap-material-design/dist/js/ripples.js';
  import './assets/style.css';

  let $ = require('jquery');

  $.material.init();
  //  $.material.ripples();
</script>
<style scoped="true">
  .content {
    min-height:calc(100vh - 30px);
  }
  .links {
    margin-top: 30px;
  }
  .footer {
    background-color: #222;
    padding: 30px;
  }
  .list-group-item {
    background-color: #fff;
    /*border: 1px solid rgba(86, 83, 83, 0.13) !important;*/
    /*box-shadow: 0 3px 2px rgba(0,0,0,0.4);*/
  }
  .title {
    color: #fff;
    /*border: 1px solid rgba(86, 83, 83, 0.13) !important;*/
    /*box-shadow: 0 3px 2px rgba(0,0,0,0.4);*/
  }
</style>
