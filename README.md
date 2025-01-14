# PangPAG

[Bandage install](https://rrwick.github.io/Bandage/

[SequnceTubeMap (online demo)](https://vgteam.github.io/sequenceTubeMap/)

VG manual page: https://github.com/vgteam/vg/wiki/vg-manpage

```bash title="Converting files"
#Convert VG to GFA
vg convert --threads 8 --gfa-out --no-wline chr3.vg > chr3.gfa

#Convert GFA to VG
vg convert --threads 8 --gfa-in --vg-out chr3.gfa > chr3.vg
```

You can easily find coordinates of genes using NCBI's genome browser of the T2T Thoroughbred genome. For example, the MC1R gene is located at on chromosome 3 at the coordinates 36307397-36308540 in the new T2T genome.  
https://www.ncbi.nlm.nih.gov/gdv/browser/genome/?id=GCF_041296265.1
```bash title="Extract vg subgraph"
# extract by coordinates
vg chunk --xg-name chr3.vg --threads 8 --path RefT2Tv5#0#chr3:36307397-36308540 > MC1R.coords.vg
```

Alternatively, you can extract regions by the node ids if known. For example, if you find an interesting variant in the VCF file.
```
# extract by node ids
vg chunk --xg-name chr3.vg --threads 8 --node-range 60441628:60441631 > MC1R.nodes.vg
```


If you want to visualize your gam alignment files, then you must first prepare your game files with the script provided by their GitHub page. The script will output a sorted gam file in the same directory as your input file.
https://github.com/vgteam/sequenceTubeMap
```bash title="Prepare GAM files for SequenceTubeMap"
./sequenceTubeMap/scripts/prepare_gam.sh sample1.giraffe.gam
```

Now you can extract regions from your graph (vg) file and sorted gam files at the same time.
```bash title="Extract region from graph and gam files."
vg chunk -x chr3.vg\
 -a sample1.giraffe.sorted.gam\
 -a sample2.giraffe.sorted.gam\
 -a sample3.giraffe.sorted.gam\
 -a sample4.giraffe.sorted.gam\
 -a sample5.giraffe.sorted.gam\
 -g --context-steps 20 -t 8 -p RefT2Tv5#0#chr3:36307397-36308540 > MC1R.vg
```

