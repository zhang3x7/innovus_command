####1 Use Innovus to restore the original design database
```
source original.enc  (restoreDesign original.enc.dat)
```
####2 Setting the eco mode
```
setEcoMode -LEQCheck true -spreadInverter true -updateTiming false -honorPowerIntent true
```
####3 Delete the filler cells or notch fill (if present)
```
deleteFiller -prefix FILL
deleteNotchFill
deleteMetalFill -mode all
```
####4 Reading in the new design verilog 
```
ecoDesign -noEcoPlace -noEcoRoute original.enc.dat <top_cell_name> newDesign.v
```
####5 eco Place
```
ecoPlace -fixPlacedInsts -timing_driven true
```
####6 eco route
```
ecoRoute -target -modifyOnlyLayers bottomLayerName:topLayerName -prototype -fix_drc

ecoRoute -fix_drc
```
####7 add filler and metal fill
```
addFiller -cell {FILL4 FILL2 FILL 1} -prefix FILL

ecoRoute -fix_filler_drc_with_path_only -passive_fill

ecoRoute -fix_drc

set_metal_fill_signoff_mode \ To set HMF generation command -output_macros -die_area_as_boundary \ Options to support streamOut -rule_file \$pegasus_rule \ Pegasus Fill rule file and Map files -layer_map_file \$gds_map \ -units 1000 \ Units for streamOut (should match Fill rule file) -trim_mf \$trim_rule \ -excl_area_size \$exclArea -and_area_size \$andArea

report_resource -start hmf_mf

add_metal_fill_signoff -fill -skip_map_check

report_resource -end hmf_mf
```
####8 timing design
```
signoffTimeDesign -reportOnly -prefix metal_fill
```
####9 Save design database
```
saveDesign eco.enc.dat
```
