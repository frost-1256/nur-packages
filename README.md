# Ryan Yin's Nix User Repository

[![Build Status](https://github.com/ryan4yin/nur-packages/workflows/Build%20and%20populate%20cache/badge.svg)](https://github.com/ryan4yin/nur-packages)
[![Ryan Yin's Cachix Cache](https://img.shields.io/badge/cachix-ryan4yin-blue.svg)](https://ryan4yin.cachix.org)

**My personal [NUR](https://github.com/nix-community/NUR) repository**, use it at your own risk.

## How to use

```nix
# flake.nix
{
  inputs = {
    nixpkgs.url = "github:NixOS/nixpkgs/nixos-unstable";
    nur-ryan4yin = {
      url = "github:ryan4yin/nur-packages";
      inputs.nixpkgs.follows = "nixpkgs";
    };
  };

  outputs = { self, nixpkgs, nur-ryan4yin, ... }@inputs: {
    nixosConfigurations.default = nixpkgs.lib.nixosSystem {
      system = "x86_64-linux";
      modules = [
        # Add packages from this repo
        nur-ryan4yin.packages.${pkgs.system}.yazi  # terminal file manager

        # Binary cache (optional)
        ({ ... }: {
          nix.settings.substituters = [ "https://ryan4yin.cachix.org" ];
          nix.settings.trusted-public-keys = [ "ryan4yin.cachix.org-1:Gbk27ZU5AYpGS9i3ssoLlwdvMIh0NxG0w8it/cv9kbU=" ];
        })
      ];
    };
  };
}
```

## Notes for myself

1. Add your packages to the [pkgs](./pkgs) directory and to
   [default.nix](./default.nix)
   * Remember to mark the broken packages as `broken = true;` in the `meta`
     attribute, or travis (and consequently caching) will fail!
   * Library functions, modules and overlays go in the respective directories
2. [Add yourself to NUR](https://github.com/nix-community/NUR#how-to-add-your-own-repository) if you want to share your packages.

## LICENSE

[MIT](./LICENSE)
