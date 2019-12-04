# How to make predictions on a metagenome dataset

This tutorial assumes that fastq/fasta files have been converted to [TFRecord](https://github.com/MicrobeLab/DeepMicrobes/blob/master/document/tfrec.md).

To get a full list of all options for `DeepMicrobes.py`:

```sh
python /path/to/DeepMicrobes.py --helpfull
```

The shell scripts used to make predictions with all tested DNNs in the paper can be found in [pipelines](https://github.com/MicrobeLab/DeepMicrobes/tree/master/pipelines). 
In these scripts we use the `--model_name` option to tell `DeepMicrobes.py` which DNN architecture we would like to use. <br>

<br>

<b>The final best DNN:</b>
* `attention`: Embed + LSTM + Attention (DeepMicrobes) <br>
<b>Other tested DNNs:</b>
* `deep_cnn`: ResNet-like CNN
* `cnn_lstm`: CNN + LSTM
* `seq2species`: Seq2species
* `embed_pool`: Embed + Pool
* `embed_cnn`: Embed + CNN
* `embed_lstm`: Embed + LSTM

<br>

## Classifying reads using DeepMicrobes

To make prediction on a metagenome dataset (referred to as `sample.tfrec`) using DeepMicrobes :
```sh
predict_deepmicrobes.sh -i sample.tfrec -b ${batch_size} -l ${level} -p ${cpus} -m ${model_dir} -o ${output_prefix}
```

Arguments: <br>
* `-i` TFRecord input containing interleaved paired-end reads <br>
* `-m` The dictionary containing model weights, should match the taxonomic level <br>
* `-o` Prefix of output file <br>
* `-b` (Optional) Batch size, should be a multiple of 4 (default: 8192) <br>
* `-l` (Optional) Taxonomic level, species/genus, should match the weights (default: species) <br>
* `-p` (Optional) Number of parallel calls for input preparation (default: 8) <br>


<b>Note</b>: The model classifies sequences faster using a larger batch size. 
We recommend users to try different values and select the largest batch size that fits into memory. 

The script takes as input a TFRecord dataset and generates a tab-delimited output file containing predictions made on each pair of reads. 
* 1st column: category labels (integer)
* 2nd column: confidence score (decimal)

The tab-delimited file can then be used to generate a species/genus profile (see [next tutorial](https://github.com/MicrobeLab/DeepMicrobes/blob/master/document/profile.md)).


