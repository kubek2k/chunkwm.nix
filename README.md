# ChunkWM nix expression

Nix expression to install chunkWM on darwin nix environment. 
Expression was shamelessly stolen from @peel/dotfiles. All the kudos should go for him.

## Installation

To use it you need to add this to your darwin nix configuration 
(somewhere along the lines of `~/.nixpkgs/darwin-configuration.nix`):

```
let 
    overlays = self: super: rec {
        ...
        chunkwm = super.recurseIntoAttrs (super.callPackage (super.fetchFromGitHub {
          owner = "kubek2k";
          repo = "chunkwm.nix";
          sha256 = "11fwr29q18x4349wdg1pd7wqd1wvxsib6mjz7c93slf40h88vd53";
          rev = "0.1";
        }) {
          inherit (super) callPackage stdenv fetchFromGitHub imagemagick;
          inherit (super.darwin.apple_sdk.frameworks) Carbon Cocoa ApplicationServices;
        });
        ...
      };
in
   ...
   nixpkgs.overlays = [ overlays ];
   services.chunkwm.enable = true;
   services.chunkwm.package = chunkwm.core;
   services.chunkwm.plugins.dir = "/run/current-system/sw/bin/chunkwm-plugins/";
   ...
```
