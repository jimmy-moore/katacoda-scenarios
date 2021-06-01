# Create a twin

To get started with Iotics, you can use spacectl.

Spacectl is a general purpose swiss army knife of operating with an iotic space. 

You can download spacectl from https://nexus.cor.corp.iotic/#browse/browse:generic:iotic-spacectl
Run it on linux, mac or windows.

TODO: Is the download public?

## Configure

First, we need to tell spacectl to create a config file for future operations. We do this by
simply pointing spacectl at the space we wish to use.
If you have a space setup already eg `yourname.iotics.space`, you can use that instead in the following
examples.

So lets begin by creating a config file based on the space `plateng.iotics.space`.

`./iotic-spacectl init --host plateng.iotics.space`{{execute}}

Behind the scenes, spacectl created a new user identity and stored some secrets in the configuration file.
The default output location is `~/.spacectl.yaml` but you can specify this with `--config <file>` for example
if you have multiple spaces you wish to manage separately.

Lets take a very quick look at the config file
`cat .spacectl.yaml`{{execute}}

The identity configuration here means that anything you create from here on in will be owned by YOU. So keep
the config file safe!

## Create a twin

To create a twin, we need to give the twin some data. This is best done with json.
To make things a little easier, you can use the command `modeltwin` to ask simple questions and output json.

If you'd rather just skip ahead, you can skip this step and use the pre-generated `example-twin.json` file.

So lets create the json definition of a twin.

`./iotic-spacectl modeltwin`{{execute}}

You can now answer simple questions in the terminal to create the metadata for your twin.
For our first example, lets suppose we create a twin of our house.

### Comments

Twins can have one or more comments, in any language.

`Comment value [enter if no more]>` `This is my house`{{execute}}
`Comment language [default 'en']>` `^ENTER`{{execute ctrl-seq}}
`Comment value [enter if no more]>` `^ENTER`{{execute ctrl-seq}}

### Labels

Twins can have one or more labels, in any language.

`Label value [enter if no more]>` `My house`{{execute}}
`Label language [default 'en']>` ``{{execute}}
`Label value [enter if no more]>` ``{{execute}}

### Location

Twins can have a location.

`Location lat value [default 0]>` `51.5013631`{{execute}}
`Location lon value [default 0]>` `-0.1593983`{{execute}}

### Visibility

You can either keep your twin PRIVATE or make it PUBLIC.

`Visibility value [default PUBLIC]>` ``{{execute}}

### Properties

For now, to keep it simple, we won't add any properties.
TODO:?

`Key [enter if no more]>` ``{{execute}}

### Tags

Tags are useful for things like 'category' or other markers.

`Tag [enter if no more]>` `cat_house`{{execute}}
`Tag [enter if no more]>` ``{{execute}}

After running through the above, we should now have a json twin file ready to use.



There just so happens to be an example of what that might look like. Lets take a look!

`cat example-twin.json`{{execute}}

A line of json, and then a feedname at the end (TODO: This will change)

Lets get spacectl to create it!

`cat example-twin.json | ./iotic-spacectl createtwins --host plateng.iotics.space`{{execute}}

You might notice the `--host plateng.iotics.space` argument. This tells spacectl which space you want to create your twins in.
If you want to create several of each twin piped into spacectl, you can use `--num`. Lets try that!

`cat example-twin.json | ./iotic-spacectl createtwins --host plateng.iotics.space --num 12 | tee dozen_twins`{{execute}}

When your twin is created, it's given a unique ID. In the above line, we save the output into the file `dozen_twins` just so we keep
track of these twin ids.
To make some future steps easier, lets grab out the twin IDs from that output.

`cat dozen_twins | grep CREATED | awk '{print $4}' > dozen_twin_ids`{{execute}}

Congratulations! You just created your first 13 twins!

## Lookup twins

But how do we know they're actually there on the space? We can use describetwin with the IDs that we saved.

`cat dozen_twin_ids | ./iotic-spacectl describetwins --host plateng.iotics.space`{{execute}}

But what if we don't know the IDs? We can do a search. In the twin json, we defined a tag - "JMTWIN". Lets use that.
There's lots of ways to search so don't worry about the exact query syntax for now.

`echo {"filter": {"text": "JMTWIN"}, "responseType": "FULL"} | ./iotic-spacectl searchtwins --host plateng.iotics.space`{{execute}}

We also created a feed on each of these twins, so let's look at feeds next! Exciting times!
