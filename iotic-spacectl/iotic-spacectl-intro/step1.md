# Create a twin

To get started with Iotics, you can use spacectl.

## Configure

First, we need to point spacectl at a space, and tell it to configure itself.

`./iotic-spacectl init --host plateng.iotics.space`{{execute}}

In the above step, spacectl creates a configuration file which contains a new user identity. Keep it safe!
The default output location is `~/.spacectl.yaml` but you can specify this with `--config <file>`.

Lets take a very quick look at the config file
`cat .spacectl.yaml`{{execute}}

The identity configuration here means that anything you create from here on in will be owned by YOU.

## Create a twin

To create a twin, we need to give the twin some data. This is best done with json.
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
