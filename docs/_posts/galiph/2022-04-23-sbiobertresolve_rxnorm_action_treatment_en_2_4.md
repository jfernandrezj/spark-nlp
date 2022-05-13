---
layout: model
title: Sentence Entity Resolver for RxNorm (Action / Treatment)
author: John Snow Labs
name: sbiobertresolve_rxnorm_action_treatment
date: 2022-04-23
tags: [licensed, rxnorm, entity_resolution, en, clinical, open_source]
task: Entity Resolution
language: en
edition: Spark NLP 3.5.1
spark_version: 2.4
supported: true
article_header:
  type: cover
use_language_switcher: "Python-Scala-Java"
---

## Description

This model maps clinical entities and concepts (like drugs/ingredients) to RxNorm codes using `sbiobert_base_cased_mli` Sentence Bert Embeddings. Additionally, this model returns actions and treatments of the drugs in `all_k_aux_labels` column.

## Predicted Entities

`RxNorm Codes`, `Action`, `Treatment`

{:.btn-box}
<button class="button button-orange" disabled>Live Demo</button>
<button class="button button-orange" disabled>Open in Colab</button>
[Download](https://s3.amazonaws.com/auxdata.johnsnowlabs.com/public/models/sbiobertresolve_rxnorm_action_treatment_en_3.5.1_2.4_1650722216601.zip){:.button.button-orange.button-orange-trans.arr.button-icon}

## How to use



<div class="tabs-box" markdown="1">
{% include programmingLanguageSelectScalaPythonNLU.html %}
```python
documentAssembler = DocumentAssembler()\
      .setInputCol("text")\
      .setOutputCol("ner_chunk")

sbert_embedder = BertSentenceEmbeddings\
      .pretrained('sbiobert_base_cased_mli', 'en','clinical/models')\
      .setInputCols(["ner_chunk"])\
      .setOutputCol("sbert_embeddings")

rxnorm_resolver = SentenceEntityResolverModel\
      .pretrained("sbiobertresolve_rxnorm_action_treatment", "en", "clinical/models") \
      .setInputCols(["ner_chunk", "sbert_embeddings"]) \
      .setOutputCol("rxnorm_code")\
      .setDistanceFunction("EUCLIDEAN")

pipelineModel = PipelineModel(
    stages = [
        documentAssembler,
        sbert_embedder,
        rxnorm_resolver
    ])

text = ["Zita 200 mg", "coumadin 5 mg", 'avandia 4 mg']
test_df= spark.createDataFrame(pd.DataFrame(text, columns=["text"]))

result= pipelineModel.transform(test_df)

result_df= result.select(F.explode(F.arrays_zip("ner_chunk.result", "rxnorm_code.result", "rxnorm_code.metadata")).alias("cols"))\
                 .select(F.expr("cols['0']").alias("ner_chunk"),
                        F.expr("cols['1']").alias("rxnorm_code"),
                        F.expr("cols['2']['all_k_aux_labels']").alias("mappings")).toPandas()

result_df["mappings"]= result_df.mappings.apply(lambda x: eval(x.split(":::")[0]))
result_df["action"]= result_df.mappings.apply(lambda x: x[list(x.keys())[0]]["action"])
result_df["treatment"]= result_df.mappings.apply(lambda x: x[list(x.keys())[0]]["treatment"])
```
```scala
val documentAssembler = DocumentAssembler()
      .setInputCol("text")
      .setOutputCol("ner_chunk")

val sbert_embedder = BertSentenceEmbeddings
      .pretrained("sbiobert_base_cased_mli", "en","clinical/models")
      .setInputCols(Array("ner_chunk"))
      .setOutputCol("sbert_embeddings")
    
val rxnorm_resolver = SentenceEntityResolverModel.pretrained("sbiobertresolve_rxnorm_action_treatment", "en", "clinical/models") \
      .setInputCols(Array("ner_chunk", "sbert_embeddings")) \
      .setOutputCol("rxnorm_code")\
      .setDistanceFunction("EUCLIDEAN")

val rxnorm_pipelineModel = new PipelineModel().setStages(Array(documentAssembler, sbert_embedder, rxnorm_resolver))

val light_model = LightPipeline(rxnorm_pipelineModel)

val result = light_model.fullAnnotate(Array("Coumadin 5 mg", "aspirin", "avandia 4 mg"))
```
</div>

## Results

```bash
|    | ner_chunk     |   rxnorm_code | action                                                   | treatment                                                                                                                                                       |
|---:|:--------------|--------------:|:---------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  0 | Zita 200 mg   |        104080 | ['Analgesic', 'Antacid', 'Antipyretic', 'Pain Reliever'] | ['Backache', 'Pain', 'Sore Throat', 'Headache', 'Influenza', 'Toothache', 'Heartburn', 'Migraine', 'Muscular Aches And Pains', 'Neuralgia', 'Cold', 'Weakness'] |
|  1 | coumadin 5 mg |        855333 | ['Anticoagulant']                                        | ['Cerebrovascular Accident', 'Pulmonary Embolism', 'Heart Attack', 'AF', 'Embolization']                                                                        |
|  2 | avandia 4 mg  |        261242 | ['Drugs Used In Diabets', 'Hypoglycemic']                | ['Diabetes Mellitus', 'Type 1 Diabetes Mellitus', 'Type 2 Diabetes']                                                                                            |

```

{:.model-param}
## Model Information

{:.table-model}
|---|---|
|Model Name:|sbiobertresolve_rxnorm_action_treatment|
|Compatibility:|Spark NLP 3.5.1+|
|License:|Open Source|
|Edition:|Official|
|Input Labels:|[sentence_embeddings]|
|Output Labels:|[rxnorm_code]|
|Language:|en|
|Size:|918.7 MB|
|Case sensitive:|false|