# ![xtensor-io](http://quantstack.net/assets/images/xtensor-io.svg)

[![Travis](https://travis-ci.org/QuantStack/xtensor-io.svg?branch=master)](https://travis-ci.org/QuantStack/xtensor-io)
[![ReadTheDocs](https://readthedocs.org/projects/xtensor-io/badge/?version=stable)](http://xtensor-io.readthedocs.io/en/stable/)
[![Binder](https://img.shields.io/badge/launch-binder-brightgreen.svg)](https://beta.mybinder.org/v2/gh/QuantStack/xtensor-io/0.2.0-binder?filepath=notebooks/xtensor-io.ipynb)
[![Join the Gitter Chat](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/QuantStack/Lobby?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Reading and writing image, sound and npz file formats to and from xtensor data structures.

## Introduction

**xtensor-io is an early developer preview, and is not suitable for general usage yet. Features and implementation are subject to change.**

`xtensor-io` offers API of to read and write various file formats into `xtensor` data structures:

 - images,
 - audio files,
 - NumPy's compressed storage format (NPZ).

`xtensor-io` wraps the [OpenImageIO](https://github.com/OpenImageIO/oiio), [libsndfile](https://github.com/erikd/libsndfile) and [zlib](https://github.com/madler/zlib) libraries.

## Installation

`xtensor-io` is a header-only library. We provide a package for the conda package manager.

```bash
conda install xtensor-io -c QuantStack
```

- `xtensor-io` depends on `xtensor` `^0.13.2`.

- `OpenImageIO`, `libsndfile` and `zlib` are optional dependencies to `xtensor-io`

  - `OpenImageIO` is required to read and write image files.
  - `libsndfile` is required to read and write sound files.
  - `zlib` is required to load NPZ files.

All three libraries are available for the conda package manager.

You can also install `xtensor-io` from source:

```
mkdir build
cd build
cmake ..
make
sudo make install
```

## Usage

Loading a png image into xarray with shape `WIDTH x HEIGHT x CHANNELS`

```cpp
auto arr = xt::load_image("test.png");
```

Writing a JPEG image from an xarray

```cpp
xt::dump_image("dumptest.jpg", arr + 5);
```

Loading an `npz` file containing multiple arrays

```cpp
auto npy_map = xt::load_npz("test.npz");

auto arr_0 = npy_map["arr_0"].cast<double>();
auto arr_1 = npy_map["arr_1"].cast<unsigned long>();
```

Opening an audio file.

```cpp
auto audio = xt::load_audio("files/xtensor.wav");
auto& arr = std::get<1>(audio);  // audio contents (like scipy.io.wavfile results)
```

Writing an autio file from an xtensor expression

```cpp
int freq = 2000;
int sampling_freq = 44100;
double duration = 1.0;

auto t = xt::arange(0.0, duration, 1.0 / sampling_freq);
auto y = xt::sin(2.0 * numeric_constants<double>::PI * freq * t);

xt::dump_audio("files/sine.wav", y, sampling_freq);
```

## License

We use a shared copyright model that enables all contributors to maintain the
copyright on their contributions.

This software is licensed under the BSD-3-Clause license. See the [LICENSE](LICENSE) file for details.
