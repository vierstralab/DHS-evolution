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

#### Analyzing Reciprocal Peaks in Jupyter Notebook
Use the "Cactus447way_april_index" Jupyter notebook, starting from the sections titled "Some Other Reciprocal Peaks Checks" and continue through the end of the notebook.
