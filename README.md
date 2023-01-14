# AIS: Attributable to Identified Sources

AIS is an evaluation framework for assessing whether the output of natural language models only contains information about the external world that is verifiable in source documents, or **attributable to identified sources**.

This repository contains annotations for model output on four source datasets (CNN/DM, QReCC, Wizard of Wikipedia, and ToTTo). We provide the model output and annotations. The detailed description of our evaluation methodology, including the annotation guidelines, annotation interfaces, and operational statistics are included in our [paper](https://arxiv.org/abs/2112.12870).

In order to obtain the model inputs and source documents, you will have to download the original datasets and map from our data to the input examples. Please see instructions in following sections for each data source.

# Data CSV Formats

Our data is stored in separate csv files based on the source task. All the csv files have the following fields:

- `ex-idx [text]` An identifier for mapping to the input example in the original data files (see the following sections for specific instructions for each dataset).
- `doc-url [text]` For CNN/DM and QReCC only — an identifier for the URL where the document is extracted from. See specific instructions for [CNN/DM](#CNN/DM-data) and [QReCC](#QReCC-data) .
- `doc-url-hash [text]` For CNN/DM only — an extra identifier for the URL (heximal formatted SHA1 hash of the URL). See [CNN/DM specific instructions](#CNN/DM-data).
- `model-name [text]` Name of the model used to generate the output.
- `output [text]` Model prediction based on the indicated document and conversational context (when applicable).
- `INT [int]` A binary annotator judgment (by majority out of 5) for interpretability of the output, given the conversational context (when applicable).
- `INT & AIS [int]` A binary annotator judgment (by majority out of 5) for attribution of the output, given the document. Attribution judgements were given only to outputs that were marked as `1` in the `INT` column. So, if `INT=0`, then this column is also marked `0`.
- `Flagged [int]` A binary annotator judgment (by majority out of 5) whether the task overall is malformed such that it is impossible to make interpretability and attribution judgments.
- `Q1 [text]` Response wording in the annotators interface selected by the majority of annotators when assessing interpretability.
- `Q2 [text]` Response wording in the annotators interface selected by the majority of annotators when assessing attribution.
- `Q1 agree [int]` Number of annotators that agree on the response when assessing interpretability (out of 5). This is `-1` if the annotation was flagged (i.e. cases when annotators didn't answer this question).
- `Q2 agree [int]` Number of annotators that agree on the response when assessing interpretability (out of 5). This is `-1` if the annotation was flagged or if the response was voted uninterpretable (i.e when annotators didn't answer this question).

# Instructions for accessing model inputs

## CNN/DM Data {#CNN/DM-data}

Each row in our csv file corresponds to a produced summary (`output`) by a certain model (`model-name`) with the consensus annotations by our annotators and a count of how many annotators voted for the consensus. In order to access the input article that was being summarized, you will have to download a copy of CNN/DM and match the rows using the “doc-url” and “doc-url-hash” fields in our csv files.

### Instructions for accessing the source articles
1. Download the CNN/DM summarization data (if downloading the raw articles, be sure to strip the articles of the gold summary highlights before using). Note: An example of instructions for correctly downloading and processing the articles can be found at https://github.com/abisee/cnn-dailymail.

2. Depending on the version of CNN/DM that you use, it may have identifiers for the articles stored in one of two ways that can be used to map to rows in our csv files:

 - Using the article URL: In each row in our csv files, we include a `doc-url` field which contains the URL of the original source article. For some versions of CNN/DM, this can be used to map to the correct article in CNN/DM data

 - Using the article ID: In each row in our csv files, we also include a `doc-url-hash` field which is the heximal formatted SHA1 hash of the URL. For some previously released versions of the CNN/DM dataset, this hashed version of the URL was used as an article id and can be used to map to the correct article in the CNN/DM data.

### Additional Notes
We also include an `ex-idx` field in our csv files. This corresponds to the row (starting from 0) in the CNN/DM test files that were provided by [this repo](https://github.com/abisee/cnn-dailymail/blob/master/url_lists/all_test.txt) for the test set examples.

### References

These annotations were made using the CNN/DM dataset, originally developed by Nallapati, Ramesh, Bowen Zhou, Caglar Gulcehre, and Bing Xiang. "Abstractive text summarization using sequence-to-sequence rnns and beyond." In Proceedings of the 20th SIGNLL Conference on Computational Natural Language Learning (2016).

For generating model output, we used publicly available models.

**Matchsum**: Zhong, Ming, Pengfei Liu, Yiran Chen, Danqing Wang, Xipeng Qiu, and Xuanjing Huang. "Extractive summarization as text matching." In Proceedings of ACL (2020).

**Pointer-Generator Networks**: See, Abigail, Peter J. Liu, and Christopher D. Manning. "Get to the point: Summarization with pointer-generator networks." In Proceedings of ACL (2017).

**BigBird**: Zaheer, Manzil, Guru Guruganesh, Kumar Avinava Dubey, Joshua Ainslie, Chris Alberti, Santiago Ontanon, Philip Pham et al. "Big bird: Transformers for longer sequences." Advances in Neural Information Processing Systems 33 (2020).

## QReCC Data {#QReCC-data}

Each row in our csv file corresponds to a produced summary (`output`) by a certain model (`model-name`) with the consensus annotations by our annotators and a count of how many annotators voted for the consensus. In order to access the input document passage and the conversation history, you will have to download a copy of [QReCC data](https://github.com/scai-conf/SCAI-QReCC-21/#data) and match the rows using the `doc-url` and `ex-idx` fields in our csv file.

### Instructions on how to access the conversational contexts

1. Download the QReCC data (preferably the version included in the [SCAI QReCC 2022 challenge data](https://github.com/scai-conf/SCAI-QReCC-21/#data) which is also accessible [here](https://zenodo.org/record/5760304#.Y5jugezMJUP) with instructions for processing).
2. The `ex-idx` field that we provide in our csv file can be used to lookup the full conversation information in the QReCC data. The format of the `ex-idx` field is `Conversation_no:Turn_no` where the `Conversation_no` and `Turn_no` correspond to fields in the `qrecc_test.json` file provided by the SCAI QRECC 2022 challenge data.

### Instructions on how to download the grounding document passages

1. Download the QReCC data (preferably the version included in the [SCAI QReCC 2022 challenge data](https://github.com/scai-conf/SCAI-QReCC-21/#data) which is also accessible [here](https://zenodo.org/record/5760304#.Y5jugezMJUP) with instructions for processing).

2. To find the appropriate source passage, use the passage identifier that we provide in the `doc-url` field. The identifiers consist of a *URL* where the passage is located appended with a *passage marker* (`_p0`, `_p1`, `_p2`, etc.) denoting which passage was selected from the html page as the source. Please see the SCAI QRECC challenge set code for how to properly extract the corresponding passage using this information. Note: The original QRECC data sometimes lists multiple passage candidates for each utterance, so please refer to our `doc-url` field to verify which passage we used.

### References

These annotations were made using the QReCC data was originally developed by Anantha, Raviteja, Svitlana Vakulenko, Zhucheng Tu, Shayne Longpre, Stephen Pulman, and Srinivas Chappidi. "Open-domain question answering goes conversational via question rewriting." In Proceedings of NAACL (2020).

Models used were **variants of T5**: Raffel, Colin, Noam Shazeer, Adam Roberts, Katherine Lee, Sharan Narang, Michael Matena, Yanqi Zhou, Wei Li, and Peter J. Liu. "Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer." Journal of Machine Learning Research (2020).

## Wizard of Wikipedia Data

Each row in our csv file corresponds to a produced summary (`output`) by a certain model (`model-name`) with the consensus annotations by our annotators and a count of how many annotators voted for the consensus. In order to access the input document passage and the conversation history, you will have to download a copy of Wizard of Wikipedia and match the rows using the `ex-idx` field in our csv file.

### Instructions on accessing conversational contexts and document passages

1. Raw Wizard of Wikipedia json files can be downloaded directly from http://parl.ai/downloads/wizard_of_wikipedia/wizard_of_wikipedia.tgz (or implicitly using Parlai library). All of our examples come from the `test_random_split.json` file.

2. Use the `ex-idx` field of our csv files to access the conversation history and source passage:

 - Extract `convo_no` and `turn_no` from the `ex-idx` field. The `ex-idx` field is formatted as `convo_no:turn_no`. (For example, `0:0` is the first turn of the first conversation in the json file. `0:1` is the second turn of the first conversation, etc.). These can be used to index the list of conversation turns in the Wizard of Wikipedia json file. For example, the json file can be indexed:

 ```
 wow_json[convo_no]['dialog'][turn_no]
 ```

 - The conversation history can be accessed by iterating through the “text” field in all of the previous turns:

 ```
 [ wow_json[convo_no]['dialog'][i]['text'] for all i < turn_no ]
 ```

 - The document passage: can be accessed via the checked sentence field in the json:

 ```
 wow_json[convo_no]['dialog'][turn_no]['checked_sentence']
 ```

### Additional notes

For more details about the raw Wizard of Wikipedia json format, please reference the bottom section of the Wizard of Wikipedia documentation: https://parl.ai/projects/wizard_of_wikipedia/.

### References

Wizard of Wikipedia data was originally created for the paper: Dinan, Emily, Stephen Roller, Kurt Shuster, Angela Fan, Michael Auli, and Jason Weston. "Wizard of Wikipedia: Knowledge-Powered Conversational Agents." In International Conference on Learning Representations. (2018).

For producing model output we used models from:

- Dinan, Emily, Stephen Roller, Kurt Shuster, Angela Fan, Michael Auli, and Jason Weston. "Wizard of Wikipedia: Knowledge-Powered Conversational Agents." In International Conference on Learning Representations. (2018).
- Shuster, Kurt, Da Ju, Stephen Roller, Emily Dinan, Y-Lan Boureau, and Jason Weston. "The Dialogue Dodecathlon: Open-Domain Knowledge and Image Grounded Conversational Agents." In Proceedings of the 58th Annual Meeting of the Association for Computational Linguistics, pp. 2453-2470. 2020.
- Raffel, Colin, Noam Shazeer, Adam Roberts, Katherine Lee, Sharan Narang, Michael Matena, Yanqi Zhou, Wei Li, and Peter J. Liu. "Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer." Journal of Machine Learning Research (2020)
- Rashkin, Hannah, David Reitter, Gaurav Singh Tomar and Dipanjan Das. “Increasing Faithfulness in Knowledge-Grounded Dialogue with Controllable Features.” In ACL (2021).

## ToTTo Data

Each row in our csv file corresponds to a produced summary (`output`) by a certain model (`model-name`) with the consensus annotations by our annotators and a count of how many annotators voted for the consensus. In order to access the input table, you will have to download a copy of the Totto dataset and match the examples using the `ex-idx` field in our csv file.

### Instructions on accessing the source table:

1. Instructions for downloading and processing the Totto source files can be found on their [git repo](https://github.com/google-research-datasets/ToTTo).

2. The tables can be accessed by matching the `ex-idx` field in our csv file with the `example_id` field in the Totto source files. For the model-written outputs, all examples are extracted from the Totto test set (`unlabelled_totto_test.json`). For the human-written outputs (`model-name` is `reference_final` and `reference_original`), the examples form from the Totto dev set data (`totto_dev.json`).

### Additional Notes

`reference_final` vs. `reference_original`: The Totto data includes multiple human-written sentences for each example including the raw table descriptor before it was cleaned by annotators (`original_sentence`) and the gold reference that has been cleaned by crowdworkers (`final_sentence`). In our paper, we only reported the results for annotating the AIS of the `final_sentence`, which is treated as the gold reference. However, in this data release, we have included annotations for both sets of human-written sentences (`model-name`denoted as `reference_original` and `reference_final`).

### References
ToTTo data was originally developed as part of this data: Parikh, Ankur P., Xuezhi Wang, Sebastian Gehrmann, Manaal Faruqui, Bhuwan Dhingra, Diyi Yang, and Dipanjan Das. "ToTTo: A controlled table-to-text generation dataset." In Proceedings of EMNLP (2020).

Model output comes from the **T5/ByT5 models** used for GEM:

- Gehrmann, Sebastian, Tosin Adewumi, Karmanya Aggarwal, Pawan Sasanka Ammanamanchi, Aremu Anuoluwapo, Antoine Bosselut, Khyathi Raghavi Chandu et al. "The GEM Benchmark: Natural Language Generation, its Evaluation and Metrics." In 1st Workshop on Natural Language Generation, Evaluation, and Metrics 2021, pp. 96-120. Association for Computational Linguistics, 2021.

- Xue, Linting, Aditya Barua, Noah Constant, Rami Al-Rfou, Sharan Narang, Mihir Kale, Adam Roberts and Colin Raffel. “ByT5: Towards a Token-Free Future with Pre-trained Byte-to-Byte Models.” Transactions of the Association for Computational Linguistics 10 (2021): 291-306.

- Raffel, Colin, Noam Shazeer, Adam Roberts, Katherine Lee, Sharan Narang, Michael Matena, Yanqi Zhou, Wei Li, and Peter J. Liu. "Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer." Journal of Machine Learning Research (2020).

# Note about Hallucinated Content

This data is intended for measuring the types of mistakes made by models when producing output that is not attributable to a source. As such, the model outputs include hallucinated or incorrect information about real-world entities (for example statements about public figures, organizations, people mentioned in CNN news stories, news writers etc.). Some model output examples also include misspellings (e.g. misspelled names) and formatting errors (e.g. unks, unicode issues). People intending to use this data for any other purposes aside from measuring attribution issues should be aware that the model output includes these types of hallucinations and that the model output should not be treated as factually correct information.

# Citations
If you use this data, please cite our paper:

```
@article{ais,
  title={Measuring Attribution in Natural Language Generation Models},
  author={Rashkin, Hannah and Nikolaev, Vitaly and Lamm, Matthew and Aroyo, Lora and Collins, Michael and Das, Dipanjan and Petrov, Slav and Tomar, Gaurav Singh and Turc, Iulia and Reitter, David},
  publisher={arXiv preprint arXiv:2105.00071},
  year={2021}
  url = {https://arxiv.org/abs/2112.12870}
}
```
