<template>
  <div class="sunday-morning-single">
    <span v-if="loading">Loading data...</span>
    <div class="page" v-if="item">
      <div class="half-page">
        <div class="header">
          <h1><span v-if="item.fields.Service_Type == 'Sunday AM'">AM</span><span v-if="item.fields.Service_Type == 'Sunday PM'">PM</span><span v-if="item.fields.Service_Type == 'Midweek'">Midweek</span> Order of Service</h1>
          <h2>{{ item.fields.Date | moment }}</h2>
        </div><!--/header-->
        <div class="order">
          <draggable class="dragarea source" @start="drag=true" @end="onEnd">
            <li v-if="item.fields.Chorus" v-for="(chrus_item,index) in item.fields.Chorus"><img src="static/chorus.svg" alt=""><strong>Chorus:</strong> {{ item.fields.chorus_name[index] }} <a href="javascript:void(0)" @click="removeElement($event)" class="close"><img src="static/close.svg" alt=""></a></li>
            <li><img src="static/welcome.svg" alt=""><strong>Welcome &amp; Prayer</strong><a href="javascript:void(0)" @click="removeElement($event)" class="close"><img src="static/close.svg" alt=""></a></li>
            <li><img src="static/handshake.svg" alt="" class="handshake"><strong>Greeting &amp; Fellowship</strong><a href="javascript:void(0)" @click="removeElement($event)" class="close"><img src="static/close.svg" alt=""></a></li>
            <li v-if="item.fields.Hymns" v-for="(hymn_item,index) in item.fields.Hymns"><img src="static/hymnal.svg" alt=""><strong>Hymn:</strong>  {{ item.fields.Hymn_Number[index] }}, {{ item.fields.Hymn_Title[index] }}<a href="javascript:void(0)" @click="removeElement($event)" class="close"><img src="static/close.svg" alt=""></a></li>
            <li v-if="item.fields['Choir Song']">
              <span v-for="(choir_song_item,index) in item.fields['Choir Song']">
                <img src="static/choir.svg" alt=""> <strong>Choir Song: </strong>{{ item.fields.choir_song_name[index] }}
              </span>              
            <a href="javascript:void(0)" @click="removeElement($event)" class="close"><img src="static/close.svg" alt=""></a></li>
            <li v-else>
                <img src="static/choir.svg" alt=""> <strong>Choir Song: </strong>    
            <a href="javascript:void(0)" @click="removeElement($event)" class="close"><img src="static/close.svg" alt=""></a></li>
            <li v-if="item.fields.Missionary_Name" v-for="single_item in item.fields.Missionary_Name"><img src="static/missions.svg" alt=""><strong>Missionary Letter: </strong>{{ single_item }}<a href="javascript:void(0)" @click="removeElement($event)" class="close"><img src="static/close.svg" alt=""></a></li>
            <li v-if='item.fields.Extra' class="missionary" v-for="(single_item,index) in item.fields.Extra">
              <img src="static/extra.svg" alt=""><span>{{ item.fields.extra_description[index] }}</span>
            <a href="javascript:void(0)" @click="removeElement($event)" class="close"><img src="static/close.svg" alt=""></a></li>
              <li class="missionary"  v-if='item.fields.Special' v-for="(special_item,index) in item.fields.Special">
                <img src="static/special.svg" alt=""><span>{{ item.fields.special_song_name[index] }} by {{ item.fields.Special_singer[index] }}</span>
              <a href="javascript:void(0)" @click="removeElement($event)" class="close"><img src="static/close.svg" alt=""></a></li>
            <li class="announcements" v-if="item.fields.Announcements">
              <img src="static/calendar.svg" alt=""><strong>Announcements:</strong>
              <br>
              <ul>                
                <li v-for="(announcement_item,index) in item.fields.Announcements">{{ item.fields.announcement_name[index] }}<a href="javascript:void(0)" @click="removeElement($event)" class="close"><img src="static/close.svg" alt=""></a></li>                  
              </ul>
            </li>
            <li class="offering" v-if="item.fields.Offering"><img src="static/offeratory.svg" alt=""><strong>Offering</strong>{{ item.fields.Offering_Musician[0] }} - <span v-if="item.fields.Offering_Song">{{ item.fields.Offering_Song[0] }}</span><a href="javascript:void(0)" @click="removeElement($event)" class="close"><img src="static/close.svg" alt=""></a></li>
            <li class="offering" v-else><img src="static/offeratory.svg" alt=""><strong>Offering</strong><a href="javascript:void(0)" @click="removeElement($event)" class="close"><img src="static/close.svg" alt=""></a></li>
            <li class="sermon" v-if="item.fields.Sermon"><img src="static/sermon.svg" alt=""><strong>Sermon: </strong>
              <br>
              <em>{{ item.fields.sermon_name[0] }}</em> <br>by {{ item.fields.sermon_preacher[0] }}
            <a href="javascript:void(0)" @click="removeElement($event)" class="close"><img src="static/close.svg" alt=""></a></li>
            <li class="sermon" v-else><img src="static/sermon.svg" alt=""><strong>Sermon: </strong>
            <a href="javascript:void(0)" @click="removeElement($event)" class="close"><img src="static/close.svg" alt=""></a></li>
          </draggable>
        </div><!--/order-->
        </div><!--/half-page-->
      <div class="half-page second-half">
        <div class="header">
          <h1><span v-if="item.fields.Service_Type == 'Sunday AM'">AM</span><span v-if="item.fields.Service_Type == 'Sunday PM'">PM</span><span v-if="item.fields.Service_Type == 'Midweek'">Midweek</span> Order of Service</h1>
          <h2>{{ item.fields.Date | moment }}</h2>
        </div><!--/header-->
        <div class="order">
          <div class="dragarea destination">
            <li v-if="item.fields.Chorus" v-for="(chrus_item,index) in item.fields.Chorus"><img src="static/chorus.svg" alt=""><strong>Chorus:</strong> {{ item.fields.chorus_name[index] }} <a href="#" class="close"><img src="static/close.svg" alt=""></a></li>
            <li><img src="static/welcome.svg" alt=""><strong>Welcome &amp; Prayer</strong><a href="#" class="close"><img src="static/close.svg" alt=""></a></li>
            <li><img src="static/handshake.svg" alt="" class="handshake"><strong>Greeting &amp; Fellowship</strong><a href="#" class="close"><img src="static/close.svg" alt=""></a></li>
            <li v-if="item.fields.Hymns" v-for="(hymn_item,index) in item.fields.Hymns"><img src="static/hymnal.svg" alt=""><strong>Hymn:</strong>  {{ item.fields.Hymn_Number[index] }}, {{ item.fields.Hymn_Title[index] }}<a href="#" class="close"><img src="static/close.svg" alt=""></a></li>
            <li v-if="item.fields.Choir_Song">
              <span v-for="(choir_song_item,index) in item.fields.Choir_Song">
                <img src="static/choir.svg" alt=""> <strong>Choir Song: </strong>{{ item.fields.choir_song_name[index] }}
              </span>              
            <a href="#" class="close"><img src="static/close.svg" alt=""></a></li>
            <li v-if="item.fields.Missionary_Name" v-for="single_item in item.fields.Missionary_Name"><img src="static/missions.svg" alt=""><strong>Missionary Letter: </strong>{{ single_item }}<a href="#" class="close"><img src="static/close.svg" alt=""></a></li>
            <li v-if='item.fields.Extra' class="missionary" v-for="(single_item,index) in item.fields.Extra">
              <img src="static/extra.svg" alt=""><span>{{ item.fields.extra_description[index] }}</span>
            <a href="#" class="close"><img src="static/close.svg" alt=""></a></li>
              <li class="missionary"  v-if='item.fields.Special' v-for="(special_item,index) in item.fields.Special">
                <img src="static/special.svg" alt=""><span>{{ item.fields.special_song_name[index] }} by {{ item.fields.Special_singer[index] }}</span>
              <a href="#" class="close"><img src="static/close.svg" alt=""></a></li>
            <li class="announcements" v-if="item.fields.Announcements">
              <img src="static/calendar.svg" alt=""><strong>Announcements:</strong>
              <br>
              <ul>                
                <li v-for="(announcement_item,index) in item.fields.Announcements">{{ item.fields.announcement_name[index] }}<a href="#" class="close"><img src="static/close.svg" alt=""></a></li>                  
              </ul>
            </li>
            <li class="offering" v-if="item.fields.Offering"><img src="static/offeratory.svg" alt=""><strong>Offering</strong>{{ item.fields.Offering_Musician[0] }} - <span v-if="item.fields.Offering_Song">{{ item.fields.Offering_Song[0] }}</span><a href="#" class="close"><img src="static/close.svg" alt=""></a></li>
            <li class="offering" v-else><img src="static/offeratory.svg" alt=""><strong>Offering</strong><a href="#" class="close"><img src="static/close.svg" alt=""></a></li>
            <li class="sermon" v-if="item.fields.Sermon"><img src="static/sermon.svg" alt=""><strong>Sermon: </strong>
              <br>
              <em>{{ item.fields.sermon_name[0] }}</em> <br>by {{ item.fields.sermon_preacher[0] }}
            <a href="javascript:void(0)" @click="removeElement($event)" class="close"><img src="static/close.svg" alt=""></a></li>
          </div>
        </div><!--/order-->
        </div><!--/half-page-->
      </div><!--/page-->
  </div>
</template>

<script>
import axios from 'axios'
import moment from 'moment'
import draggable from 'vuedraggable'
import $ from "jquery";

export default {
  components: {
      draggable,
  },
  name: 'sunday_morning_single',
  data:function(){
    return {
      id : '',
      item: null,
      app_id : '',
      app_key : '',
      loading: false
    };
  },
  mounted:function(){
    this.id = this.$route.params.id;
    this.getAppCredentials();
    this.getItem();
  },
  filters:{
    moment: function(date){
      return moment(date).format('dddd, MMMM Do, YYYY');
      }
    },
  methods:{
    onEnd:function(event){
      $(".dragarea.destination").html($(".dragarea.source").html());
    },
    copyToDestination:function(){
      $(".dragarea.destination").html($(".dragarea.source").html());
    },
    removeElement:function(event){
      $(event.target).closest('li').remove();
      this.copyToDestination();
    },
    getAppCredentials:function(){
      if(localStorage.getItem('app_id'))
        this.app_id = localStorage.getItem('app_id');
      if(localStorage.getItem('app_key'))
        this.app_key = localStorage.getItem('app_key');
    },
    getItem:function(){
      var _self = this;
      this.loading = true;
      if(this.app_id != "" && this.app_key != ""){
        axios.get('https://api.airtable.com/v0/'+this.app_id+'/Services/'+this.id+'?api_key='+this.app_key)
          .then(function (response) {
            _self.item = response.data;
            _self.loading = false;
          })
          .catch(function (error) {
            console.log(error);
            _self.loading = false;
          });
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@charset "UTF-8";
@import url(https://fonts.googleapis.com/css?family=Lato:400,400i,700,700i);
img.calendar {
  width: 30px;
  margin-right: 5px;
}
.page h1,
.page h2 {
    text-align: center;
    margin-top: 0
}
.page h2,
.page .dragarea {
    font-style: italic
}
.page {
/*    width: 11in;
    height: 8.5in;
    padding: .25in;*/
    padding: 30px;
    text-align: center;
    font-family: Lato, sans-serif;
    display: flex;
    justify-content: center
}
.half-page {
    width: 4.75in
}
.page h1 {
    font-size: 20pt;
    weight: 900;
    line-height: 24pt;
    margin-bottom: 0
}
.page h2 {
    font-size: 18pt;
    weight: 400;
    line-height: 100%
}
.page .header {
    border-bottom: 1px solid #000;
    margin-bottom: .25in
}
.page .dragarea {
    list-style-type: none;
    padding-left: 0;
    margin: 0;
    font-size: 14pt;
    line-height: 16pt;
    text-align: left
}
.page .dragarea li {
    margin-bottom: .2in;
    position: relative;
}
.page .dragarea li a.close {
  position: absolute;
    top: 0;
    right: 0;
}
.page .dragarea li a.close {
    position: absolute;
    top: 0;
    right: 0;
}
.page .dragarea li img{
    /*width: 20px;*/
    height: 22px;
    margin-right: 5px;
}
.page .dragarea li strong {
    font-style: normal;
    margin-right: .15in
}
.page .dragarea li.offering {
    text-align: center
}
.page .dragarea li.announcements,
.page .dragarea li.missionary,
.page .dragarea li.sermon {
    text-align: center;
    padding: .1in;
    background-color: #DADADA;
    border: 1px solid #000
}
.page .dragarea li.announcements strong,
.page .dragarea li.missionary strong,
.page .dragarea li.sermon strong {
    margin-right: 0
}
.page .dragarea li.announcements em,
.page .dragarea li.missionary em,
.page .dragarea li.sermon em {
    text-decoration: underline
}
.page .dragarea li.announcements ul,
.page .dragarea li.missionary ul,
.page .dragarea li.sermon ul {
    list-style-type: none;
    margin-left: 0;
    padding-left: 0
}
.page .dragarea li.announcements ul li:before,
.page .dragarea li.missionary ul li:before,
.page .dragarea li.sermon ul li:before {
    margin-right: 20px
}
.page .dragarea li.announcements ul li ul,
.page .dragarea li.missionary ul li ul,
.page .dragarea li.sermon ul li ul {
    padding-left: 31px;
    list-style: none
}
.page .dragarea li.announcements ul li li:before,
.page .dragarea li.missionary ul li li:before,
.page .dragarea li.sermon ul li li:before {
    content: '—'
}
.second-half {
  display: none;
}
@media only screen and (max-width: 740px) {
   .page .dragarea li a.close {
    display: none;
}
}
@media print {
  .page {
    width: 11in;
    height: 8.5in;
    padding: .25in;
    text-align: center;
    font-family: Lato, sans-serif;
    display: flex;
    justify-content: space-between
}
  .page .dragarea li a.close {
    display: none;
}
.second-half {
  display: block;
}
}
</style>
