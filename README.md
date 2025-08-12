# Tuxedo Control Center for NixOS

<br>

## Overview

This repository provides a Nix derivation for the Tuxedo Control
Center until it is packaged in
[Nixpkgs](https://github.com/NixOS/nixpkgs) (see
[NixOS/nixpkgs#132206](https://github.com/NixOS/nixpkgs/issues/132206)).

[Tuxedo](https://www.tuxedocomputers.com/) is a German laptop
manufacturer that provides Linux-friendly laptops. Their system
control is done via an app called "Tuxedo Control Center" (TCC). This
open source app provides fan control settings among other
things. Without this app, the Tuxedo laptops default to very noisy fan
control settings. It lives on
[Github](https://github.com/tuxedocomputers/tuxedo-control-center).

<br>

## Usage

To enable Tuxedo Control Center, add the module from this repository
to your `/etc/nixos/configuration.nix`.

<br>

### Option 1: Stable Nix

```nix
{ config, pkgs, ... }:
let
  tuxedo = import (builtins.fetchTarball "https://github.com/Red-Flake/tuxedo-nixos/archive/master.tar.gz");
in {

 # ...

 imports = [
   tuxedo.outputs.nixosModules.default
 ];

 nixpkgs.overlays = [ tuxedo.outputs.overlays.default ];

 hardware.tuxedo-control-center.enable = true;
}
```

<br>

### Option 2: Nix Flake

This repository is a [Nix Flake](https://nixos.wiki/wiki/Flakes). As
such, it exports its module in a way that makes it somewhat convenient
to use in your Flakes-enabled NixOS configuration.

<br>

First enable the module in your `flake.nix`:

```nix
{
  inputs = {
	nixpkgs.url = "github:NixOS/nixpkgs/nixos-24.11";

	# ...

	tuxedo-nixos.url = "github:Red-Flake/tuxedo-nixos";
  };

  outputs = { self, nixpkgs, tuxedo-nixos }: {
	nixosConfigurations = {
	  your-system = nixpkgs.lib.nixosSystem {

	  # ...

	  modules = [
		./configuration.nix
		tuxedo-nixos.nixosModules.default

		# ...
	  ];

	  # ...

	  };
	};
  };
}
```

<br>

Then enable the module in `configuration.nix`:

```nix
  hardware.tuxedo-control-center.enable = true;
```

<br>

## Updating

To update to a new version, see the [updating
instructions](./pkgs/tuxedo-control-center/README.md).
