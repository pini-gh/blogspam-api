# BlogSpam v3

I'm in the process of cutting down on the number of machines I host,
and one of the casualities is the [BlogSpam.net](https://blogspam.net/) service.

Rather than killing it outright I need to make it more efficient, such
that it can run upon an existing server I have, rather than requiring a dedicated machine of its own to run upon.  It has been suggested that I can reduce functionality, and port it to golang to make that happen.

> **NOTE** I did update the site to say the service will be retired, but I suspect nobody using the service will have noticed.  Oops.

I don't want to spend huge amounts of time on the process, as [the
existing code](https://github.com/skx/blogspam.js) is open-source and
available for users who wish to continue.  But I'm certainly willing to
spend a day or two trying to move it.

## Overview

The current codebase, version 2, is pretty simple:

* Read an incoming HTTP POST.
* The POST is a JSON object with a small number of fields:
    * Name
    * Email
    * Body
    * IP
    * etc
* Once the submission has been decoded from JSON invoke a series of plugins against it.
    * If any single plugin decides the submission was spam report that
    * Otherwise return a "good" result.

I'm not going to change this basic setup, so really I need a golang HTTP-server that listens for POST-submissions, decodes the JSON, and invokes a series of "plguins" on it.  The plugins won't be _real_ plugins, but self-contained test-code.


## Proof of Concept

This repository contains a proof of concept - it launches a service, reads the JSON-bodies, and invokes plugins.

I've ported several existing plugins and confirmed they pass the tests in the original repositories test-suit, and added 100% golang test-coverage.

The biggest difference in terms of code is that the existing nodejs codebase stores "bad" IPs in redis, and blocks them for 48hours.  Ideally I'd just skip that part.


## TODO

* Port more plugins
* Make a blog post
