---
layout: model
title: Part of Speech UD model for Norwegian (pos_nynorsklia)
author: John Snow Labs
name: pos_nynorsklia
date: 2022-04-01
tags: [open_source, universal_dependency, pos, part_of_speech, "no", norwegian]
task: Part of Speech Tagging
language: "no"
edition: Spark NLP 3.4.3
spark_version: 3.0
supported: true
article_header:
  type: cover
use_language_switcher: "Python-Scala-Java"
---

## Description

Pretrained Part of Speech model (`pos_nynorsklia`) trained on Universal Dependencies 2.9 (UD_Norwegian-NynorskLIA) in Norwegian language.

{:.btn-box}
<button class="button button-orange" disabled>Live Demo</button>
[Open in Colab](https://github.com/JohnSnowLabs/spark-nlp-workshop/blob/master/tutorials/streamlit_notebooks/GRAMMAR_EN.ipynb){:.button.button-orange.button-orange-trans.co.button-icon}
[Download](https://s3.amazonaws.com/auxdata.johnsnowlabs.com/public/models/pos_nynorsklia_no_3.4.3_3.0_1648798331595.zip){:.button.button-orange.button-orange-trans.arr.button-icon}

## How to use



<div class="tabs-box" markdown="1">
{% include programmingLanguageSelectScalaPythonNLU.html %}
```python
          
document = DocumentAssembler()\ 
    .setInputCol("text")\ 
    .setOutputCol("document")

sentence = SentenceDetectorDLModel.pretrained("sentence_detector_dl", "xx")\ 
    .setInputCols(["document"])\ 
    .setOutputCol("sentence")

tokenizer = Tokenizer()\ 
    .setInputCols(["sentence"])\ 
    .setOutputCol("token") 

pos = PerceptronModel.pretrained("pos_nynorsklia", "no")\ 
    .setInputCols(["sentence", "token"])\ 
    .setOutputCol("pos")
    
pipeline = Pipeline(stages=[document, sentence, tokenizer, pos])
    
data = spark.createDataFrame([["I love Spark NLP"]]).toDF("text")

result = pipeline.fit(data).transform(data)

```
```scala

val document = new DocumentAssembler()
  .setInputCol("text")
  .setOutputCol("document")

val sentence = SentenceDetectorDLModel.pretrained("sentence_detector_dl", "xx")
  .setInputCols("document")
  .setOutputCol("sentence")

val tokenizer = new Tokenizer() 
    .setInputCols("sentence") 
    .setOutputCol("token")
    
val pos = PerceptronModel.pretrained("pos_nynorsklia", "no")
    .setInputCols("sentence", "token")
    .setOutputCol("pos")
    
val pipeline = new Pipeline().setStages(Array(document, sentence, tokenizer, pos))

val data = Seq("I love Spark NLP").toDF("text")

val result = pipeline.fit(data).transform(data)
```
</div>

{:.model-param}
## Model Information

{:.table-model}
|---|---|
|Model Name:|pos_nynorsklia|
|Compatibility:|Spark NLP 3.4.3+|
|License:|Open Source|
|Edition:|Official|
|Input Labels:|[sentence, form]|
|Output Labels:|[pos]|
|Language:|no|
|Size:|551.6 KB|

## References

Model is trained on Universal Dependencies (treebank 2.9) `UD_Norwegian-NynorskLIA`

[https://github.com/UniversalDependencies/UD_Norwegian-NynorskLIA](https://github.com/UniversalDependencies/UD_Norwegian-NynorskLIA)