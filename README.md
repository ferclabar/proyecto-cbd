# Trabajo Fin de Grado Fernando Claros Barrero


## Converter to Parquet

Bitcoin stores all the transactions in the binary format described [here](https://webbtc.com/api/schema), however Spark is much better with columnar data formats such as Parquet, so we provide a simple converter from the Blockchain binary format into parquet files. The converter was inspired by the examples in the [hadoopcryptoledger](https://github.com/ZuInnoTe/hadoopcryptoledger/wiki/Using-Hive-to-analyze-Bitcoin-Blockchain-data) project and uses some of its classes for parsing the original format.

### Building

```bash
sbt clean assembly
```
will create a jar file in `./target/scala-2.11/bitcoin-insights.jar`

### Running

Converter assumes the binary block data in the input directory and writes parquet files to the output directory. Paths to input and output directories are passed to the converter. Depending on the size of the task, or in other words, how many blocks you want to convert, tweak the memory parameters of the following example command:
```bash
ls ~/bitcoin/input/
# blk00003.dat

spark-submit \
  --driver-memory 2G \
  --executor-memory 2G \
  --class io.radanalytics.bitcoin.ParquetConverter \
  --master local[8] \
  ./target/scala-2.11/bitcoin-insights.jar ~/bitcoin/input ~/bitcoin/output

# ... <output from the conversion> ...

ls ~/bitcoin/output
# addresses  blocks  edges  transactions
```

There are some prepared some bash scripts that can be used to automate the conversion tasks and make them idempotent: [run.sh](parquet-converter/run.sh), [run-new.sh](parquet-converter/run-new.sh),

## Notebook
```bash
cd notebook
make build
make run
```
