# Import the twins into our space

## Init spacectl

`./iotic-spacectl init --host plateng.iotics.space`{{execute}}

## Import the twins

`cat kfc_twins | ./iotic-spacectl createtwins --host plateng.iotics.space | tee kfc_created_twins`{{execute}}

Now lets see if we can find them!

