## Organizing Data

Written by Brandon T. Locke; portions from Thomas G. Padilla

### OpenRefine

[OpenRefine](http://openrefine.org/) (formerly known as GoogleRefine), is a very popular tool for working with unorganized, non-normalized (what some may call "messy") data. OpenRefine accepts TSV, CSV, XLS/XLSX, JSON, XML, RDF as XML, and Google Data formats, though others may be used with extensions. It works by opening into your default browser window, but all of the processing takes place on your machine and your data isn't uploaded anywhere.

**This tutorial will demonstrate some of the most popular and powerful features of OpenRefine, including geocoding using an API, algorithmic word normalization and correction, and working with multi-value cells.**

### Data
**You can [download a .zip file of the following three datasets](@@@@@).**

#### Comic Book Metadata (**authors-people.csv**)
Dataset comprising records for comic-books and books relating to comics in the British National Bibliography.[See the full readme](http://www.thomaspadilla.org/data/dataprep/Readme.txt)

#### Comic Book Publisher Locations(**comic-publisher-info.csv**)
Very small dataset containing the locations of several different comic book publishers.


#### Academy Award Nominee Metadata (**aa-movies.csv**)
This is a set of data fields scraped from Wikipedia pages for every movie nominated for an Academy Award for best film. Data provided by Taylor Arnold.


### Organizing Movie metadata

#### Loading the Dataset

2. Open OpenRefine - it should open a window in your default web browser
1. Click 'Browse' and locate the aa-movies.csv on your hard drive. Then click 'Next.'
1. The Configure Parsing Options screen will ask you to confirm a few things. It has made guesses, based on the data, on the type of file, the character encoding and the character that separates columns. Take a look at the data in the top window and make sure everything looks like it's showing up correctly.
1. Name the project "movies-metadata" and click 'Create Project' in the top right corner.

#### Evaluation
Take a minute to look around. Consider the structure of the data with principles of "tidy data" in mind. This will help guide what types of operations you perform on the data. Also take time to evaluate the type of information that is represented and what type of questions you might want to ask of it.

#### Working with Multi-Value Cells
This data has multiple actors listed in the 'Starring' column, with each name separated by a comma. For many purposes, this works great—it's the most compact and concise way to represent this information. But for many purposes, you may need to format this data differently. Here are a few different ways you may need to export this dataset.

**Multiple 'Starring' Columns with One Person in Each**
1. Select Starring > Edit Column > Split into several columns
2. You'll see a few options—for this you want to leave things as-is and separate on the commas. Click OK.
3. You now have 19 separate 'Starring' columns, and each column is either empty or has one value.

Let's try another way. Click on 'Undo/Redo' in the top left and undo your split command.

**Create TRUE/FALSE Columns Based on Values**
1. Select Starring > Edit column based on this column
2. In the GREL window, type `if(value.contains("Marie Prevost"), "true", "false")`
3. Call this column 'isMariePrevost' and click OK.
4. Do this again for Thomas Meighan. Starring > Edit column based on this column.
5. Type `if(value.contains("Thomas Meighan"), "true", "false")`, name it 'isThomasMeighan' and click OK.

**Create an Actor-Movie Network Dataset**
To create networks, we need to have source and target pairs—in other words, we need a separate line with each unique combination of movie and actor.

1. Select Starring > Edit cells > Split multi-valued cells
2. This will open a dialog box asking for a separator—leave the comma and click OK.
3. You'll see that we've now added several rows below each row with multiple actors listed. The actor name is the only bit of information in the entire row.
4. Select movie_id > Edit cells > Fill down.
5. You can do this for each column of information that you want in the network dataset.
6. We have lots of movies that don't have any actor information, meaning they won't really work with our network dataset. Remove these by clicking Starring > Facet, Customized Facets > Facet by blank.
7. Click True to select 248 rows. Click on All > Edit Rows > Remove all matching rows.
8. Click 'Remove All' in the top left to clear the facet.

### Organizing Comics Metadata
*This portion is adapted from [Thomas Padilla's 'Getting Started with OpenRefine'](http://thomaspadilla.org/dataprep/)*

#### Loading the Dataset
1. Click 'Open' in the top right to open a new OpenRefine tab
1. Click 'Browse' and locate the authors-people.csv on your hard drive. Then click 'Next.'
1. The Configure Parsing Options screen will ask you to confirm a few things. It has made guesses, based on the data, on the type of file, the character encoding and the character that separates columns. Take a look at the data in the top window and make sure everything looks like it's showing up correctly.
1. Name the project "comics-metadata" and click 'Create Project' in the top right corner.

#### Evaluation
Take a minute to look around. Consider the structure of the data with principles of "tidy data" in mind. This will help guide what types of operations you perform on the data. Also take time to evaluate the type of information that is represented and what type of questions you might want to ask of it (e.g. Which publishers are most prominently represented in the collection?)

#### Facet Data
Each column has a facet function that allows you to quickly identify inconsistencies in your data by counting the number of unique occurrences for each piece of data in that column. Using this function we will analyze the distribution of comics in the dataset by publisher. The first step is to begin assessing the consistency of data in the Publisher column.

Try the following sequence of steps:

1. Click the Publisher column > Select Facet > Select Text Facet
2. Facet by 'count' on the left hand side > Observe the record count for the Publisher Titan
3. Facet by 'name' on the left hand side > Scroll down to Titan > Observe variant representation of Titan
4. Hover cursor to the right of 'Titan.,' > Click 'edit' > Type Titan > Titan., transformed to Titan across 33 records

#### Filter Data
Each column contains a 'text filter' function. The text filter is useful for identifying pieces of data that may have many variants. Based on the previous section we know that the publisher Titan may be represented in a variety of ways. In order to explore this further we use the text filter to filter for all occurrences of Titan. Click the Publisher Column > Filter then type 'titan.'

A couple of interesting things result. We see at the top that 4684 rows match Titan. If we were going on the result from step 3 in the prior section we could have assumed that 'Titan' had only 3627 comics in the collection. Closer examination using the text filter function shows us how many records we might have missed as a result of typos and variant spelling.

At this point it is possible to remove some of the observed inconsistency by clicking the edit option within individual variant cells and applying that change to all identical cells.

#### Cluster
In addition to faceting and filtering it is also possible to cluster and normalize variation across the dataset. Clustering will look for patterns of variation without the need for you to (1) sleuth your way through the dataset looking for small variations (2) using facets or filters to eliminate them one at a time. Begin with the default method of 'key collision' using the 'fingerprint' function. Clustering reveals patterns of irregularity throughout the selected column of data. It is then possible to review clustering results and merge the data into the desired form. For more information on all of the available clustering methods and functions consult [OpenRefine documentation on Github](https://github.com/OpenRefine/OpenRefine/wiki/Clustering-In-Depth).

1. Click Publisher > Edit cells > Cluster and edit...
2. Review the proposed merges. Do they make sense?
3. Select a few suggestions that seem right and click 'Merge & Re-Cluster' to edit hundreds of values at once.

#### Transform Data
There are two different ways to transform data using OpenRefine. The first method allows you to utilize preset transformations that perform functions like trimming leading and trailing whitespace (an extra space at the beginning or end of an entry). This might seem like a trivial sort of data formatting issue but without this transformation each piece of data with an extra space at beginning or end would be interpreted as different from an otherwise identical piece of information (e.g. " Titan" and "Titan").

Click Publisher > Edit cells > Common transforms > Trim leading and trailing whitespace

The other method of transformation allows you to utilize the [OpenRefine Expression Language (GREL)](https://github.com/OpenRefine/OpenRefine/wiki/Understanding-Regular-Expressions) to normalize data. Let's say we want to remove the periods in the author names:

1. Click author-persons > Edit cells > Transform...
2. In the GREL window, type `value.replace('.','')`
3. Click ok.
> The replace function searches for the things in the first set of quotes & replaces it with what's in the second set

#### Joining a Dataset with Similar Information
1. In our `comic-publisher-info.csv` file, we have a list of publishers and the locations where they are located. These publisher names are also in our dataset, so we'd benefit from 'joining' that dataset and adding pertinent information.
1. Click 'Open' in the top right - this will open a new Open Refine tab
2. Load 'comic-publisher-info.csv' and create a project called 'locations'
3. Go back to the 'comics-metadata' project, click Publisher > Add column based on this column.
4. Call the new column 'PublisherLocation' and then in the GREL window, type `cell.cross("locations", "publishing_company")[0].cells["publisher_location"].value`. This looks to the "locations" project, looks at the "publishing_company" column in it, matches it to the "Publisher" column in our project, and then pulls the "publisher_location" into this new column. Click OK.
5. Not all rows have publishers that match, but the ones that do should now have a 'Publisher Location' name


#### Geocoding
*[Geocod.io](https://geocod.io/) may also be a good option*

*If you're doing this for your own project, you'll need to get a MapQuest API Key from the [MapQuest Developer Site](https://developer.mapquest.com/) - click the 'Get your Free API Key' button on the front page and fill out the information. For this workshop, I'm making a key available, and will cancel the key after our session*

*We'll also just do a few rows, since this can take awhile*

- Once you have an API key, click Author-Persons > Filter and type in 'Aaron, Jason'
- Location > Edit Column > Add Column by Fetching URLs... and enter this expression: `'http://open.mapquestapi.com/nominatim/v1/search.php?' + 'key=jGWdEphQshWla0lvxcJr1KBsR3BM7eX9&' + 'format=json&' + 'q=' + escape(value, 'url')` **Note: In the future, be sure to add your own API key in the above expression where it says `key=`**
- Name the column 'geocodingResponse' and click OK. This will take quite some time to finish.
- The new 'geocodingResponse' column won't be very clear or useful - it will be the full JSON response with all of the information Google has about that location.
- Click geocodingResponse > Edit Column > Add Column based on this column
- Enter `value.parseJson()[0].lat + ', ' + value.parseJson()[0].lon` and call the new column 'latlng.' Hit OK. This will parse the JSON and correctly format the latitute and longitude in the new column.
- You should see that the resulting column has the latitude and longitude for the city name.
- You can delete the 'geocodingResponse' column (Edit Column > Remove This Column) after you have already extracted the lat/lng coordinates.

> Some software will want latitude and longitude separately. If that's the case, Edit latlng Column > Split into several columns... and then split by the substring ","

#### Saving and Exporting
In the top right corner, you can click on 'Export' and save the data in a number of different formats, including csv and HTML tables.

You may also want to export the entire project. This is useful if you want to share the project with others, or if you want to continue working on a different machine. It's also useful for transparency and documentation, as every change you've made is documented (and reversible).

## Additional OpenRefine Resources
- [OpenRefine Wiki](https://github.com/OpenRefine/OpenRefine/wiki)
- [OpenRefine Recipes](https://github.com/OpenRefine/OpenRefine/wiki/Recipes)
- [Cleaning Data with OpenRefine](https://libjohn.github.io/openrefine/)
- [Fetching and Parsing Data from the Web with OpenRefine](https://programminghistorian.org/lessons/fetch-and-parse-data-with-openrefine)
