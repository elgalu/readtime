# readtime

[![Tests](https://img.shields.io/github/actions/workflow/status/alanhamlett/readtime/tests.yml?branch=master)](https://github.com/alanhamlett/readtime/actions/workflows/tests.yml)
[![Coverage](https://codecov.io/gh/alanhamlett/readtime/branch/master/graph/badge.svg?token=EbUnuwbra3)](https://codecov.io/gh/alanhamlett/readtime)

Calculates the time some text takes the average human to read, based on Medium's [read time forumula](https://help.medium.com/hc/en-us/articles/214991667-Read-time).


### Algorithm

Medium's Help Center says,

> Read time is based on the average reading speed of an adult (roughly 265 WPM). We take the total word count of a post and translate it into minutes, with an adjustment made for images. For posts in Chinese, Japanese and Korean, it's a function of number of characters (500 characters/min) with an adjustment made for images.

Source: https://help.medium.com/hc/en-us/articles/214991667-Read-time (Read Sept 23rd, 2018)

Double checking with real articles, the English algorithm is:

    seconds = num_words / 265 * 60 + img_weight * num_images

With `img_weight` starting at `12` and decreasing one second with each image encountered, with a minium `img_weight` of `3` seconds.


### Installation

    virtualenv venv
    . venv/bin/activate
    pip install readtime

Or if you like to live dangerously:

    sudo pip install readtime


### Usage

Import `readtime` and pass it some text, HTML, or Markdown to get back the time it takes to read:

    >>> import readtime
    >>> result = readtime.of_text('The shortest blog post in the world!')
    >>> result.seconds
    2
    >>> result.text
    u'1 min'

The result can also be used as a string:

    >>> str(readtime.of_text('The shortest blog post in the world!'))
    u'1 min read'

To calculate read time of Markdown:

    >>> readtime.of_markdown('This is **Markdown**')
    1 min read

To calculate read time of HTML:

    >>> readtime.of_html('This is <strong>HTML</strong>')
    1 min read

To customize the WPM (default 265):

    >>> result = readtime.of_text('The shortest blog post in the world!', wpm=5)
    >>> result.seconds
    96
    >>> result.text
    u'2 min'
    >>> result.wpm
    5


### Contributing

Before contributing a pull request, make sure tests pass:

    virtualenv venv
    . venv/bin/activate
    pip install tox
    tox

Many thanks to all [contributors](https://github.com/alanhamlett/readtime/blob/master/AUTHORS)!
