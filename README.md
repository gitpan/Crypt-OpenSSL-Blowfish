# NAME

Crypt::OpenSSL::Blowfish - Blowfish Algorithm using OpenSSL

# SYNOPSIS

```perl
use Crypt::OpenSSL::Blowfish;

my $cipher = Crypt::OpenSSL::Blowfish->($key, {});

or (for compatibility.with 0.02 and below)

my $cipher = Crypt::OpenSSL::Blowfish->($key); # DON'T do this

my $ciphertext = $cipher->encrypt($plaintext);
my $plaintext = $cipher->decrypt($ciphertext);
```

# BLOWFISH IS CONSIDERED LEGACY BY OPENSSL v3

This module has been updated with support for OpenSSL v3 but should
be considered to be end of life.  OpenSSL requires the _legacy_ provider
to be specifically loaded to use Blowfish.  It is a deprecated encryption
module and you **need** to move to a different encryption algorithm.

# COMPATIBILITY WARNING

**WARNING** Version 0.02 and below **DO NOT** produce OpenSSL compatible
encryption or decrypt OpenSSL encrypted messages properly.

Basically, those versions call BF\_encrypt and BF\_decrypt without properly
converting the input to big endian and the output to little endian where
needed.

Version 0.03 and above correctly calls BF\_ecb\_encrypt and BF\_ecb\_decrypt
or the EVP\_\* functions but _DEFAULTS_ to the **incorrect** method for
compatibility.  This **MAY** change in subsequent versions.

To obtain the correct method you **must** provide a second option in the new
constructor.  Even an empty hash will work and will be compatible with later
versions.

```perl
my $cipher = Crypt::OpenSSL::Blowfish($key, {});
```

The encryption/decryption method is then compatible with the BF\_ecb\_encrypt
and BF\_ecb\_decrypt and well as the new EVP\_ methods using **EVP\_bf\_ecb** cipher
when compiled with openssl v3.

# DESCRIPTION

Crypt::OpenSSL::Blowfish implements the Blowfish Algorithm using functions
contained in the OpenSSL crypto library.  Crypt::OpenSSL::Blowfish has an
interface similar to Crypt::Blowfish, but produces different result than
Crypt::Blowfish. This is no longer correct if you use the new method to
instantiate Crypt::OpenSSL::Blowfish.  Using the new method results in
encryption which is compatible with Crypt::Blowfish.

# METHODS

## new($key, ... )

```perl
# New method - openssl compatible
my $cipher = Crypt::OpenSSL::Blowfish->new($key, {});

# Old method - NOT openssl compatible
my $cipher = Crypt::OpenSSL::Blowfish->new($key);
```

## encrypt(data)

```perl
my $encrypted = $cipher->encrypt($plaintext);
```

## encrypt(data)

```perl
my $decrypted = $cipher->decrypt($ciphertext);
```

## get\_little\_endian(data)

Converts the data to little-endian format. Required to convert the old
encryption to openssl compatible bf-ecb/EVP\_bf\_ecb.

See t/upgrade.t for examples to handle the endian conversion and encryption
upgrades.

```perl
my $leval = get_little_endian(data)
```

## get\_big\_endian(data)

Converts the data to little-endian format. Required to convert the old
encryption to openssl compatible bf-ecb/EVP\_bf\_ecb

See t/upgrade.t for examples to handle the endian conversion and encryption
upgrades.

```perl
my $beval = get_big_endian(data)
```

# SEE ALSO

[Crypt::Blowfish](https://metacpan.org/pod/Crypt%3A%3ABlowfish)

http://www.openssl.org/

# AUTHOR

Vitaly Kramskikh, <vkramskih@cpan.org>
Timothy Legge, <timlegge@cpan.org>

# LICENSE

No license was mentioned in the original version 0.01 or 0.02. Based
on the changes made by TIMLEGGE, 0.03 and forward will be as follows:

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself, either Perl version 5.8.5 or,
at your option, any later version of Perl 5 you may have available.

In the unlikely chance that that is an issue for anyone feel free to
contact TIMLEGGE
