# Publish and follow a feed

Iotic twins can have feeds which allow you to publish and follow data related to that twin.

In the previous step, you created a dozen twins, each with a feed. We can use spacectl to follow and publish
data to the feeds.

Remember also that we saved the output when we created the twins, so that we can use the twin IDs.
Let's use that output to create a couple of files.

`cat dozen_twins | grep CREATED | awk '{print $4 "\ttestfeed"}' > dozen_twins_follow`{{execute}}

`cat dozen_twins | grep CREATED | awk '{print $4 "\ttestfeed\t{\"hello\":\"world\"}"}' > dozen_twins_publish`{{execute}}

Don't worry if you don't understand all the above, we're just creating a list of twin feeds to follow, and a the second one
is a list of twin feeds along with some data to publish (`{"hello":"world}"`).

Now let's go!

First, lets setup something following the dozen twin feeds.

`cat dozen_twins_follow | ./iotic-spacectl follow --host plateng.iotics.space`{{execute T1}}

Now lets publish that data!

`cat dozen_twins_publish | ./iotic-spacectl publish --host plateng.iotics.space`{{execute T2}}

Will the data show up? The suspense! Switch back to our first terminal and you should see the data.

So we've learnt how to create a twin, a feed, publish and follow data on it.

## So how would you get data into Iotics?

1. Get the data from where it is (an API, csv, database, etc)
2. Convert it to json
3. Use `iotic-spacectl createtwins` to import it into your space
4. If you defined some feeds, use `iotic-spacectl publish` to publish new data to your twins feeds.
