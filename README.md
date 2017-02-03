# Code-Sample-Guide


## Express examples
[WillCallTickets - Server.js](https://github.com/WillCallTickets/willcall/blob/master/server.js)  
[Sample routing file](https://github.com/WillCallTickets/WillCall/blob/master/routes/stripe.js)


## Promise examples
[updating an invoice in the order flow](https://github.com/WillCallTickets/willcall/blob/master/server/controllers/store.js)  
[using new Promise() - line 183](https://github.com/WillCallTickets/WillCall/blob/master/server/controllers/members.js)  
[using angular's $q](https://github.com/WillCallTickets/willcall/blob/master/public/js/models/showModel.js)  
[Use of promses.all](https://github.com/WillCallTickets/WillCall/blob/master/lib/dbops/invoices.js) 
 ```
 // line 50
 // construct an array of promises based upon the user's cart items - then insert in db

 .then(function(invoice_id){
    
    var _date = Date.now();
    var unc = invoice_id + '_';
    var itemPromises = [];
    
    cart.items.forEach(function(itm){
      itemPromises.push(
        knex('invoiceitems').insert({
          invoice_id: invoice_id,
          ticket_id: (itm.kind === 'ticket') ? itm.item.showdate_id: null,
          product_id: (itm.kind === 'product') ? itm.item.productsku_id : null,
          uniqueid: unc + _date++,
          purchasename: purchaseEmail,
          dtshowdate: (itm.kind === 'ticket') ? itm.item._parentShowDate.dateofshow: null,
          name: itm.mainDesc,
          ages: (itm.kind === 'ticket') ? itm.item.ages : '',
          description: itm.secondaryDesc.reduce(function(prev,cur){return prev += ' ' + cur;},'').trim(),
          price: itm.itemPrice,
          quantity: itm.qty,
          lineitemtotal: itm.itemPrice * itm.qty,
          status: 'inprocess'
        })
      );
    })
    
    return Promise.all(itemPromises);
    })
  .then(function(data){
    // evaluate return data and return
    return 'success';
  });
 ```

Another example
[Use of promises.all](https://github.com/WillCallTickets/blazecast/blob/master/controllers/api.js)
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



# A c# "elegant" solution

*** It should be noted that the particlar ORM that I used, subsonic, created an active record pattern for interacting with the database ***

### [_Collection implementation using generics](https://github.com/WillCallTickets/Fox_2014/blob/master/Utils/_Collection.cs)

* Solved the problem of having several tables/classes that needed to share the same functionality.  
* Address the issue of managing collections that need to keep track of a user-defined ordering of rows. 
* [table example utilizing the iDisplayOrder column](https://github.com/WillCallTickets/Fox_SqlProgrammability/blob/master/Tables/dbo.JShowAct.Table.sql)
* I was able to expand on this model and implement different sort orders based on "principal" ownership, allowing for multiple discrete entites to have their own sort-order over a shared table
* This allowed me to decouple the manipulation layer (jquery ajax calls) from the storage layer (sql). 	
* this enabled me to put all of the shared functionality into a single file for management of several tables  
[PrincipalOrder.ashx](https://github.com/WillCallTickets/Fox_2014/blob/master/WcWeb/Admin/_customControls/PrincipalOrder.ashx)



## REACT - Using Higher Order Components
This construct really clicked for me recently. I am a big fan of using base classes in c# and I see many parallels to HOCs in React. I am looking forward to fleshing out this implementation  
[C# base class simplistic example] (https://github.com/WillCallTickets/Fox_2014/blob/master/Wcss/Derived/_ContextBase.cs)  
[React HOC even more simplistic example](https://github.com/WillCallTickets/react-auth0-routerv4/blob/master/src/containers/_baseContainer.js)  



RE: "this" - In the past my methodology for handling "this" was to be explicit when referencing, defining it as "self" at the head of a function.  
[How I historically handled this...](https://github.com/WillCallTickets/Fox_2014/blob/master/Z2Web/assets/javascripts/z2ModalService.js)  
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

