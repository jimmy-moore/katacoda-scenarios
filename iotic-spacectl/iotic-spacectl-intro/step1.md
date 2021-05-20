# Create a twin

Let's download one of these food rating things!

`wget https://ratings.food.gov.uk/OpenDataFiles/FHRS522en-GB.xml`{{execute}}

hmm it's xml. We'll have to do a bit to move it around...

`cat FHRS522en-GB.xml | xml2 | 2csv EstablishmentDetail Geocode/Longitude Geocode/Latitude BusinessName > Lambeth.csv`{{execute}}

In the above line, we're grabbing out a few fields we're interested in, and putting them in a csv so it's easier to use.

Lets see how this looks, and find the KFC outlets in Lambeth:

`cat Lambeth.csv | grep KFC`{{execute}}

Next we need to munge this around into some twin like data...

`cat Lambeth.csv | grep KFC | awk -F, '{print "{\"comments\": {\"added\": [{\"lang\": \"en\", \"value\": \"KFC\"}]}, \"labels\": {\"added\": [{\"lang\": \"en\", \"value\": \"$3\"}]}, \"location\": {\"location\": {\"lat\": $2, \"lon\":$1}}, \"newVisibility\": \"PUBLIC\"}, \"properties\": {\"clearedAll\": false}, \"tags\": {\"added\": [\"KFC\"]}}\ttestfeed"}' > kfc_twins`

OK, all looks good. Next lets get the data for the whole of the UK.