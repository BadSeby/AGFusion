# Annotate Gene Fusion (AGFusion)
Python package that visualizes and annotates gene fusions. Currently, the software can only visualize the PFAM domains of in-frame gene fusions. The software can also output fasta files of the predicted cDNA, CDS, and protein sequences resulting from fusion of all combinations of transcripts.

# Example Usage

The minimum amount of information you need to provide are the two fusion gene partners (gene symbol or Ensembl ID) and their respective predicted fusion junctions in genomic coordinates.

Example usage from the command line:

```
./agfusion \
  --gene5prime ENSMUSG00000022770 \
  --gene3prime ENSMUSG00000002413 \
  --junction5prime 31684294 \
  --junction3prime 39648486 \
  --db agfusion.db \
  --genome GRCm38 \
  --out DLG1-BRAF
```

Example usage within a python script:

```
import agfusion
import pyensembl

data = pyensembl.EnsemblRelease(84,'mouse')
db = agfusion.AGFusionDB('../data/agfusion.db')

dlg1 = agfusion.Gene(
    gene="ENSMUSG00000022770",
    junction=31684294,
    db=db,
    pyensembl_data=data
)

braf = agfusion.Gene(
    gene="ENSMUSG00000002413",
    junction=39648486,
    db=db,
    pyensembl_data=data
)

#construct the gene fusion

fusion = agfusion.model.Fusion(dlg1,braf,db=db,middlestar=False)

#save the predicted cDNA, CDS, and protein sequences

fusion.save_transcript_cdna('DLG1-BRAF_mouse')
fusion.save_transcript_cds('DLG1-BRAF_mouse')
fusion.save_proteins('DLG1-BRAF_mouse')

#save the visualizations of the fusion domain structure

fusion.save_images('DLG1-BRAF_mouse')
```

Example output visualization of the domain structure of the DLG1-BRAF fusion:

![alt tag](https://github.com/murphycj/AGFusion/blob/master/test/DLG1-BRAF/ENSMUST00000023454-ENSMUST00000002487.png)

You can programmatically change domains names and colors:

```
./agfusion \
  --gene5prime ENSMUSG00000022770 \
  --gene3prime ENSMUSG00000002413 \
  --junction5prime 31684294 \
  --junction3prime 39648486 \
  --db agfusion.db \
  --genome GRCm38 \
  --out DLG1-BRAF \
  --colors Pkinase_Tyr:red \
  --rename Pkinase_Tyr:Kinase
```

![alt tag](https://github.com/murphycj/AGFusion/blob/master/test/DLG1-BRAF/ENSMUST00000132176-ENSMUST00000002487.renam.recolor.png)

# Dependencies

python 2.7.8
pyensembl>=0.9.5
matplotlib>=1.5.0
biomart>=0.9.0
pandas>=0.18.1
biopython>=1.67
mpld3>=0.2
jsonpickle>=0.9.

# Installation

```
pip install agfusion
```

After installing pyensembl you need to install the reference genome you will use.

For GRCh38:

```
pyensembl install --release 84 --species homo_sapiens
```

For GRCh37:

```
pyensembl install --release 75 --species homo_sapiens
```

For GRCm38:

```
pyensembl install --release 84 --species mus_musculus
```

# License

MIT license