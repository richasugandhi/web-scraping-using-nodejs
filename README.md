# web-scraping-using-nodejs

"use strict";
var express = require('express');
const app = express();
const rp = require('request-promise');
const cheerio = require('cheerio');

rp('https://www.myfitnesspal.com/food/search?search=maxico', function (error, response, html) {
  if (!error && response.statusCode == 200) {
    var $ = cheerio.load(html);
    $('div.jss7').each(function(i, element){
      var url = $(this).find('a').attr('href');
    
      var finalUrl = 'https://www.myfitnesspal.com'+url;
      console.log(finalUrl);

        rp(finalUrl, function(nexterror, nextresponse, nexthtml) {
        	if (!error && response.statusCode == 200) {
        		var s$ = cheerio.load(nexthtml);
        		s$('div.jss74 ').each(function(i, element){
        			var nutritional = s$(this).text();
        			console.log(nutritional);

        			var nutritionalInt = parseInt(nutritional);
        			console.log(nutritionalInt);
        		});
        	}
        });
    });
   }
});



const PORT = process.env.PORT || 3333;
app.listen(PORT, () => {
  console.log(`Server listening on port ${PORT}...`);
});
