# Publish and follow a feed

Iotic twins can have feeds which allow you to publish and follow data related to that twin.

In the previous step, you created a dozen twins, each with a feed. We can use spacectl to follow and publish
data to the feeds.

Remember also that we saved the output when we created the twins, so that we can use the twin IDs.
Let's use that output to create a couple of files.

`cat dozen_twins | grep CREATED | awk '{print $4 " testfeed"}' > dozen_twins_follow`{{execute}}

`cat dozen_twins | grep CREATED | awk '{print $4 " testfeed {\"hello\":\"world\"}"}' > dozen_twins_publish`{{execute}}

Don't worry if you don't understand all the above, we're just creating a list of twin feeds to follow, and a the second one
is a list of twin feeds along with some data to publish (`{"hello":"world}"`).

Now let's go!

First, lets setup something following the dozen twin feeds.

`cat dozen_twins_follow | ./iotic-spacectl follow --host plateng.iotics.space`{{execute T1}}

Now lets publish that data!

`cat dozen_twins_publish | ./iotic-spacectl publish --host plateng.iotics.space`{{execute T2}}

Will the data show up? The suspense!