## Shrimp

Bash / Zsh tool to help build a shim command that run original command
through container

:fried_shrimp: fresh from the :ocean:, packed in a :package:

![shrimp](https://user-images.githubusercontent.com/1087399/153924037-f45548f8-79fe-4977-840c-ac912be53008.png)

## Support container engine

- Docker
- Podman

## Usage
<!--FLAGS:START-->

    Build a shim command that run original command. If the original command is not available, try container version
    
    Usage:
      shrimp [options] <binary name>
    
    Option:
      --help	this message
      --recipe	show all available recipes
    
    Config:
      SHRIMP_HOME		$HOME/.shrimp
      SHRIMP_RECIPE		$HOME/.shrimp/recipe
      SHRIMP_BIN		$HOME/.shrimp/bin
      SHRIMP_TAG		shrimp:%s
      SHRIMP_ENGINE		
      SHRIMP_WORKDIR	/etc/data
      SHRIMP_BIND_CWD	true
      SHRIMP_STRICT		false
      SHRIMP_BUILD_ARGS	
      SHRIMP_RUN_ARGS	

<!--FLAGS:END-->

## Configurations

Shrimp has a very minimal configuration which you can set through the
environment variable

- `$SHRIMP_HOME`: Path for Shrimp to maintain its recipes and shims
- `$SHRIMP_RECIPE`: Path to Shrimp's recipe directory
- `$SHRIMP_BIN`: Path to directory for Shrimp to keep shim files. Shrimp will
automatically create the directory using `mkdir -p` if not exists
- `$SHRIMP_TAG`: Image tag format when building image using recipe, using
`printf` format with the first argument being a command name
- `$SHRIMP_ENGINE`: Container engine to be use (`docker` or `podman`)
- `$SHRIMP_WORKDIR`: Path for Shrimp to bind the working directory to
- `$SHRIMP_BIND_CWD`: Option whether to have Shrimp bind current working
directory to `$SHRIMP_WORKDIR`
- `$SHRIMP_STRICT`: Option whether to have Shrimp exit if container image is
not found (`false` will let Shrimp run shim as original command)
- `$SHRIMP_BUILD_ARGS`: Additional arguments to pass to container engine
during building process
- `$SHRIMP_RUN_ARGS`: Additional arguments to pass to container engined during
run

For default values, please see the configuration output in the Usage section
above

## Order of execution

Shrimp's shim will looking for the executable in the following order of
execution

- Original executable command
- Available build recipe in the `$SHRIMP_RECIPE`

## Build Recipe

Build recipe is a build file to pass to the container engine to build the
image for that particular command.

- For Docker, this would be a `Dockerfile`
- For Podman, this would be a `Containerfile`

The recipe should contain the entry point in which, upon running the
container, should run the given command without a need of passing the command
name.

Shrimp's recipe should be place in `$SHRIMP_HOME/<command>/<build file>`

For example, a recipe for `vim` in both Docker and Podman engine would have

`$SHRIMP_HOME/vim/Dockerfile` and `$SHRIMP_HOME/vim/Containerfile`

For more examples, check out `/examples` directory
