---
layout: model
title: Italian BertForQuestionAnswering model (from mrm8488)
author: John Snow Labs
name: bert_qa_bert_italian_finedtuned_squadv1_it_alfa
date: 2022-06-02
tags: [it, open_source, question_answering, bert]
task: Question Answering
language: it
edition: Spark NLP 4.0.0
spark_version: 3.0
supported: true
article_header:
  type: cover
use_language_switcher: "Python-Scala-Java"
---

## Description

Pretrained Question Answering model, adapted from Hugging Face and curated to provide scalability and production-readiness using Spark NLP. `bert-italian-finedtuned-squadv1-it-alfa` is a Italian model orginally trained by `mrm8488`.

{:.btn-box}
<button class="button button-orange" disabled>Live Demo</button>
<button class="button button-orange" disabled>Open in Colab</button>
[Download](https://s3.amazonaws.com/auxdata.johnsnowlabs.com/public/models/bert_qa_bert_italian_finedtuned_squadv1_it_alfa_it_4.0.0_3.0_1654182311430.zip){:.button.button-orange.button-orange-trans.arr.button-icon}

## How to use



<div class="tabs-box" markdown="1">
{% include programmingLanguageSelectScalaPythonNLU.html %}
```python
document_assembler = MultiDocumentAssembler() \ 
    .setInputCols(["question", "context"]) \
    .setOutputCols(["document_question", "document_context"])

spanClassifier = BertForQuestionAnswering.pretrained("bert_qa_bert_italian_finedtuned_squadv1_it_alfa","it") \
    .setInputCols(["document_question", "document_context"]) \
    .setOutputCol("answer") \
    .setCaseSensitive(True)

pipeline = Pipeline().setStages([
    document_assembler,
    spanClassifier
])

example = spark.createDataFrame([["What's my name?", "My name is Clara and I live in Berkeley."]]).toDF("question", "context")

result = pipeline.fit(example).transform(example)
```
```scala
val document = new MultiDocumentAssembler()
  .setInputCols("question", "context")
  .setOutputCols("document_question", "document_context")

val spanClassifier = BertForQuestionAnswering
  .pretrained("bert_qa_bert_italian_finedtuned_squadv1_it_alfa","it")
  .setInputCols(Array("document_question", "document_context"))
  .setOutputCol("answer")
  .setCaseSensitive(true)
  .setMaxSentenceLength(512)

val pipeline = new Pipeline().setStages(Array(document, spanClassifier))

val example = Seq(
  ("Where was John Lenon born?", "John Lenon was born in London and lived in Paris. My name is Sarah and I live in London."),
  ("What's my name?", "My name is Clara and I live in Berkeley."))
  .toDF("question", "context")

val result = pipeline.fit(example).transform(example)
```
</div>

{:.model-param}
## Model Information

{:.table-model}
|---|---|
|Model Name:|bert_qa_bert_italian_finedtuned_squadv1_it_alfa|
|Compatibility:|Spark NLP 4.0.0+|
|License:|Open Source|
|Edition:|Official|
|Input Labels:|[sentence, token]|
|Output Labels:|[embeddings]|
|Language:|it|
|Size:|410.2 MB|
|Case sensitive:|true|
|Max sentence length:|512|

## References

- https://huggingface.co/mrm8488/bert-italian-finedtuned-squadv1-it-alfa
- https://github.com/crux82/squad-it/blob/master/README.md#evaluating-a-neural-model-over-squad-it
- https://twitter.com/mrm8488
- https://link.springer.com/chapter/10.1007/978-3-030-03840-3_29
- https://github.com/crux82/squad-it
- https://rajpurkar.github.io/SQuAD-explorer/
- https://www.linkedin.com/in/manuel-romero-cs/