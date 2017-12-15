# Contribute to Simpla

Simpla is an open-source and community-driven project, and we'd love your help to push it forward!

There are lots of ways you can contribute to Simpla, from reporting issues, to contributing code in PRs, to creating your own components and adapters.

## How Simpla is organized

Simpla is a large project, spread across many repositories. All official code is on Github under the [simplajs](https://github.com/simplajs) organization.

### Core library

**Location:** [simplajs/simpla](https://github.com/simplajs/simpla)

Simpla itself is a tiny (~4kb) JavaScript library that provides a low-level API for components and deveopers to set, get, and manipulate JSON data based on a standard [content model](/guides/content-model.html). It also provides several [adapters](/guides/adapters.html) for content storage (eg: Github) and authentication (eg: NetlifyIdentity), as well as a standard interface for developers to create their own custom adapters.

### Official elements

**Location:** seperate repos under simplajs, eg: [simplajs/simpla-text](https://github.com/simplajs/simpla-text)

All official Simpla elements (eg: simpla-text, simpla-img) live in separate Github repos under the simplajs organisation. Each one has its own documentation, test suites, demos, issue trackers, etc.

### Element catalogue

**Location:** [simplajs/simpla-elements](https://github.com/simplajs/simpla-elements)

This is simply a listing of elements available for Simpla, both official and from the community. It doesn't hold any element code. You can submit your own elements to the catalogue by submitting a PR, after which they will show up in the collection on [webcomponents.org](https://www.webcomponents.org/collection/simplajs/simpla-elements).

### Documentation

**Location:** [simplajs/docs](https://github.com/simplajs/docs)

The documentation you're reading now is also open-source, see the [docs contributing guide](/contributing/docs.html) for more.

### Website

**Location:** [simplajs/simplajs.org](https://github.com/simplajs/simplajs.org)

Simpla's project website ([simplajs.org](https://www.simplajs.org)) is also open-source, and we happily welcome PRs and issues on it.

## Reporting issues

The easiest way to contribute to Simpla is by reporting issues on Github. Issues could be bugs, feature requests, or general feedback.

When creating an issue, especially if it's a bug, try to be as detailed as possible. Include the exact steps required to reproduce the problem, what browser(s) you experienced it on, any errors that were in your console, etc. There are issue templates on Github to help with this.

If you suspect the issue is part of the core library, submit it against [simplajs/simpla](https://github.com/simplajs/simpla). If you think it's due to an element, submit it against the relevant element repository. If you're not sure what's causing it just make your best guess. We can always move an issue if it proves to be coming from another part of the ecosystem.

## Submitting pull requests

We ❤️ pull requests! We happily accept PRs on all parts of the ecosystem, from the core library to the elements.

When creating your PR, try to keep the following guidelines in mind:

- The changes you are proposing should either address an existing issue or have been discussed with a maintainer previously, to make sure they're something the community wants
-  We prefer small focussed PRs addressing specific issues over large sweeping changes. If there are several changes you'd like to contribute, make multiple PRs
- Follow the provided PR template when submitting the actual pull request
- Try to follow the code style conventions already present in the code you are adding

## Creating elements

Coming soon...

## Creating adapters

Coming soon...

## Sharing the project

What if you don't have time to contribute code, and can't find any issues? Spread the word! Simpla lives or dies by the people using it, the more eyes we can get to the project the better.

Some ideas:
- Tweet about Simpla
- Share the project with a colleague
- Write a short post on Medium about how you've used Simpla in your own projects
- Share the code for a project you've built with the community
