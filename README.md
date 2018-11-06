+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
Exercise #1: Example customer questions
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Description: Below are 3 common customer questions we receive. Please draft responses and/or followup responses to each question in the space below. This exercise helps us evaluate your ability to interpret customer requests and provide clear and comprehensive responses leveraging the available resources found in our docs, public site and app. Feel free to ask clarifying questions back to the customer in your response when appropriate. 

=======================================================================================
Question #1:
=======================================================================================
Hi Segment,

I'm using Wistia and I want to track my videos, specifically I want to track "Play", "Pause", and "End" events. 

- The events need to show up in Google Analytics as events.
- The GA event Category should be "Video" for all video events.
- The event label should be the name of the video being played. 

Can you help me with that? I'd love to see actual code samples on how to fire the event. 

Best,
Sara


Answer #1: 
================================================================

Hi Sara,

Thank you for reaching out regarding this! I would be more than happy to try to assist you. Currently, Segment does not have direct integrations with Wistia. We do, however, integrate with Google Analytics, as does Wistia, so we should still be able to get the information you want from the GA dashboard. 

Firstly, do you have Google Analytics up and running with your Segment account? If not, that would be the first step to take care of. Next, you will want to add the Google Analytics tracking scripts which can be found [here](https://developers.google.com/analytics/devguides/collection/analyticsjs/). Wistia requires that for the custom tracking they perform. Once you have that, Wistia should start tracking some of the info you want by default. The default configurations with Wistia track ‘Play’ and ‘100% watched’ which can stand in for ‘End’. The default event category is ‘Video’ and the event label should be the video title. The other thing you were wanting to track was ‘Pause’ which you should be able to do with a custom event. 

For adding ‘Pause’, you may want to reference Wistia’s JavaScript Play API docs found [here](https://wistia.com/support/developers/player-api). A pause event might added using comparable code to:
```javascript
video.bind("pause", function() {
  console.log("The video was just paused!");
});
```

A more robust tracking example with play, pause, and end might look like:

```javascript
video.bind('play', function() {
  if (video.time() == 0 || video.time() >= video.duration()) {
    ga('track','event','video','video start', video.name())
  } else {
    ga('track','event','video','play', video.name())
  };
})
wistiaEmbed.bind('pause',function(event) {
  ga('track','event','video','pause',video.name());
}); 
wistiaEmbed.bind("end", function () {
  ga('track','event','video','end',video.name());
});
```

Please let me know if you have any follow up questions or continue to face issues with setup. We’re always here to help!

Best,

Kyle


=======================================================================================
Question #2:
=======================================================================================
Hi Segment,

We recently got up and running on Segment. Got the JS and PHP sources set up broadcasting to Mixpanel, GA and Intercom. I was wondering if it is also possible to connect Intercom as a cloud source and also broadcast the events back to other integrations.

So for example, it would be nice to we could track opens and clicks from Intercom with Segment so we can broadcast those to Mixpanel and GA. We can of course do this ourselves by creating a simple webhook and use the PHP source to do that. But was just wondering if there is an even easier way.

Best,
Jerome


Answer to Question #2: 
================================================================

Hi Jerome,

Thanks for reaching out regarding this! We would be happy to help. It’s great that you already have Mixpanel, GA, and Intercom all up and running on Segment. Are you currently not seeing your Intercom data coming into Segment? If the Intercom data is not coming through, let me know as that is likely a different issue than what you are inquiring about.

Intercom is one of our Object Cloud Sources ([cloud sources list](https://segment.com/docs/sources/#object-cloud-sources)). At Segment, we have Sources and Destinations. Sources send data to Segment, while Destinations receive data from Segment. Both Mixpanel and GA can serve as Destinations. So you will just want to make sure you have Mixpanel and GA set up as Destinations for Intercom (the Source).

Please let me know if you have any other questions or setup issues. We’re always happy to help. Thanks for choosing Segment!

Best,

Kyle


=======================================================================================
Question #3:
=======================================================================================

Hey Segment,

I'm using analytics.js to track events and I’m currently sending data to Intercom and Google Analytics. I’m tracking a few events that I don’t want to send to Google Analytics, but I do want to send them to Intercom. Can I do this in the Segment UI? If so, where do I do that? Is there another way to filter events in the `.track()` call itself? 

Best,
Riley


Answer to Question #3: 
================================================================


Hi Riley,

Thanks for reaching out to us! If you are wanting to send only certain data/information to a Destination, you will want to do that with Schema Controls. If you’re on Segment’s Business plan, you can filter track calls right from the Segment UI on your Source Schema page by clicking on the field in the “Integrations” column and then adjusting the toggle for each tool. We recommend using the UI if possible since it’s a much simpler way of managing your filters and can be updated with no code changes on your side.

Please let me know if this solution works for you, otherwise we can try a different route. Thanks for choosing Segment. We are always here to help!

Best,

Kyle


+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
Exercise #2: Basic SQL exercise
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Description: Included below are credentials to access an example data set in a Postgres data warehouse, along with some questions requiring basic mysql analysis to answer.


=======================================================================================
Postgres cluster credentials:
=======================================================================================

* Hostname: ec2-54-83-194-117.compute-1.amazonaws.com
* Password: AbVv-wMQGmbOwFpeSdw9paFipJ
* Username: ujzapgrhcborxq
* Port: 5432
* Database: dbt6sotajkgv26
* Schema: music_is_me


=======================================================================================
Question #1: How many users canceled their plan? List them by name here.
=======================================================================================

1 user canceled their plan - Kenny Rogers

```sql
SELECT name FROM users WHERE plan_name='Canceled';
```


=======================================================================================
Question #2: How many different plans can users sign up for? What are they?
=======================================================================================

A user can sign up for 3 different plans - Basic, Advanced, Enterprise

```sql
SELECT DISTINCT plan_name FROM user_plans WHERE plan_name!='Canceled';
```


=======================================================================================
Question #3: How many users are signed up for each plan? Please present the result in a single table ordered by the number of users in each plan.
=======================================================================================

To see a count of each plan, along with how many have canceled:

```sql
SELECT plan_name, count(plan_name) FROM user_plans GROUP by plan_name;
```
 
To see only a count of each plan without canceled users:

```sql
SELECT plan_name, count(plan_name) FROM user_plans WHERE plan_name!='Canceled' GROUP by plan_name;
```

Note that this number may be slightly inaccurate as a few users have managed to sign up without a plan_name. This might be due to a user completing a sign up but then maybe bouncing before full account creation. In theory, I would think we would still capture their name, email, etc for re-engagement if they left before completing their full account so maybe this makes sense?


=======================================================================================
Question #4: A few users have appear to have a null `plan_id`. The original events all included a value for the `plan_id` but some were sent through as an integer (`plan_id: 1`) and others were sent as a string (`plan_id: '1'`). Why would that matter? 
=======================================================================================

This would matter based on the data type for the plan_id column. If the data type is set up as an integer, you would want to be sending the data through as an integer. It seems that the data type was set up as ‘text’, so you would likely want to send the plan_id through as a string and not an integer. 


=======================================================================================
Extra Credit: Are there any other insights you were able to uncover when analyzing the data set?
=======================================================================================

It seems that when a user cancels their account, the plan_name gets saved as ‘Canceled’. This feels very odd as canceled is not a plan type. Seems like a strange way to handle that information. What would be better is to have a separate column, say ‘plan_canceled’ in the `users` or `user_plans` table that stores a boolean value of whether someone cancels or not. This would be a bit more intuitive and would allow you to see if a certain plan level is more likely to get canceled over others. 


+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
Exercise #3: Explaining a technical concept
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Description: For this exercise, we'd like to test your ability to read code and provide clear technical and non-technical descriptions of what the code is solving for.

=======================================================================================
Question #1: Describe what the following code block does when executed in a browser
=======================================================================================

```
function formatQs() {
	var output = {};
	var qs = document.location.search.substring(1);
	qs = qs.split('&');

	for (var i = 0; i < qs.length; i++) {
		var tokens = qs[i].split('=');
	  output[tokens[0].toLowerCase()] = tokens[1];
	}

	return output;
}
```

This code takes any query string in the URL occuring after a ‘?’ and returns it as an object where each key/value pair of the object has its respective field name as the key and the query parameter value as the value. 

For instance, if I went to *https://www.google.com/?q1=foo&q2=bar&q3=foobar* in my browser, I would expect calling `formatQs();` to return 
```javascript
{
q1: ‘foo’, 
q2: ‘bar’, 
q3: ‘foobar’
}
```

```javascript
function formatQs() {
  var output = {};
  // takes the query string portion of a URL occuring after the ? and saves it as a string
  var qs = document.location.search.substring(1); //-> ‘q1=foo&q2=bar&q3=foobar’
  // splits the string into an array on the ampersand
  qs = qs.split('&'); //-> [‘q1=foo’, ‘q2=bar’, ‘q3=foobar’]

  // Loops through the array and breaks down into a single object where the field for each element before the = sign becomes the key and the value after the = sign becomes the value of the key/value pair
  for (var i = 0; i < qs.length; i++) {
    var tokens = qs[i].split('=');
    output[tokens[0].toLowerCase()] = tokens[1];
  }

  return output; //-> {q1: ‘foo’, q2: ‘bar’, q3: ‘foobar’}
}
```

=======================================================================================
Question #2: Describe a specific scenario in which the above function would be used for tracking purposes, and more specifically how Google Analytics uses the data to track marketing campaigns.
=======================================================================================
This could be used in a variety of ways. One way might be to track ad clicks from a particular source to your site. For instance, maybe you run an ad on Facebook, and when a user clicks said advertisement, the redirect to your site has a particular pixel that fires a Facebook UTM parameter. Something like:
*https://www.example.com/signup?utm_source=facebook.com&utm_campaign=fbadver1*

You can then track the firing of that pixel in GA and see how many visitors convert through sign-up using that particular ad. 


=======================================================================================
Question #3: Assuming that the analytics.js library is loaded on the page, please update the code block above to send through the standard utm parameters as properties in an analytics.track() call. Additional requirements:
* Only send the officially supported utm campaign parameters as properties
* Only send utm parameter properties that are included in the original URL query string (see below for example)
* The track call event name should be "User Referred"
* Example url: https://example.com/home?utm_source=google&utm_campaign=simulator_push&utm_content=trees&utm_name=forest
=======================================================================================

From my understanding, the accepted 5 utm parameters are utm_source, utm_medium, utm_campaign, utm_term, and utm_content. So assuming the URL formatting was set up correctly, the above block already creates a nice object which you should be able to pass in to the `.track()` call. The track method takes an optional properties object after the event name and its seems you could call the `formatQs()` function in place of manually typing the expected properties. Something like this:

`analytics.track(‘User Referred’, formatQs());`

The above seems like it should generate a .track() comparable to:

```javascript
analytics.track(‘User Referred’, { 
  utm_source: ‘google’,
  utm_campaign: ‘simulator_push’
  utm_content: ‘trees’,
  utm_name: ‘forest’
});
```

Now if there could be non-standard utm values in the URL such as ‘utm-name’, then some checking should occur to omit rogue parameters, and I would edit the code to something like this:

```javascript
function formatQs() {
 // define an array to hold the 5 accepted utm parameters
 const utmParams = [
   'utm_source',
   'utm_medium',
   'utm_campaign',
   'utm_term',
   'utm_content'
 ]
 let output = {};
 let qs = document.location.search.substring(1);
 qs = qs.split('&');
  for(let i = 0; i < qs.length; i++) {
   // get a substring of each element in qs before the '=' and check whether it is an accepted utm parameter included in utmParams
   if(utmParams.includes(qs[i].substr(0, qs[i].indexOf('=')))) {
     // if it is one of the accepted utm options then continue with creating it as a key/value pair in the object
     let tokens = qs[i].split('=');
     output[tokens[0].toLowerCase()] = tokens[1];
   }
 }
 return output;
}
```

This should return for the `output` an object of
```javascript
{
utm_source: 'google',
utm_campaign: 'simulator_push',
utm_content: 'trees' 
}
```
where `utm_name: ‘forest’` was not added as *utm_name* is not a standard supported utm parameter. 


+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
End
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
