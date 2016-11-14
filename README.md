# cosr-back

[![Chat with us on Slack](https://slack.commonsearch.org/badge.svg)](https://slack.commonsearch.org) [![Build Status](https://travis-ci.org/commonsearch/cosr-back.svg?branch=master)](https://travis-ci.org/commonsearch/cosr-back) [![Coverage Status](https://coveralls.io/repos/github/commonsearch/cosr-back/badge.svg?branch=master)](https://coveralls.io/github/commonsearch/cosr-back?branch=master) [![Apache License 2.0](https://img.shields.io/github/license/commonsearch/cosr-back.svg)](LICENSE)

This repository contains the main components of the [Common Search](https://about.commonsearch.org) backend.

Your help is welcome! We have a complete guide on [how to contribute](CONTRIBUTING.md).


## Understand the project

This repository has 4 components:

 - **cosrlib**: Python code for parsing, analyzing and indexing documents
 - **spark**: Spark jobs using cosrlib.
 - **urlserver**: A service for getting metadata about URLs from static databases
 - **explainer**: A web service for explaining and debugging results, hosted at [explain.commonsearch.org](https://explain.commonsearch.org/)

Here is how they fit in our [general architecture](https://about.commonsearch.org/developer/architecture):

![General technical architecture of Common Search](https://about.commonsearch.org/images/developer/architecture-2016-02.svg)


## Local install

A complete guide available in [INSTALL.md](INSTALL.md).


## Launching the tests

See [tests/README.md](tests/README.md).


## Using plugins

Common Search supports the insertion of user-provided plugins in its processing pipeline. Some are included by default, for instance:

```
make docker_shell
spark-submit spark/jobs/pipeline.py --source url:https://about.commonsearch.org/ --plugin plugins.grep.Words:words="common search",output=/tmp/grep_result
```

See the [plugins/](plugins/) directory for more examples and [Analyzing the web with Spark](https://about.commonsearch.org/developer/tutorials/analyzing-the-web-with-spark-on-ec2) for a complete tutorial.


## Launching the explainer

The explainer allows you to debug results easily. Just run:

```
make docker_explainer
```

Then open [http://192.168.99.100:9703](http://192.168.99.100:9703) in your browser (Assuming `192.168.99.100` is the IP of your Docker host)


## Launching an index job

```
make docker_shell
spark-submit spark/jobs/pipeline.py --source commoncrawl:limit=1 --plugin plugins.filter.Homepages:index_body=1 --profile
```

After this, if you have a `cosr-front` instance connected to the same Elasticsearch service, you will see the results!

A tutorial is currently being written on this topic.
