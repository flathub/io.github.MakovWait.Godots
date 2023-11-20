# Flatpak for Godots

## Running host applications

This version of Godots is built with special permissions to be able to run commands on the host system outside of the sandbox via [flatpak-spawn](https://docs.flatpak.org/en/latest/flatpak-command-reference.html#flatpak-spawn). This is done by prefixing the command with `flatpak-spawn --host`. For example, if you want to run `gnome-terminal` on the host system outside of the sandbox, you can do so by running `flatpak-spawn --host gnome-terminal`.

### Using an external script editor

To spawn an external editor in Godot, all command line arguments must be split from the commands path in the [external editor preferences](https://docs.godotengine.org/en/latest/getting_started/editor/external_editor.html) and because the command needs to be prefixed with `"flatpak-spawn --host"`, the **Exec Path** is replaced by `flatpak-spawn` and the **Exec Flags** are prefixed by `--host [command path]`.

For example, for Visual Studio Code, where your [external editor preferences](https://docs.godotengine.org/en/3.2/getting_started/editor/external_editor.html) would *normally* look like this...

```text
Exec Path:  code
Exec Flags: --reuse-window {project} --goto {file}:{line}:{col}
```

...it should look like this **inside the Flatpak sandbox**:

```text
Exec Path:  flatpak-spawn
Exec Flags: --host code --reuse-window {project} --goto {file}:{line}:{col}
```

### Using Blender

Godot expects the Blender executable to be named `blender` (lowercase), so a script exactly named `blender` that executes Blender via `flatpak-spawn --host` should be created. Below are two [Bash](https://www.gnu.org/software/bash/) scripts which may need to be modified depending on your [shell](https://en.wikipedia.org/wiki/Shell_(computing)) and how Blender is installed.

#### Bash script assuming Blender is installed in `PATH` (e.g. using distribution packages)

```bash
#!/bin/bash

flatpak-spawn --host blender "$@"
```

#### Bash script assuming Blender is installed from Flathub

```bash
#!/bin/bash

flatpak-spawn --host flatpak run org.blender.Blender "$@"
```

Make sure your script is executable using `chmod +x blender`. Use the directory path containing your script in the Editor Settings (**Filesystem > Import > Blender > Blender 3 Path**).
