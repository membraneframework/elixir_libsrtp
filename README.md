# Elixir bindings for [libsrtp]

[![Hex.pm](https://img.shields.io/hexpm/v/srtp.svg)](https://hex.pm/packages/srtp)
[![API Docs](https://img.shields.io/badge/api-docs-yellow.svg?style=flat)](https://hexdocs.pm/elixir-libsrtp/)
[![CircleCI](https://circleci.com/gh/membraneframework/elixir-libsrtp.svg?style=svg)](https://circleci.com/gh/membraneframework/elixir-libsrtp)

## Installation

Firstly, install [libsrtp]. Then, add `libsrtp` to your list of dependencies in `mix.exs`:

```elixir
def deps do
  [
    {:libsrtp, "~> 0.1.0"}
  ]
end
```

## Usage

This library allows to encrypt plain RTP to SRTP and decrypt it back. The following snippet shows how to encrypt and decrypt a packet:

```elixir
iex> in_srtp = LibSRTP.new()
iex> LibSRTP.add_stream(in_srtp, %LibSRTP.Policy{ssrc: :any_inbound, key: "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"})
:ok
iex> packet = <<128, 14, 15, 143, 98, 145, 127, 247, 233, 164, 145, 140, 1, 2, 3, 4>>
iex> {:ok, protected_packet} = LibSRTP.protect(in_srtp, packet)
{:ok,
 <<128, 14, 15, 143, 98, 145, 127, 247, 233, 164, 145, 140, 112, 112, 222, 241, 148, 205, 10, 185, 78, 20, 27, 103, 2, 207>>}
iex> out_srtp = LibSRTP.new()
iex> LibSRTP.add_stream(out_srtp, %LibSRTP.Policy{ssrc: :any_outbound, key: "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"})
:ok
iex> {:ok, unprotected_packet} = LibSRTP.unprotect(out_srtp, protected_packet)
{:ok, <<128, 14, 15, 143, 98, 145, 127, 247, 233, 164, 145, 140, 1, 2, 3, 4>>}
iex> unprotected_packet == packet
true
```

## Copyright and License

Copyright 2020, [Software Mansion](https://swmansion.com/?utm_source=git&utm_medium=readme&utm_campaign=elixir-libsrtp)

[![Software Mansion](https://logo.swmansion.com/logo?color=white&variant=desktop&width=200&tag=membrane-github)](https://swmansion.com/?utm_source=git&utm_medium=readme&utm_campaign=elixir-libsrtp)

Licensed under the [Apache License, Version 2.0](LICENSE)

[libsrtp]: https://github.com/cisco/libsrtp
