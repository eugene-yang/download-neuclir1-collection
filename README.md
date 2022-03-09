# NeuCLIR Collection 2022

This repository contains the scripts for downloading and validating scripts for the documents. 
Document ids are stored in `resources/{lang}/ids.*.jsonl.gz`

Required packages for the scripts are recorded in `requirements.txt`. 

We recommand creating a new python environment for downloading. Package versions could have some unintentional effect on decoding 
the documents from Common Crawl. Please contact us if you have documents with mismatch hashs. 

## Download Documents

To download the documents from Common Crawl, please use the following command.
If you plan to use this collection with [`ir_datasets`](https://ir-datasets.com/), please specify `~/.ir_datasets/neuclir22` 
as the storage or make a soft link to to the directory you wish to store the documents. The document ids and hashs are 
stored in `resource/{lang}/ids.*.jsonl.gz`. 

```bash
python download_documents.py --storage ./data/ \
                             --zho ./resource/zho/ids.*.jsonl.gz \
                             --fas ./resource/fas/ids.*.jsonl.gz \
                             --rus ./resource/rus/ids.*.jsonl.gz \
                             --jobs 4 --check_hash
```

If you wish to only download the documents for one language, just specify the id file for the language
you wish to download. 
The full description of the arguments can be found when execute with the `--help` flag.

## Postprocessing of the Downloaded Documents

Multiprocessing during download results in arbitrary ordering of the documents in the saved `.jsonl` files. 
To support full reproducibility, we provide script to postprocess the file to match the document order specified in the document id files. 
`fix_document_order.py` changes the ordering of the documents, validates the document hashs, and verifies all and only specified documents are in 
the result file. The unsorted file will be renamed as `docs.jsonl.bak`. You could delete the file manually. Following is a sample command. 

```bash
python fix_document_order.py --raw_download_file ./data/{lang}/docs.jsonl \
                             --id_file ./resources/hc4/{lang}/ids.*.jsonl.gz \
                             --check_hash
```
