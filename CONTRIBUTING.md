# Contributing to Stockpot

Thank you so much for your interest in contributing to Stockpot!

## Overview

Stockpot is a [Rails Engine](https://guides.rubyonrails.org/engines.html) which means that it's "considered miniature applications that provide functionality to their host applications." It can technically be run on its own via `rails s`, but it's meant to be run inside a larger app, so running it on its own won't really provide a lot of benefit, or really work correctly at the moment. Think of Stockpot as a helpful addon to an existing Rails app that unlocks a handful of API routes to use, saving time writing them yourself.

The project's repo will look _similar_ to a Rails app, though very pared down. There's an app directory, but inside you'll only find controllers. As Stockpot is just an API that returns JSON, there's no need for views, and, for now, there's also no need for models, so you won't find them there either.

## Working with the code

- Make sure to update the [CHANGELOG](CHANGELOG.md) with your changes.
  - Please follow the existing [Keep a Changelog](https://keepachangelog.com/en/1.0.0/) format
  - Add yourself to the contributors list at the bottom so that attribute links work
  - Attribute the changes you've made to yourself (and others if it was a group effort) by specifying the PR/Issue number(s) and your linked username like this: `([#1](https://github.com/Freshly/stockpot/pull/1) [jaysonesmith])`
- Add tests where applicable! We want to make sure that our changes don't detract from existing functionality and that other's can continue to rely on Stockpot for their needs.

## Releases

### How

If you have the ability to release new versions of Stockpot, here are the steps:

- Update the version number in [version.rb](./lib/stockpot/version.rb)
- Run `bundle` so that the [Gemfile.lock](Gemfile.lock) gets updated.
- Run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org). Remember to use the MAJOR.MINOR.PATCH formatting of [Semantic Versioning](https://semver.org/)

### Who

- [Freshly developers](https://github.com/Freshly/)
