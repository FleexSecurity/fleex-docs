### Spawning new boxes

Once you have configured everything, the real fun begins.

To spawn a new fleet (aka, many boxes with starting with fleetname-box_id) run:
```
fleex spawn -n FLEET_NAME -c FLEET_COUNT
```
Remember that for any fleex subcommand, you can use the `-h` flag to get help regarding its usage.

Fleex will automatically wait until all boxes are ready before exiting.