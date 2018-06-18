# How to create an animated PNG file in Python

Instructions for creating an animated PNG file from a collection of PNG files in Python.

## Date

2018-05-04

## Instructions

- Get the `apng` package, either `pip install apng` or [from source](https://github.com/eight04/pyAPNG). Note that it requires `pillow` to save -- without it, only reading is supported, which is kind of pointless.
- Quick version:

      from apng import APNG
      import glob
      image_files = glob.glob('*.png')
      APNG.from_files(image_files, delay=100).save('anim.png')  # delay is in ms

- More fine-grained control:

      from apng import APNG
      import glob

      anim = APNG()
      image_files = glob.glob('*.png')
      delays = [100] * len(image_files)
      delays[-1] = 1000  # 1 second pause at the end, for cycling
      for file, delay in zip(image_files, delays):
          anim.append(file, delay=delay)

      anim.save('anim.png')

## Further information

- [Usage examples](https://github.com/eight04/pyAPNG#usage)
- [API documentation](http://pyapng.readthedocs.io/en/latest/)
