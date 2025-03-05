# Gutenberg

[![Build Status](https://github.com/openzim/gutenberg/workflows/CI/badge.svg?query=branch%3Amaster)](https://github.com/openzim/gutenberg/actions?query=branch%3Amaster)
[![codecov](https://codecov.io/gh/openzim/gutenberg/branch/master/graph/badge.svg)](https://codecov.io/gh/openzim/gutenberg)
[![Docker](https://img.shields.io/docker/v/openzim/gutenberg?label=docker&sort=semver)](https://hub.docker.com/r/openzim/gutenberg/)
[![PyPI version shields.io](https://img.shields.io/pypi/v/gutenberg2zim.svg)](https://pypi.org/project/gutenberg2zim/)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

Scraper to create ZIM files from [Project Gutenberg](https://www.gutenberg.org/).

The goal of this project is to create a tool to make available the contents of Project Gutenberg
as [ZIM files](http://www.openzim.org/).

## Usage

### Using Docker (Recommended)

Docker is the recommended way to use the Gutenberg scraper as it comes with all the required dependencies pre-installed.

1. **Pull the Docker image**:

```bash
docker pull ghcr.io/openzim/gutenberg:latest
```

2. **Run the scraper**:

```bash
docker run -it --rm -v $(pwd)/output:/data ghcr.io/openzim/gutenberg:latest gutenberg2zim -m /data
```

Important notes:
- The `-v $(pwd)/output:/data` option mounts the `output` folder in your current directory to the `/data` folder inside the container, allowing you to access the downloaded ebooks on your local machine.

3. **Show available options**:

```bash
docker run ghcr.io/openzim/gutenberg:latest gutenberg2zim --help
```


### Manual Installation

If you prefer not to use Docker, you can install the scraper directly:

#### Requirements

* Python 3.6+
* [Beautiful Soup 4](https://www.crummy.com/software/BeautifulSoup/)
* [lxml](http://lxml.de)
* [zimscraperlib](https://github.com/openzim/python-scraperlib)
* wget
* jpegoptim
* pngquant
* [gifsicle](https://www.lcdf.org/gifsicle/)
* [ImageMagick](https://www.imagemagick.org/)
* [Calibre](https://calibre-ebook.com/)

#### Installation

```bash
pip3 install gutenberg2zim
```

#### Running

```bash
gutenberg2zim --mirror=http://mirror.switch.ch/mirror/gutenberg/
```

### Common Options

* `--help` displays the help.
* `--zim-file=myfile.zim` sets the ZIM file name.
* `--language=en,fr` scrape English and French books (and any other comma-separated language)
* `--formats=html,pdf` download HTML and PDF file formats (and any other comma-separated format)
* `--books=all` download all books. You can specify particular books using book IDs: `--books=123,456,789`
* `--concurrency=3` run 3 parallel download processes
* `--dl-concurrency=3` run 3 parallel download processes for book files (HTML, PDF, etc.)
* `--low-memory` if you have limited memory (typically from a container/CI), scraper will use slower but less memory-intensive operations
* `--mirror=https://.../` specify a custom Project Gutenberg mirror URL
* `--no-index` do not create search index (not recommended)
* `--title=TITLE` sets the ZIM title
* `--optimization-cache=FOLDER` sets a folder to cache optimized files. Allowing for faster re-runs
* `--debug` to log at DEBUG level
* `--keep` to keep build folder and not delete it at the end
* `--output=FOLDER` folder to write the ZIM into

## Project Gutenberg Mirror

For best performance and to avoid unnecessary load on Project Gutenberg's servers, it's recommended to use a mirror:

```bash
gutenberg2zim --mirror=http://mirror.switch.ch/mirror/gutenberg/
```

Common mirrors are:
* http://mirror.switch.ch/mirror/gutenberg/
* http://www.gutenberg.org/dirs/
* http://aleph.gutenberg.org/
* http://www.gutenberg.lib.md.us/

## Docker Development Environment

For development purposes, you can build the Docker image locally:

```bash
docker build -t openzim/gutenberg .
```

Then run it with:

```bash
docker run -v /path/to/output:/output openzim/gutenberg gutenberg2zim --mirror=http://mirror.switch.ch/mirror/gutenberg/ --dl-concurrency=1
```

## License

[GPLv3](https://www.gnu.org/licenses/gpl-3.0) or later, see [LICENSE](LICENSE) for more details.