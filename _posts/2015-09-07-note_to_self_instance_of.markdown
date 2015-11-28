---
layout: post
title:  "Note to self: RSpec's #instance_of"
date:   2015-09-03 12:39:00
---
When you're don't care what the values of a hash are, just that a hash is received(as opposed to nil), `instance_of(Hash)` is the value you pass into your mock.

I hope this saves you some searching in the future.
