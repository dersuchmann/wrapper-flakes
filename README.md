#   Wrapper Flakes

Monorepo of shared flakes that my other projects can import.

I like to pin my flake inputs to fixed commits in `flake.nix` already.
This makes it easier to keep the number of nixpkgs versions used among my projects low,
which in turn prevents my Nix store from filling up with multiple versions of everything.

In a sense, this repo is a shared list of all the pinned flake inputs I use in my projects.
Each of these flakes is a thin wrapper around a pinned version of some other flake, mostly [NixOS/nixpkgs](https://github.com/NixOS/nixpkgs). 
This lets my projects pick inputs from a shared curated set of commits, rather than from thousands available overall.

An added benefit is that this repo exposes them under more useful names:

`nixpkgs-unstable-241030-070313-nixpkgs-2d2a9ddb` is more informative than `NixOS/nixpkgs/2d2a9ddb...` alone, which doesn't contain the channel, kind (nixpkgs-\* vs nixos-\*), or commit date. 
The naming scheme also ensures that the lexicographic ordering matches my own ordering preference.

##  Usage

Before:

```nix
inputs = {
    nixpkgs.url = "github:NixOS/nixpkgs/2d2a9ddb..."; # or
    nixpkgs.url = "github:NixOS/nixpkgs/nixpkgs-unstable";
};
```

After:

```nix
inputs = {
    nixpkgs.url = "github:dersuchmann/wrapper-flakes?dir=nixpkgs-unstable-241030-070313-nixpkgs-2d2a9ddb";
};
```

Or with helper function:

```nix
inputs = let wrap = dir: "github:dersuchmann/wrapper-flakes?dir=${dir}"; in
{
    nixpkgs.url = wrap "nixpkgs-unstable-241030-070313-nixpkgs-2d2a9ddb";
};
```
