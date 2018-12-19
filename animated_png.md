# How to Create an Animated PNG File in Python

## Setup

Install the `apng` package, either `pip install apng` or [from source](https://github.com/eight04/pyAPNG).
Note that it requires `pillow` to save files – without it, only reading is supported, which is kind of pointless.

## Usage (quick version)

``` python
from apng import APNG
import glob
image_files = glob.glob('*.png')
APNG.from_files(image_files, delay=100).save('anim.png')  # delay is in ms
```

## Usage (fine-grained version)

``` python
from apng import APNG, PNG
import glob

anim = APNG()
image_files = glob.glob('*.png')
delays = [100] * len(image_files)  # per-frame delay
delays[-1] = 1000  # 1 second pause at the end, for cycling
for file, delay in zip(image_files, delays):
    anim.append(PNG.open(file), delay=delay)

anim.save('anim.png')
```

## Resources

  - [Usage examples](https://github.com/eight04/pyAPNG#usage)
  - [API documentation](http://pyapng.readthedocs.io/en/latest/)
