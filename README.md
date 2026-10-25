# CompRobo ROS2 Pixi/Robostack Template

A project template that sets up a ROS2 Jazzy workspace with Neato support using [Pixi](https://pixi.prefix.dev/) and [Robostack](https://robostack.github.io/) for isolation and dependency management. Intended to make computational setup simpler, support most Linux versions, and improve reproducibility. 

It's intended that you create a new workspace with this template for each project you make.

## Supported Platforms

Any Linux version _should_ work. Tested on Ubuntu 24.04 and Mint 22.1.

MacOS and Windows should be supportable with relatively minor changes to this template, though more work may be needed on `neato_packages`.

## Prerequisites

You need [Pixi](https://pixi.prefix.dev/) installed before using this template.

To install it:

1. **IMPORTANT**: Remove any `source /opt/ros/<distro>/setup.bash` or `source ~/ros2_ws/install/setup.bash` lines from your `~/.bashrc`. These conflict with Pixi. Instead, move them into `~/.bash_aliases` as an alias so you can still use your global ROS install when needed:
   ```bash
   alias ros2-setup='source /opt/ros/jazzy/setup.bash && source ~/ros2_ws/install/setup.bash'
   ```
2. Install Pixi:
   ```bash
   curl -fsSL https://pixi.sh/install.sh | bash
   ```
3. Add shell completion to `~/.bashrc`:
   ```bash
   eval "$(pixi completion --shell bash)"
   ```
4. Restart your terminal.

## Creating a new workspace

```bash
pixi exec copier copy gh:rivques/comprobo_ros_pixi_template <your_workspace_name>
```

Copier will ask for your package name, initial node name, description, author, and license. It then:

1. Initializes a git repo
2. Runs `ros2 pkg create` (via the pixi-managed CLI) to scaffold your package inside `src/`
3. Adds `neato_packages` as a git submodule
4. Provides `pixi run <task>` commands for common tasks like building, running nodes, and launching Gazebo worlds.

After Copier finishes:

```bash
cd <your_workspace_name>
pixi run build
pixi run <your_package_name>
```


## Benefits and drawbacks of this approach
### Benefits
- **Support for any Linux version**: No longer is Ubuntu 24.04 specifically required. This should work on any Linux version that supports Pixi. Also, updating to newer ROS versions should be more painless now.
- **Improved dependency management**: Because each workspace is entirely self-contained, different projects can use different versions of packages without conflict. This should mean no more Numpy 1.x errors, no more `--break-system-packages`, and no more sacrificing the global Python install to ROS. This benefit should become greater later into the year as projects use increasingly diverse dependencies.
- **Easier setup**: The installation process should be much simpler than the existing process. Steps 3-4 of the computational setup should become ~5 lines of code total (though this needs testing, especially to make sure that ROS_DOMAIN_ID and hping3 are set up correctly).
- **You can run a build from anywhere**: ok this is a small one but running `colcon build` from `src` instead of `ros2_ws` has bit me enough times that i'm calling it out

### Drawbacks
- **New process**: People aren't used to this workflow, so we won't know the common errors, quirks, etc.
- **Another layer of abstraction**: Pixi is on top of Colcon, which is on top of Robostack/Conda, which is on top of ROS2, CMake, etc. This means there's more opportunities for things to go wrong and more places the issue could be.
- **Incomplete package support**: While Robostack has [a lot](https://robostack.github.io/jazzy.html) of packages, especially the common ones, some packages aren't yet distributed. It's supposed to be fairly easy to request a new package, and building from source is possible, but could be a pain.
- **Worse autocomplete**: ROS2 tab-complete doesn't work when using `pixi run ros2 <command>`. This is probably fixable, but I haven't figured it out yet.

## Default documentation
For documentation on the project this generates, see [its README](/template/README.md.jinja).

## AI Usage
I researched Pixi and created a working demo project on my own. Claude turned my notes and demo project into this template.# comprobo_ros_pixi_template
