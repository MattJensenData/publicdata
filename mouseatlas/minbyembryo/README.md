# Minimum Data, by EmbryoID

This data is a subset of the cell annotations from Cao, et al.

STEPS:

1. Start with cell_annotation.csv link from https://oncoscape.v3.sttrcancer.org/atlas.gs.washington.edu.mouse.rna/downloads.

2. Get numbered list of column names, so we can pick columns.

> head -n1 cell_annotate.csv | tr ',' "\n" | cat -n > colNames.txt

3. Pull out only the minimal fields of interest
   EmbryoId, development stage, cell type, and x,y,z of the cleaned up trajectory.
   
> cat cell_annotate.csv | grep -v 'NA NA '| tr ' ' '_'| awk -F',' '{print $6,$9,$23,substr($29,0,10),substr($30,0,10),substr($31,0,10)}' | tr ' ' ',' | tail --lines=+2 > concise_xyz_by_embryo.csv

4. mkdir byEmbryo

5. Split the rows up by emrbyo ID.

> cat concise_xyz_by_embryo.csv | awk -F',' '{print $0 > "byEmbryo/embryo_min"$1".csv"}'

6. Add some column names back in, as the first line, to each file.

> sed -i '1s;^;embryoId,stage,cellType,mtX,mtY,mtZ\n;' byEmbryo/*csv

The byEmbryo folder now has 68 CSV files, one per embryo.
