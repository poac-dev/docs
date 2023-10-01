## Creating a new project

Use this command when you start a new poac project.

```bash
$ poac create hello_world
     Created binary (application) `hello_world` package
```

> If you want to integrate your existing project with Poac, use the `init` command:
>
> ```bash
> your-pj/$ poac init
>      Created binary (application) `your-pj` package
> ```
>
> This command just creates a `poac.toml` file not to let your project break.

### Install dependencies

Like Cargo for Rust does, Poac installs dependencies at build time.
However, Poac does not support [weired specifiers](https://stackoverflow.com/q/22343224) for versions, such as `~` and `^`.
You can specify dependencies like:

`poac.toml`

```toml
[dependencies]
"boost/bind" = ">=1.64.0 and <2.0.0"
```

We regularly avoid auto updating packages to major versions which bring breaking changes, but minor and patch are acceptable.

> If you would use a specific version, you can write the version as following:
>
> ```toml
> [dependencies]
> "boost/bind" = "1.66.0"
> ```
After editing `poac.toml`, executing the `build` command will install the package and its dependencies.

```bash
hello_world/$ poac build
 Downloading packages ...
  Downloaded boost/bind v1.66.0
  Downloaded boost/core v1.66.0
  Downloaded boost/assert v1.66.0
  Downloaded boost/config v1.66.0
   Compiling 1/1: hello_world v0.1.0 (/Users/ken-matsui/hello_world)
    Finished debug target(s) in 0.70s
```

To use this dependency, update the `main.cpp` file.

`src/main.cpp`

```cpp
#include <iostream>
#include <boost/bind.hpp>
int f(int a, int b) {
  return a + b;
}
int main(int argc, char** argv) {
  std::cout << boost::bind(f, 5, _1)(10) << std::endl;
}
```

You can now run this source code:

```bash
hello_world/$ poac run
   Compiling 1/1: hello_world v0.1.0 (/Users/ken-matsui/hello_world)
    Finished debug target(s) in 0.50s
     Running `/Users/ken-matsui/hello_world/poac_output/debug/hello_world`
15
```

> We currently support building a project with header-only dependencies.
> Building with build-required dependencies will be soon supported.