# HW6 - {GiphyAPI - GifTastic}
Homework week 6 for UCLA Coding BootCamp.
This assignment makes use of the GIPHY API to make a dynamic web page that populates with gifs of your choice.

## This app displays buttons related to a certain topic and allows you to add search terms to generate additional buttons that when clicked, accesses the GIPHY API and generates 10 static GIPHY images. Click on an image to pause or play the GIF.

## Requirements
#### The app takes the entered search term topics from a user input box and pushes them into array from which it creates buttons in the HTML. Clicking on a button grabs 10 static, non-animated gif images from the GIPHY API and places them on the page.
- When the user clicks one of the still GIPHY images, the gif should animate. 
- If the user clicks the gif again, it should stop playing.
- With every gif is displayed its rating (PG, G, etc.). 

## Technologies Used
- HTML
- CSS Bootstrap
- JavaScript to make the page dynamic
- jQuery for Dom Manipulation
- AJAX for API GET requests

## Code Explanation
- A form was implemented to take the value from a user input box and add it into the `topics` array.
- CSS Bootstrap was used to arrange the page into columns and display the gifs in a gallery format.
- AJAX Call to Giphy's API was created to access the images by topic entered.
- Event listeners on "click" were utilized as follows:
	- To execute the function that adds topics to the array: $("#addShow").on("click", function(event)
	- To display the gifs to the page by clicking on the topic buttons: $(document).on("click", "#show", displayNetflixShow).
	- To pause and play the gifs by clicking on the Gifs: $(document).on("click", ".netflixGiphy", pausePlayGifs).

-------------

### Function Using AJAX Request to Giphy
- I created a function that allowed me to make an AJAX request to the Giphy API and then allowed me to further parse through the JSON object that was returned to access the rating, and url for the animated and still versions of the gifs.
- The Q parameter of the API was replaced by the search term input by the user and displayed as a button on the page.
- Results in the API link were limited to 10.
A div for each image was created with Bootstrap column size, class & data state attributes. From the returned JSON object, I included the rating, and static & animated image urls, and prepended that to the #gifArea in the HTML to display the gallery of returned gifs.

```
 	function displayNetflixShow() {

	var x = $(this).data("search");
	console.log(x);

	var queryURL = "https://api.giphy.com/v1/gifs/search?q=" + x + "&api_key=dc6zaTOxFJmzC&limit=10";

	console.log(queryURL);

	$.ajax({
          url: queryURL,
          method: "GET"
        }).done(function(response) {
        	var results = response.data;
        	console.log(results);
        	for (var i = 0; i < results.length; i++) {
        	
        	var showDiv = $("<div class='col-md-4'>");

        	var rating = results[i].rating;
        	var defaultAnimatedSrc = results[i].images.fixed_height.url;
        	var staticSrc = results[i].images.fixed_height_still.url;
        	var showImage = $("<img>");
        	var p = $("<p>").text("Rating: " + rating);

        	showImage.attr("src", staticSrc);
        	showImage.addClass("netflixGiphy");
        	showImage.attr("data-state", "still");
        	showImage.attr("data-still", staticSrc);
        	showImage.attr("data-animate", defaultAnimatedSrc);
        	showDiv.append(p);
        	showDiv.append(showImage);
        	$("#gifArea").prepend(showDiv);

        }
	});
}

```