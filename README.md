# Code-Sample-Guide


## Express examples
[WillCallTickets - Server.js](https://github.com/WillCallTickets/willcall/blob/master/server.js)  
[Example routing file](https://github.com/WillCallTickets/WillCall/blob/master/routes/stripe.js)


## Promise examples
[updating an invoice in the order flow](https://github.com/WillCallTickets/willcall/blob/master/server/controllers/store.js)  
[using new Promise() - line 183](https://github.com/WillCallTickets/WillCall/blob/master/server/controllers/members.js)  
[using angular's $q -  line 61](https://github.com/WillCallTickets/willcall/blob/master/public/js/models/showModel.js)  
[Use of promses.all - line 50](https://github.com/WillCallTickets/WillCall/blob/master/lib/dbops/invoices.js)   
[Another example of promises.all - line 257](https://github.com/WillCallTickets/blazecast/blob/master/controllers/api.js)
```
// line 257

exports.getFedPodcastEpisodes = function(req, res, next){

  var podcast = {};
  var pId = req.params.itunes_podcast_id;

  // eCollection is the returned collection of episode objects

  return audiosearch.get('/shows', {'itunes_id':pId})
  .then(function(data){
    podcast = data;
    var episodePromises = [];
    if(podcast.episode_ids.length > 0){
      podcast.episode_ids.forEach(function(itm){
        episodePromises.push( audiosearch.getEpisode(itm) );
      });
    }
    return Promise.all(episodePromises);
  })
  .then(function(data){
    podcast.eCollection = data;
    res.send(podcast);
  })
  .catch(function(err){
    console.log('ERROR AT API CATCH', err);
    res.send(err);
  });

}
```

## Galvanize projects
[Q2 group project - Cloak And Tagger](https://github.com/WillCallTickets/cloakandtagger)  
[Q3 group project - Blazecast](https://github.com/WillCallTickets/blazecast)  
[Q4 solo "Capstone" project - WillCallTicketing](https://github.com/WillCallTickets/WillCall)  
[WillCallTicketing - Live Link](https://willcalltickets.herokuapp.com/)


## A javascript "elegant" solution 
handling http/https issues with loading static resources
```
//Server.js

// Proxy Resource - resolves by correcting https and http for resources
app.get('/proxyresource/:resourceurl',
  resourceController.proxyResource);


// resourceController
var request = require('request');
var atob = require('atob');

exports.proxyResource = function(req, res, next) {
  
  // decode
  var decodedString = atob(req.params.resourceurl);
  // send it back as a stream
  // console.log('PXY API atob', decodedString, req.params.resourceurl);
  
  request(decodedString).pipe(res);
};
```
#### use as a filter to wrap the link
```
angular.module('MyApp')
  .filter('removeProtocol', function () {
    return function (input) {
      if (input) {
        return input.replace(/^http:\/\//g, '')
        .replace(/^https:\/\//g, '');
      }
    }
  })
  .filter('proxyResource', function () {
    return function (input) {
      if (input) {
        var encoded = btoa(input);
        return '/proxyresource/' + encoded;
      }
    }
  })
  .filter('trustMe', ['$sce', function($sce){
    return function(input){
      if(input){
        return $sce.trustAsHtml(input);
      }
    }
  }]);
  ```




## REACT - Using Higher Order Components
This construct really clicked for me recently. I am a big fan of using base classes in c# and I see many parallels to HOCs in React. I am looking forward to fleshing out this implementation  
[C# base class example] (https://github.com/WillCallTickets/Fox_2014/blob/master/Wcss/Derived/_ContextBase.cs)  
[React HOC simplistic example](https://github.com/WillCallTickets/react-auth0-routerv4/blob/master/src/containers/_baseContainer.js)  



## RE: "this"
Historically, my methodology for handling "this" was to be explicit when referencing, defining it as "self" at the head of a function. My mantra here is "When there is a possibiliity of ambiguity - be specific".  
["this" reference example](https://github.com/WillCallTickets/Fox_2014/blob/master/Z2Web/assets/javascripts/z2ModalService.js)  
```
...
    $.fn.z2Modal = function (_fnHandlerPath, _fnSuccess, _inputs, _fnParamValidate) {  
  
        //ensure we are just dealing with a single element
        var self = $(this).get(0);
        if (self != undefined) {
            var selfId = self.id;
            var sender = $('#' + selfId + ' .wct-modal-action');
            var errorDisplayElement = $('#' + selfId + ' .wctmodal-error');
            var successDisplayElement = $('#' + selfId + ' .wctmodal-success');

...
```


## A c# "elegant" solution

*** the ORM that I used, subsonic, created an active record pattern for interacting with the database ***

[_Collection implementation using generics](https://github.com/WillCallTickets/Fox_2014/blob/master/Utils/_Collection.cs)

* Solved the problem of having several tables/classes that needed to share the same functionality.  
* Address the issue of managing collections that need to keep track of a user-defined ordering of rows. 
* [Table example utilizing the iDisplayOrder column](https://github.com/WillCallTickets/Fox_SqlProgrammability/blob/master/Tables/dbo.JShowAct.Table.sql)
* I was able to expand on this model and implement different sort orders based on "principal" ownership, allowing for multiple discrete entites to have their own sort-order over a shared table
* This allowed me to decouple the manipulation layer (jquery ajax calls) from the storage layer (sql). 	
* All of the shared functionality could be accessed from a single file for management of several tables  
[PrincipalOrder.ashx](https://github.com/WillCallTickets/Fox_2014/blob/master/WcWeb/Admin/_customControls/PrincipalOrder.ashx)

