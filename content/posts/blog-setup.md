+++
date = '2025-12-16T15:31:48Z'
draft = false
title = 'A Censorship Resistant Blog'
+++

A blog that's simple to use, easy to read, but hard to take down.
<!--more-->

## What I wanted

- **Unkillable.** If someone gets annoyed, they shouldn’t be able to delete my site or stop others reading it.
- **Tools I believe in.** If I’m going to talk about decentralisation, I should use it.
- **Automated where possible.** Publishing should be frictionless to encourage more writing.

## The Stack

- **Hugo** for a static site - obvious choice, widely used
- **IPFS** for content-addressed hosting. Arweave and Swarm are also good options, but IPFS has great tooling for static sites
- **ENS** for the name.

There's nothing particularly innovative or unusual here, that's kind of the point.

There are platforms that do a lot of this work for you (Fleek is a good example), but I don't want to rely on any single vendor.
If any piece of the stack changes or disappears, I can move to something else.

## Hosting and pinning

In a perfect world I’d run my own IPFS node, pin my own content, and depend on nobody. I'm not going to do that - I have limited resources.

The point of this blog is to write, not to run infrastructure.

So I’m using external pinning/hosting (Storacha / Pinata). It’s not ideologically pure, but it’s good enough:

- If they disappear, my content still exists as CIDs.
- I can move pinning elsewhere later.
- I’m not blocked on running infra before I publish a single post.

Importantly, I'm not locked in to any of the services I'm using.

## The annoying bit: ENS updates

Currently, **each deployment produces a new CID**, which means my ENS `contenthash` needs updating.

I haven't found an elegant way to automate this, so I'm just manually updating my ENS record. It’s annoying, but it’s not a disaster.

## If you want this setup

The repo is public, so feel free to fork it and use it for your own blog. The README should be self-explanatory - I wrote it for my future self when I inevitably forget how everything works.
