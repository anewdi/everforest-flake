*Obviously this is not my colorscheme. All credit to [Sainhe](https://github.com/sainnhe/everforest). This is just a convenient way for nix users to access the colorscheme.*

A simple flake which allows you to easily use the everforest theme in you config. 
Chose type(light/dark) and variant(hard/medium/soft) with the the following options 
```
colorscheme.everforest.type = "dark" #default
colorscheme.everfores.variant = "medium" # default
```

## Colors
You may then access the colors with:
```
config.colorscheme.everforest.colors.yourcolor
```
You can read about the avaliable colors [here](https://github.com/sainnhe/everforest/blob/master/palette.md#highlights)

## Presets
To use something like foot, just do ``programs.foot.colors = config.colorscheme.everforest.foot`` once you have imported the flake module. 

The option is an attrset of colors so it can also be merged like this:
```
programs.alacritty.settings = {
    some.config = true;
} // config.colorscheme.everforest.alacritty;
```

Currently avaliable presets:
* zathura
* foot
* alacritty
* zellij
* wezterm

## Install
Example `flake.nix` file:
```
{
  inputs = {
    nixpkgs.url = "github:nixos/nixpkgs/nixos-unstable";
    home-manager = {
      url = "github:nix-community/home-manager";
      inputs.nixpkgs.follows = "nixpkgs";
    };

    # add everforest flake
    everforest.url = "github:anewdi/everforest";
  };

  outputs = { home-manager, nixpkgs, ... }@inputs:
  let
    system = "x86_64-linux";
  in
  {
    homeConfigurations."${username}" = home-manager.lib.homeManagerConfiguration {
      pkgs = import nixpkgs { inherit system; };

      # pass inputs as specialArgs
      extraSpecialArgs = { inherit inputs; };

      # import your home.nix
      modules = [ ./home-manager/home.nix ];
    };
  };
}
```
Example `home.nix` file:
```
{ inputs, pkgs, ... }:
{
  # add the home manager module
  imports = [ inputs.everforest.homeManagerModules.default ];

  colorscheme.everforest = {
      type = "light";
      variant = "medium";
  };

  programs.foot = {
    enable = true;
    colors = config.colorscheme.everforest.foot;
  };

  programs.alacritty = {
    enable = true;
    settings = {
      window = {
        decorations = "None";
      };
    } // config.colorscheme.everforest.alacritty;
  };
}
```

