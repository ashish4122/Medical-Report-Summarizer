**Medical Reports Summarizer**






Simple library and command line utility for extracting summary from HTML
pages or plain texts. The package also contains simple evaluation
framework for text summaries. 



## Installation

Make sure you have [Python](http://www.python.org/) 3.6+ and
[pip](https://crate.io/packages/pip/)
([Windows](http://docs.python-guide.org/en/latest/starting/install/win/),
[Linux](http://docs.python-guide.org/en/latest/starting/install/linux/))
installed. Run simply (preferred way):

```sh
$ [sudo] pip install sumy
$ [sudo] pip install git+git://github.com/miso-belica/sumy.git  # for the fresh version
```

## Usage

Thanks to some good soul out there, the easiest way to try sumy is in your browser at https://huggingface.co/spaces/issam9/sumy_space

Sumy contains command line utility for quick summarization of documents.

```sh
$ sumy lex-rank --length=10 --url=https://en.wikipedia.org/wiki/Automatic_summarization # what's summarization?
$ sumy lex-rank --language=uk --length=30 --url=https://uk.wikipedia.org/wiki/Україна
$ sumy luhn --language=czech --url=https://www.zdrojak.cz/clanky/automaticke-zabezpeceni/
$ sumy edmundson --language=czech --length=3% --url=https://cs.wikipedia.org/wiki/Bitva_u_Lipan
$ sumy --help # for more info
```

Various evaluation methods for some summarization method can be executed
by commands below:

```sh
$ sumy_eval lex-rank reference_summary.txt --url=https://en.wikipedia.org/wiki/Automatic_summarization
$ sumy_eval lsa reference_summary.txt --language=czech --url=https://www.zdrojak.cz/clanky/automaticke-zabezpeceni/
$ sumy_eval edmundson reference_summary.txt --language=czech --url=https://cs.wikipedia.org/wiki/Bitva_u_Lipan
$ sumy_eval --help # for more info
```

If you don't want to bother by the installation, you can try it as a container.

```sh
$ docker run --rm misobelica/sumy lex-rank --length=10 --url=https://en.wikipedia.org/wiki/Automatic_summarization
```

## Python API

Or you can use sumy like a library in your project. Create file `sumy_example.py` ([don't name it `sumy.py`](https://stackoverflow.com/questions/41334622/python-sumy-no-module-named-sumy-parsers-html)) with the code below to test it.

```python
# -*- coding: utf-8 -*-

from __future__ import absolute_import
from __future__ import division, print_function, unicode_literals

from sumy.parsers.html import HtmlParser
from sumy.parsers.plaintext import PlaintextParser
from sumy.nlp.tokenizers import Tokenizer
from sumy.summarizers.lsa import LsaSummarizer as Summarizer
from sumy.nlp.stemmers import Stemmer
from sumy.utils import get_stop_words


LANGUAGE = "english"
SENTENCES_COUNT = 10


if __name__ == "__main__":
    url = "https://en.wikipedia.org/wiki/Automatic_summarization"
    parser = HtmlParser.from_url(url, Tokenizer(LANGUAGE))
    # or for plain text files
    # parser = PlaintextParser.from_file("document.txt", Tokenizer(LANGUAGE))
    # parser = PlaintextParser.from_string("Check this out.", Tokenizer(LANGUAGE))
    stemmer = Stemmer(LANGUAGE)

    summarizer = Summarizer(stemmer)
    summarizer.stop_words = get_stop_words(LANGUAGE)

    for sentence in summarizer(parser.document, SENTENCES_COUNT):
        print(sentence)
```


