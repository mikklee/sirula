# Sirula

Sirula (simple rust launcher) is an app launcher for wayland.
Currently, the only feature is launching apps from `.desktop` files.
Feel free to submit pull requests for any feature you like.

I wrote sirula partially to learn rust, so do not expect perfect rust code.
I'd be happy to hear any criticism of my code.

## Examples

`sample-config/a`:

![](sample-config/a/sirula.gif)
[open](https://raw.githubusercontent.com/DorianRudolph/sirula/master/sample-config/sirula.gif)

`sample-config/b`: Overlay in the center of the screen.

![](sample-config/b/sirula.png)

## Building

- Dependency: [gtk-layer-shell](https://github.com/wmww/gtk-layer-shell)
- Build: `cargo build --release`
  - Optionally, `strip` the binary to reduce size
- Alternatively, install with `cargo install --path .`
- There is also an unofficial [AUR package](https://aur.archlinux.org/packages/sirula-git/)

## Nix

I made this fork so that I could get Sirula to work on NixOs. The following override should work:

```nix
{ config, pkgs, ... }:

{
  nixpkgs.config = {
    packageOverrides = pkgs: {
      sirula = pkgs.sirula.overrideAttrs (old: rec {
        name = "sirula-${version}";
        version = "b15efe85ef1fe50849a33e5919d53d05f4f66090";
        src = pkgs.fetchFromGitHub {
          owner = "Mikklee";
          repo = "sirula";
          rev = "b92856855c7e81f700b64857127ee5f3aba65a49";
          sha256 = "sha256-LiP4lf5KX3QYIUxo2FLpiv8y+LutTqZ5hnm2fPTha9U=";
        };
        cargoSha256 = "";
        cargoHash = "";
        cargoDeps = old.cargoDeps.overrideAttrs (pkgs.lib.const {
          name = "${name}-vendor.tar.gz";
          inherit src;
          outputHash = "sha256-npwgE2yDBwmGuQkJGuYed1scNzmm55ASFsZCQwzcqOg=";
        });
      });
    };
  };
}
```

## Configuration

Use `config.toml` and `style.css` in your `.config/sirula` directory.
See `sample-config` for documentation.
