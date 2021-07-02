### Running distributed commands

When your fleet is ready, you can finally run distributed commands.

We will start with an example command:
```
fleex scan -n pwn -i /tmp/test-input -o scan-results.txt -c "sudo /usr/bin/massdns -r /home/op/lists/resolvers.txt -t A -o S {{INPUT}} -w {{OUTPUT}}"
```

This will send the `sudo /usr/bin/massdns -r /home/op/lists/resolvers.txt -t A -o S {{INPUT}} -w {{OUTPUT}}` command to all the boxes of your `pwn` fleet.

Note that those `{{INPUT}}` and `{{OUTPUT}}` are special labels that gets adjusted accordingly for each box.
Fleex will split the input file, in this case `/tmp/test-input` and send a chunk to each box.

Once every box has finished, all output files will be downloaded to your local machine and merged automatically: the final output will be the `scan-results.txt` file, the name passed to the `-o` flag.