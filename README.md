# Docs

The documentation serves as the primary source of information for individuals new to the SVX (Secure Value Exchange) platform. It explains the fundamental concepts of digital identity networks and highlights the specific role that Meeco's SVX platform plays within this ecosystem. The documentation serves as an essential resource for users to grasp the core concepts and functionalities of the SVX platform, empowering them to leverage its capabilities effectively.

## Concepts

## Platform

## Tools

Meeco has made available for use some tools to make using the API-of-Me easier.

The Meeco CLI is available [here](https://github.com/Meeco/cli). Open the [Meeco CLI](tools/meeco-cli.md) section in this documentation for detailed installation instructions, and then you can get into the [Quickstart Guide](getting-started/quickstart.md) to create a User and an Item to save to your Vault.

We've also created an encryption library called `Cryppo` that makes using the encryption and decryption routines we recommend much easier. We've created a Javascript and Ruby library - the examples contained within this guide will focus on the JS version. Check out `Cryppo-JS` [here](https://github.com/Meeco/cryppo-js) and `Cryppo` with Ruby [here](https://github.com/Meeco/cryppo).

`cryppo-cli` is a tool that we've created that quickly makes you a Data Encryption Key, and let's you encode and decode information with it from the command line. We recommend using this to follow along with some of the examples on the site.

You can even use them to generate encrypted data to use in the calls to the Developer Portal API Sandbox

To read more about Cryppo and the Cryppo-CLI, open the Cryppo page in the Meeco Docs [here](tools/cryppo.md)

## Environments

Currently, we have the following environments setup:

* **sandbox**: allows you to experiment freely in an environment that is always updated to include the latest and the greatest functionalities.
* **pre-production**: mimics the production environment, allows you to perform regression tests.
* **production**: where the rubber hits the road.
