#### 1 How to deal with high fanout (unverified)
The common pratice of dealing with high fanout with innovus is to first split the fanout and then use buffer tree to deal with load

Run the following command before ecoPlace and ecoRoute

```
setOptMode -fixFanoutLoad true
optDesign -preCTS
```

if user already knows which ones are high fanout nets, then
```
setOptMode -fixFanoutLoad true
optDesign -preCTS -drv -selectedNets <filename_having_nets>
```
if some high fanout nets are left without processing after using the above command, then
```
addRepeater -preRoute -rule <rulefile> -selNet <selected_nets_file>
```
