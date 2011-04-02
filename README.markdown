# Gibberish - Encryption in Ruby made simple

### What

Gibberish is an opinionated cryptography library for Ruby. Its objective is easy to use
encryption in Ruby while remaining secure.

### Why

While OpenSSL is an extremely capable encryption library, it lacks a terse and clean
interface in Ruby.

### Goals

- This library will also remain easily iteroperable with the OpenSSL command
line interface. Each function will include documentation on how to perform
the same routine via the command line with OpenSSL

- It should default to a reasonably secure setting, e.g. 256-bit AES, or SHA1 for HMAC
But it should allow the user to specify a stronger setting, within reason.

- Procedures should be well tested and be compatible with Ruby 1.8.7 and 1.9


## Requirements

Ruby compiled with OpenSSL support

## Installation

    gem install gibberish

## Digests

    Gibberish::MD5("somedata")
    #=> aefaf7502d52994c3b01957636a3cdd2

    Gibberish::SHA1("somedata")
    #=> efaa311ae448a7374c122061bfed952d940e9e37

    Gibberish::SHA256("somedata")
    #=> 87d149cb424c0387656f211d2589fb5b1e16229921309e98588419ccca8a7362

## AES

Defaults to 256 bit CBC encryption

    cipher = Gibberish::AES.new("p4ssw0rd")
    cipher.enc("Some top secret data")
    #=> U2FsdGVkX187oKRbgDkUcMKaFfB5RsXQj/X4mc8X3lsUVgwb4+S55LQo6f6N\nIDMX

    cipher.dec("U2FsdGVkX187oKRbgDkUcMKaFfB5RsXQj/X4mc8X3lsUVgwb4+S55LQo6f6N\nIDMX")
    #=> "Some top secret data"

Gibberish AES is fully compatible with default OpenSSL on the command line

    echo "U2FsdGVkX187oKRbgDkUcMKaFfB5RsXQj/X4mc8X3lsUVgwb4+S55LQo6f6N\nIDMX\n" | \
    openssl enc -d -aes-256-cbc -a -k p4ssw0rd

## HMAC

Defaults to 256 bit digest

    Gibberish::HMAC("key", "some data")
    #=> 521677c580722c5c52fa15d978e8656341c4f3c5

## RSA

    k = Gibberish::RSA.generate_keypair(1024)
    cipher = Gibberish::RSA.new(k.public_key)
    enc = cipher.encrypt("Some data")
    # Defaults to Base64 output
    #=> "JKm98wKyJljqmpx7kP8ZsdeXiShllEMcRHVnjUjc4ecyYK/doKAkVTLho1Gp\ng697qrljyClF0AcIH+XZmeF/TrqYUuCEUyhOD6OL1bs5dn8vFQefS5KdaC5Y\ndLADvh3mSfE/w/gs4vaf/OtbZNBeSl6ROCZasWTfRewp4n1RDmE=\n"
    cipher = Gibberish::RSA.new(k.private_key)
    dec = cipher.decrypt(enc)


## TODO

- Cover OpenSSL exceptions with more reasonable and easier to understand exceptions.
