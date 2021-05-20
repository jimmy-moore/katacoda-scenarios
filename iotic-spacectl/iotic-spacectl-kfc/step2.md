# Get all the data...

First lets get the index...

`curl https://ratings.food.gov.uk/open-data/en-gb -L -o index.html`{{execute}}

Now we need to grab out the URLs for all the different region downloads.
Why would they make it easy for us?!

`cat index.html | xml2 | grep OpenDataFiles | tr "@=" "  " | awk '{print $3}' > urls.txt`{{execute}}

And now, we need to download all of those xml files. We may as well also convert to a csv file as well.
This might take a little time (Couple of minutes)...

`cat urls.txt | awk '{system("curl -L " $1 " | xml2 | 2csv EstablishmentDetail Geocode/Longitude Geocode/Latitude BusinessName AddressLine1 PostCode >> food.csv")}'`{{execute}}

Now we have a list of food locations, and as we worked out in step1, we can convert them to twin json.

`cat food.csv | grep KFC | awk -F, '{print "{\"comments\": {\"added\": [{\"lang\": \"en\", \"value\": \"KFC " $4 " " $5 "\"}]}, \"labels\": {\"added\": [{\"lang\": \"en\", \"value\": \"KFC " $4 " " $5 "\"}]}, \"location\": {\"location\": {\"lat\": " $2 ", \"lon\": " $1 "}}, \"newVisibility\": \"PUBLIC\"}, \"properties\": {\"clearedAll\": false}, \"tags\": {\"added\": [\"KFC\", \"cat_misc\"]}}\ttestfeed"}' > kfc_twins`{{execute}}
