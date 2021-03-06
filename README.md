Pre-trained ELMo Representations for Many Languages
===================================================

We release our ELMo representations trained on many languages
which helps us win the [CoNLL 2018 shared task on Universal Dependencies Parsing](http://universaldependencies.org/conll18/results.html)
according to LAS.

## Technique Details

We use the same hyperparameter settings as [Peters et al. (2018)](https://arxiv.org/abs/1802.05365) for the biLM
and the character CNN.
We train their parameters
on a set of 20-million-words data randomly
sampled from the raw text released by the shared task (wikidump + common crawl) for each language.
We largely based ourselves on the code of [AllenNLP](https://allennlp.org/), but made the following changes:

* We support unicode characters;
* We use the *sample softmax* technique
to make training on large vocabulary feasible ([Jean et al., 2015](https://arxiv.org/abs/1412.2007)).
However, we use a window of words surrounding the target word
as negative samples and it shows better performance in our preliminary experiments.

The training of ELMo on one language takes roughly 3 days on an NVIDIA P100 GPU.


## Downloads

|   |   |   |   |
|---|---|---|---|
| [Arabic](http://vectors.nlpl.eu/repository/hit/ar.model.tar.xz) | [Bulgarian](http://vectors.nlpl.eu/repository/hit/bg.model.tar.xz) | [Catalan](http://vectors.nlpl.eu/repository/hit/ca.model.tar.xz) | [Czech](http://vectors.nlpl.eu/repository/hit/cs.model.tar.xz)  |
| [Old Church Slavonic](http://vectors.nlpl.eu/repository/hit/cu.model.tar.xz) | [Danish](http://vectors.nlpl.eu/repository/hit/da.model.tar.xz) | [German](http://vectors.nlpl.eu/repository/hit/de.model.tar.xz) | [Greek](http://vectors.nlpl.eu/repository/hit/el.model.tar.xz) | 
| [English](http://vectors.nlpl.eu/repository/hit/en.model.tar.xz) | [Spanish](http://vectors.nlpl.eu/repository/hit/es.model.tar.xz) | [Estonian](http://vectors.nlpl.eu/repository/hit/et.model.tar.xz) | [Basque](http://vectors.nlpl.eu/repository/hit/eu.model.tar.xz) |
| [Persian](http://vectors.nlpl.eu/repository/hit/fa.model.tar.xz) | [Finnish](http://vectors.nlpl.eu/repository/hit/fi.model.tar.xz) | [French](http://vectors.nlpl.eu/repository/hit/fr.model.tar.xz) | [Irish](http://vectors.nlpl.eu/repository/hit/ga.model.tar.xz) | 
| [Galician](http://vectors.nlpl.eu/repository/hit/gl.model.tar.xz) | [Ancient_Greek](http://vectors.nlpl.eu/repository/hit/grc.model.tar.xz) | [Hebrew](http://vectors.nlpl.eu/repository/hit/he.model.tar.xz) | [Hindi](http://vectors.nlpl.eu/repository/hit/hi.model.tar.xz) | 
| [Croatian](http://vectors.nlpl.eu/repository/hit/hr.model.tar.xz) | [Hungarian](http://vectors.nlpl.eu/repository/hit/hu.model.tar.xz) | [Indonesian](http://vectors.nlpl.eu/repository/hit/id.model.tar.xz) | [Italian](http://vectors.nlpl.eu/repository/hit/it.model.tar.xz) |
| [Japanese](http://vectors.nlpl.eu/repository/hit/ja.model.tar.xz) | [Korean](http://vectors.nlpl.eu/repository/hit/ko.model.tar.xz) | [Latin](http://vectors.nlpl.eu/repository/hit/la.model.tar.xz) | [Latvian](http://vectors.nlpl.eu/repository/hit/lv.model.tar.xz) |
| [Dutch](http://vectors.nlpl.eu/repository/hit/nl.model.tar.xz) | [Norwegian-Bokmaal](http://vectors.nlpl.eu/repository/hit/nb.model.tar.xz) | [Norwegian-Nynorsk](http://vectors.nlpl.eu/repository/hit/nn.model.tar.xz) | [Polish](http://vectors.nlpl.eu/repository/hit/pl.model.tar.xz) | 
| [Portuguese](http://vectors.nlpl.eu/repository/hit/pt.model.tar.xz) | [Romanian](http://vectors.nlpl.eu/repository/hit/ro.model.tar.xz) | [Russian](http://vectors.nlpl.eu/repository/hit/ru.model.tar.xz) | [Slovak](http://vectors.nlpl.eu/repository/hit/sk.model.tar.xz) |
| [Slovenian](http://vectors.nlpl.eu/repository/hit/sl.model.tar.xz) | [Swedish](http://vectors.nlpl.eu/repository/hit/sv.model.tar.xz) | [Turkish](http://vectors.nlpl.eu/repository/hit/tr.model.tar.xz) | [Uyghur](http://vectors.nlpl.eu/repository/hit/ug.model.tar.xz) |
| [Ukrainian](http://vectors.nlpl.eu/repository/hit/uk.model.tar.xz) | [Urdu](http://vectors.nlpl.eu/repository/hit/ur.model.tar.xz) | [Vietnamese](http://vectors.nlpl.eu/repository/hit/vi.model.tar.xz) | [Traditional-Chinese](http://vectors.nlpl.eu/repository/hit/zht.model.tar.xz) |

The models are hosted on the [NLPL Vectors Repository](http://wiki.nlpl.eu/index.php/Vectors/home).

**ELMo for Simplified Chinese**

We also provided [simplified-Chinese ELMo](http://pbmpb9h15.bkt.gdipper.com/zhs.model.tar.xz).
It was trained on xinhua proportion of [Chinese gigawords-v5](https://catalog.ldc.upenn.edu/ldc2011t13),
which is different from the Wikipedia for traditional Chinese ELMo.

## Pre-requirements

* python 3.6 
* pytorch 0.4
* other requirements from allennlp

## Usage

First, after unzip the model, please change the `"config_path"` field in `${lang}.model/config.json`
to `${project_home}/configs/cnn_50_100_512_4096_sample.json`.

Then, prepare your input file in the [conllu format](http://universaldependencies.org/format.html), like
```
1   Sue    Sue    _   _   _   _   _   _   _
2   likes  like   _   _   _   _   _   _   _
3   coffee coffee _   _   _   _   _   _   _
4   and    and    _   _   _   _   _   _   _
5   Bill   Bill   _   _   _   _   _   _   _
6   tea    tea    _   _   _   _   _   _   _
```
Fileds should be separated by `'\t'`. We only use the second column and space (`' '`) is supported in
this field (for Vietnamese, a word can contains spaces).
Do remember tokenization!

When it's all set, run

```
python src/gen_elmo.py test \
    --input_format conll \
    --input /path/to/your/input \
    --model /path/to/your/model \
    --output_prefix /path/to/your/output \
    --output_format hdf5 \
    --output_layer -1
```

It will dump an hdf5 encoded `dict` onto the disk, where the key is `'\t'` separated
words in the sentence and the value is it's 3-layer averaged ELMo representation.
You can also dump the cnn encoded word with `--output_layer 0`,
the first layer of the LsTM with `--output_layer 1` and the second layer
of the LSTM with `--output_layer 2`.  
We are actively changing the interface to make it more adapted to the 
AllenNLP ELMo and more programmatically friendly.

## Training Your Own ELMo

Please run 
```
python src/biLM.py train -h
```
to get more details about the ELMo training. However, we
need to add that the training process is not very stable.
In some cases, we end up with a loss of `nan`. We are actively working on that and hopefully
improve it in the future.

## Citation

If our ELMo gave you nice improvements, please cite us.

```
@InProceedings{che-EtAl:2018:K18-2,
  author    = {Che, Wanxiang  and  Liu, Yijia  and  Wang, Yuxuan  and  Zheng, Bo  and  Liu, Ting},
  title     = {Towards Better {UD} Parsing: Deep Contextualized Word Embeddings, Ensemble, and Treebank Concatenation},
  booktitle = {Proceedings of the {CoNLL} 2018 Shared Task: Multilingual Parsing from Raw Text to Universal Dependencies},
  month     = {October},
  year      = {2018},
  address   = {Brussels, Belgium},
  publisher = {Association for Computational Linguistics},
  pages     = {55--64},
  url       = {http://www.aclweb.org/anthology/K18-2005}
}
```

Please also cite the 
[NLPL Vectors Repository](http://wiki.nlpl.eu/index.php/Vectors/home)
for hosting the models.
```
@InProceedings{fares-EtAl:2017:NoDaLiDa,
  author    = {Fares, Murhaf  and  Kutuzov, Andrey  and  Oepen, Stephan  and  Velldal, Erik},
  title     = {Word vectors, reuse, and replicability: Towards a community repository of large-text resources},
  booktitle = {Proceedings of the 21st Nordic Conference on Computational Linguistics},
  month     = {May},
  year      = {2017},
  address   = {Gothenburg, Sweden},
  publisher = {Association for Computational Linguistics},
  pages     = {271--276},
  url       = {http://www.aclweb.org/anthology/W17-0237}
}
```
