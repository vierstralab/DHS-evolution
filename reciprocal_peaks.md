## How to Identify Reciprocal Peaks Between Human and Mouse

### Plan
1. Map the human DHS index to mouse and the mouse DHS index to human.
2. Use `bedmap --indicator` to check if mapped peaks intersect with indexes.
3. Analyze in Python which peaks are reciprocal.

### Procedures

#### Mapping Mouse DHS to Human Coordinates
```bash
"/home/aabisheva/anaconda3/bin/bnMapper.py" \
    "/net/seq/data2/projects/ENCODE4Plus/indexes/index_altius_mouse_24-04-17/output/masterlist.only_autosomes.filtered.bed" \
    "/net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/cactus447way_chain/Homo_sapiens-to-Mus_musculus.chain2" \
    -i BED -f BED12 -fBED12 --gap 20 --threshold 0.1 > \
    "/net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/cactus447way_reciprocal_mapping/human_to_mouse_nasi_index_april.txt"
```

#### Mapping Human DHS to Mouse Coordinates
Performed in batch for all species using `/net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/bnmapper_batch2.sh`, with results located at:
```plaintext
/net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/cactus447way_bnmapper_april_index
```

#### Sorting Data Before Using `bedmap --indicator`
- **Mouse Masterlist:**
    ```bash
    module load bedops/2.4.26
    cp /net/seq/data2/projects/ENCODE4Plus/indexes/index_altius_mouse_24-04-17/output/masterlist.only_autosomes.filtered.bed \
    /net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/cactus447way_reciprocal_mapping/mouse.masterlist.only_autosomes.filtered.bed
    sort-bed /net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/cactus447way_reciprocal_mapping/mouse.masterlist.only_autosomes.filtered.bed > \
    /net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/cactus447way_reciprocal_mapping/mouse.masterlist.only_autosomes.filtered_sorted.bed
    ```

- **Human Masterlist:**
    ```bash
    sort-bed /net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/masterlist_DHSs_Altius.filtered.04_25.bed > \
    /net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/cactus447way_reciprocal_mapping/human_masterlist.only_autosomes.filtered.sorted.bed
    ```

- **bnMapper Results:**
    ```bash
    sort-bed /net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/cactus447way_reciprocal_mapping/human_to_mouse_nasi_index_april.txt > \
    /net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/cactus447way_reciprocal_mapping/human_to_mouse_nasi_index_april_sorted.txt
    sort-bed /net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/cactus447way_bnmapper_april_index/Homo_sapiens-to-Mus_musculus_swapped_mapped.txt > \
    /net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/cactus447way_reciprocal_mapping/Homo_sapiens-to-Mus_musculus_swapped_mapped_sorted.txt
    ```

#### Using `bedmap --indicator`
```bash
bedmap --ec --delim '\t' --echo --indicator --echo-map-id \
/net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/cactus447way_reciprocal_mapping/human_to_mouse_nasi_index_april_sorted.txt \
/net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/cactus447way_reciprocal_mapping/human_masterlist.only_autosomes.filtered.sorted.bed > \
/net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/cactus447way_reciprocal_mapping/intersection.txt

bedmap --ec --delim '\t' --echo --indicator --echo-map-id \
/net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/cactus447way_reciprocal_mapping/Homo_sapiens-to-Mus_musculus_swapped_mapped_sorted.txt \
/net/seq data2/projects/aabisheva/DHS_evolution/benchmark/hg38/cactus447way_reciprocal_mapping/mouse.masterlist.only_autosomes.filtered_sorted.bed > \
/net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/cactus447way_reciprocal_mapping/intersection_2.txt
```
#### Analyzing Reciprocal Peaks in the Jupyter Notebook
Use the "Cactus447way_april_index-latest_version_reciprocal_peaks" Jupyter notebook.

### Additional testing

To examine how these newly identified peaks intersect with reciprocal peaks identified in "Mouse Regulatory...", several methods can be employed. For instance, you can use the UCSC LiftOver tool:

- [UCSC LiftOver Tool](https://genome.ucsc.edu/cgi-bin/hgLiftOver)

Load the following file into the LiftOver tool:
```plaintext
/net/seq/data2/projects/jvierstra/mouse.human.resubmission/build/master-peaks-mapped/reciprocal-peaks.human.bed
```
Note that it uses the hg19 annotation. Convert it to hg38 and save the results to:
```plaintext
/net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/cactus447way_reciprocal_mapping/uscs_liftover_human_hg19_to_hg38.bed
```

Next, sort the converted file:
```bash
sort-bed /net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/cactus447way_reciprocal_mapping/uscs_liftover_human_hg19_to_hg38.bed >\
/net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/cactus447way_reciprocal_mapping/uscs_liftover_human_hg19_to_hg38_sorted.bed
```

Then use `bedmap --indicator` to identify intersections:
```bash
bedmap --ec --delim '\t' --echo  --indicator --echo-map-id \
/net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/cactus447way_reciprocal_mapping/uscs_liftover_human_hg19_to_hg38_sorted.bed \
/net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/cactus447way_reciprocal_mapping/new_approach_05_07/reciprocal-peaks.human.bed >\
/net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/cactus447way_reciprocal_mapping/uscs_liftover_intersect.txt
```

To analyze the intersections using bash:
```bash
awk '{sum += $5} END {print "Number of old reciprocal peaks (in hg19) which have intersection with new reciprocal peaks (in hg38) - another algorithm:", sum}' \
/net/seq/data2/projects/aabisheva/DHS_evolution/benchmark/hg38/cactus447way_reciprocal_mapping/uscs_liftover_intersect.txt
```
