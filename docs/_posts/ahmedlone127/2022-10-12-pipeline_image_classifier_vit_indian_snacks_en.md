---
layout: model
title: English pipeline_image_classifier_vit_indian_snacks ViTForImageClassification from thak123
author: John Snow Labs
name: pipeline_image_classifier_vit_indian_snacks
date: 2022-10-12
tags: [vit, en, images, open_source, pipeline]
task: Image Classification
language: en
edition: Spark NLP 4.2.1
spark_version: 3.0
supported: true
article_header:
  type: cover
use_language_switcher: "Python-Scala-Java"
---

## Description

Pretrained VIT  model, adapted from Hugging Face and curated to provide scalability and production-readiness using Spark NLP.`image_classifier_vit_indian_snacks` is a English model originally trained by thak123.


## Predicted Entities

`marker`, `pencil`, `pens`, `crayon`, `chalk`

{:.btn-box}
<button class="button button-orange" disabled>Live Demo</button>
<button class="button button-orange" disabled>Open in Colab</button>
[Download](https://s3.amazonaws.com/auxdata.johnsnowlabs.com/public/models/pipeline_image_classifier_vit_indian_snacks_en_4.2.1_3.0_1665534715249.zip){:.button.button-orange.button-orange-trans.arr.button-icon}

## How to use



<div class="tabs-box" markdown="1">
{% include programmingLanguageSelectScalaPythonNLU.html %}
```python

    pipeline = PretrainedPipeline('pipeline_image_classifier_vit_indian_snacks', lang = 'en')
    annotations =  pipeline.transform(imageDF)
    
```
```scala

    val pipeline = new PretrainedPipeline("pipeline_image_classifier_vit_indian_snacks", lang = "en")
    val annotations = pipeline.transform(imageDF)
    
```
</div>

{:.model-param}
## Model Information

{:.table-model}
|---|---|
|Model Name:|pipeline_image_classifier_vit_indian_snacks|
|Type:|pipeline|
|Compatibility:|Spark NLP 4.2.1+|
|License:|Open Source|
|Edition:|Official|
|Language:|en|
|Size:|321.9 MB|

## Included Models

- ImageAssembler
- ViTForImageClassification