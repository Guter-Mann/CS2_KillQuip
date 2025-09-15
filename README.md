
<h1 align="center">KillQuip</h1>

---

## Documentation

- [English Documentation](./docs/README.en.md)
- [Русская документация](./docs/README.ru.md)

## Overview

This project allows you to add custom phrases in the CS2 chat, which will appear after
each precise shot, adding some fun to the gameplay.

### **WARNING!**

This script is not recommended for use on official servers or servers with VAC enabled.
The developer is not responsible for account bans or any other consequences. Use at
your own risk.

## Table of Contents

- [Technologies](#technologies)
- [Demo](#demo)
- [How It Works?](#how-it-works)
- [Installation](#installation)
  - [Compilation](#compilation)
  - [Package Installation](#package-installation)
- [Configuration](#configuration)
- [Usage](#usage)

## Technologies

- [OpenCV](https://opencv.org)
- [NumPy](https://numpy.org)
- [Nuitka](https://nuitka.net)

## Demo

![GIF1](https://github.com/Guter-Mann//Xlams/blob/main/GIFs/KillQuip/1.gif?raw=true)
![GIF2](https://github.com/Guter-Mann//Xlams/blob/main/GIFs/KillQuip/2.gif?raw=true)

## How It Works?

I searched extensively for something like an API for the Source2 engine.
In the end, I concluded that some “workarounds” were necessary since there is no command
that provides statistics about kills in a match.

Therefore, I used the OpenCV2 library to detect kill information in the top-right corner
of the screen.

![GIF3]()

The script monitors a highlighted area for messages in a red frame, which appear when a
player makes a kill (or assists in a kill).

If the condition is met, a configuration file quip.cfg is created in the root game
directory with the following command:

```commandline
say {text}
```

Where `{text}` is replaced with a random phrase from the pre-recorded list.

After that, the script automatically presses the key, loads the configuration file
`quip.cfg`, and sends the message to the general chat. For proper operation, the script
waits 8.5 seconds.

## Installation

### 1. Clone the repository

```commandline
git clone https://github.com/Guter-Mann/KillQuip.git
```

### 2. Create virtual environment

```commandline
python -m venv venv
```

### 3. Activate virtual environment

```commandline
.\venv\Scripts\activate
```

### 4. Install dependencies

```commandline
pip install -r .\requirements.txt
```

### Compilation

This project uses [Nuitka](https://nuitka.net), which converts Python code to C++ and
then compiles it. I use the [MinGW64 v12.3](https://objects.githubusercontent.com/github-production-release-asset-2e65be/220996547/86825ef3-e192-47cb-a35b-6534c686ac07?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=releaseassetproduction%2F20240803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240803T124102Z&X-Amz-Expires=300&X-Amz-Signature=27bcd64354dac92c70216813768d49896ab4dd45b5a1daa4c3e694120fcdae69&X-Amz-SignedHeaders=host&actor_id=77664190&key_id=0&repo_id=220996547&response-content-disposition=attachment%3B%20filename%3Dwinlibs-x86_64-posix-seh-gcc-12.3.0-llvm-16.0.4-mingw-w64ucrt-11.0.0-r1.7z&response-content-type=application%2Foctet-stream)
compiler, which can be downloaded from the official website.

Before compilation, create a temporary directory (e.g., `temp`) and copy the `src`
folder, as well as `errors.log` and `settings.cfg` files into it.

Then run the following command to compile:

```commandline
python -m nuitka --standalone --mingw64 --include-data-file=./errors.log=errors.log --include-data-file=./settings.cfg=settings.cfg KillQuip.py
```

#### Possible Compilation Error

If you encounter a `UnicodeDecodeError`, it may be because one of the project files has
a non-standard encoding or a special filename, such as `.gitignore` or `.ide`. In that
case, delete the files created by Nuitka and retry the compilation.

**Example error:**

```commandline
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xcf in position 1034796: invalid continuation byte
Nuitka-Reports: Compilation crash report written to file 'nuitka-crash-report.xml'. Please include
Nuitka-Reports: it in your bug report.
```

### Package Installation

Install the package:

```commandline
python setup.py install
```

After installation, you can run the program with:

```commandline
kill-quip
```

or

```commandline
kq
```

On first run, a `FileNotFoundError` will occur, generating a configuration file with
default settings.

## Configuration

Before use, configure the script using the following commands:

### - Выделить область в котором выводятся сообщения

```shell
python.exe .\KillQuip.py -sa
```

This script allows you to mark the message tracking area. I use the Snip & Sketch tool (installed by default) to create screenshots of the area.

![GIF4](https://github.com/Guter-Mann//Xlams/blob/main/GIFs/KillQuip/4.gif?raw=true)

### - Set the root game directory path

```shell
python.exe .\KillQuip.py -p
```

Enter the directory path manually or copy it:

```shell
python.exe .\KillQuip.py -p "{path}"
```

Where `{path}` is replaced with the directory path. It’s recommended to use quotes
because spaces or special characters may be misinterpreted.

### - Add phrases

```shell
python.exe .\KillQuip.py -m
```

This command starts a script that sequentially records the entered phrases.
To finish, simply press Enter on an empty line.

### - Configure the game

Open the developer console in CS2 and enter:

```commandline
bind "p" "exec quip"
```

This command allows the script to automatically press the "p" key and send the prepared phrase to chat.

## Usage

Before or during a match, run the script:

```shell
python.exe .\KillQuip.py
```

Then start playing, and the script will automatically send funny phrases to the chat after each kill.
