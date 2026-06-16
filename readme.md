## Special setup instructions for Compute Canada first-timers

I wanted to make some instructions I can share to make the onboarding process simpler for other people who have use case requirements similar to my own. First off ComputeCanada can be accessed through ssh mainly. I recommend the nibi cluster.
I recommend using the [onDemand web interface](https://ondemand.sharcnet.ca/pun/sys/dashboard/) for Jupyter lab. This portal also allows for many things like file management, custom jobs, and login node terminal. Refer to the [wiki](https://docs.alliancecan.ca/wiki/Technical_documentation) for all other help.

running `diskusage_report` will show you your drive usage and your `project`, `scratch`, and `home` directories. It is recommended to create a personal folder in `project` and store your datasets, and code there. `Scratch` is volatile large storage and `home` is used for personal files like configs and binaries.

A couple binaries I recommend you are lazygit (for easier cli github management), gh (github tool for signing into git), and micro (a terminal editor alternative if your not familiar with VIM but want something better than Nano)

To install binaries, the best way I have found is to:

1. go to the github
2. go to releases
3. find a packaged release for AMD 64 since we're on x64 linux.
4. download usually as a .tar.gz or .zip using curl
   1. `curl -LO <release_url>` curl the release you found by right click copy url for that package.
   2. if tar.gz`tar -xzfv <file>.tar.gz` to uncompress .tar.gz
   3. if zip: `unzip <file>.zip` to unzip
   4. for other compressed formats look online.
5. go into that package and find the binary. This is sometimes located in `bin`.
6. copy that to your ~/.local/bin and now it should be on your _Path_ (meaning you can always call that command by typing it in the terminal)
7. Thats it. Now you can access your program from the terminal as you normally would.

Use `gh auth login` to login to git so you can clone repos and push code.

use UV for all your python environment requirements. More instructions for that below. **It is recommended to not use Conda on the clusters**

## UV setup

1. Install UV if not already installed.

```bash
# On macOS and Linux.
curl -LsSf https://astral.sh/uv/install.sh | sh
```

2. run `uv init` in project directory
   1. This will create a `pyproject.toml`, `.python-version`, `main.py`, and a git repository.
3. You can create a requirements.txt and list your projects python libarary requirements here, optionally include version numbers.
4. Before creating the venv, add these find-links to your `pyporject.toml` to computeCanada wheelhouse to automatically use their binaries on environment setup.
   ```toml
   # To get UV to search compute canada wheels, add this to the bottom of your pyproject.toml
   [tool.uv]
    # Use Compute Canada's wheelhouse first
    find-links = [
        "/cvmfs/soft.computecanada.ca/custom/python/wheelhouse/generic",
        # "/cvmfs/soft.computecanada.ca/custom/python/wheelhouse/gentoo/avx2",
        "/cvmfs/soft.computecanada.ca/custom/python/wheelhouse/gentoo/avx512",
    ]
   ```
5. run `uv add -r requirements.txt` to create .venv
6. optionally install the kernel to be used in jupyter lab

```bash
uv run ipython kernel install --user --name "Sample_UV_ENV_NAME" --display-name "This is where you put the name of your UV environment"
```

(optional) UV can install python deps universally as a tool. For example: `uv tool install huggingface_hub`

To run a script or command inside your uv environment you can use `uv run`
for example: `uv run test.py` or `uv run which python`
