![bobbleHAT](https://cdn.rawgit.com/perbu/bobblehat/master/gopher/bobblehat.svg)

A Go library for Raspberry Pi HATs (Hardware Attached on Top), starting with the [Sense HAT](https://www.raspberrypi.org/products/sense-hat/).

[![GoDoc](https://godoc.org/github.com/perbu/bobblehat?status.svg)](https://godoc.org/github.com/perbu/bobblehat) [![Build Status](https://travis-ci.org/perbu/bobblehat.svg?branch=master)](https://travis-ci.org/perbu/bobblehat)

### Documentation

#### Screen

<img src="https://cdn.rawgit.com/perbu/bobblehat/master/gopher/screen.svg" width="200">

The Sense HAT has an 8x8 LED matrix that could be used to display the status of a headless server, to write a mini-game, or countless other possibilities.

You can create an 8x8 frame buffer, set pixels with (x,y) coordinates, and then draw the frame buffer to the screen.

```go
fb := screen.NewFrameBuffer()
fb.SetPixel(0, 0, color.Red)
screen.Draw(fb)
```

Colors are specified as red, green, blue (RGB) components with a range of 0-255. However, these are converted down to 32 shades (0-31) before being sent to the screen.

```go
cyan := color.New(0, 255, 255)
```

A frame buffer is an 8x8 texture that can be drawn to the screen, but you can also create textures of any size (width, height).

```go
tx := texture.New(16, 16)
tx.SetPixel(8, 8, color.White)
```

Or load a PNG file into a new texture.

```go
tx, err := texture.Load("image.png")
```

The `blit` function will copy between textures with destination and source offsets (x, y) and dimensions (width, height). See the image scrolling example for one use, but this can always be used to draw multi-pixels sprites (opaque).

```go
texture.Blit(fb.Texture, 0, 0, tx, 0, 0, 8, 8)
```

#### Stick

<img src="https://cdn.rawgit.com/perbu/bobblehat/master/gopher/stick.svg" width="200">

The Sense HAT has a tiny joystick control.

```go
input, err := stick.Open("/dev/input/event0")
if err != nil {
	log.Fatal(err)
}

for {
	select {
	case e := <-input.Events:
		switch e.Code {
		case stick.Enter:
			fmt.Println("⏎")
		case stick.Up:
			fmt.Println("↑")
		case stick.Down:
			fmt.Println("↓")
		case stick.Left:
			fmt.Println("←")
		case stick.Right:
			fmt.Println("→")
		}
	}
}
```

#### Motion

<img src="https://cdn.rawgit.com/perbu/bobblehat/master/gopher/motion.svg" width="200">

Gyroscope, Accelerometer, Magnetometer.

Not yet implemented.

### Weather

<img src="https://cdn.rawgit.com/perbu/bobblehat/master/gopher/weather.svg" width="200">

Temperature, Humidity, Barometric pressure

Not yet implemented

### Gopher Gala 2016

#### Team

* [Carlisia Campos](https://github.com/carlisia), Developer, California, U.S.
* [Nathan Youngman](https://github.com/nathany), Developer, Alberta, Canada
* [Olga Shalakhina](https://github.com/osshalakhina), Illustrator, Ukraine
* [Ravi Sastryk](https://github.com/ravisastryk), Developer, California, U.S.

#### Media

bobbleHAT in action:

* [First light](https://www.instagram.com/p/BA5LhnHBkx0/)
* [Scrolling gopher favicon](https://www.instagram.com/p/BA7rzTmhk_p/)
* Follow [bobbleHAT](https://twitter.com/gobobblehat) on Twitter for more videos.

Team:

* [MacGyvering standoffs to use the joystick](https://twitter.com/carlisia/status/691115626891350016)
* [Ravi working on SWIG for RTIMULib](https://twitter.com/carlisia/status/691064926509465601/photo/1)

### License

The code is licensed under the Simplified BSD License.
Copyright © 2016 bobbleHAT Authors.

The original Go gopher © 2009 Renée French. Used under Creative Commons Attribution 3.0 license.

Illustrations © 2016 Olga Shalakhina.

### Related Projects

* [Gobot](http://gobot.io/)
* [EMBD](http://embd.kidoman.io/)
* [sense-hat](https://github.com/RPi-Distro/python-sense-hat) (Python)
* [RTIMULib](https://github.com/RPi-Distro/RTIMULib) (C++ dependency of sense-hat)
* [golang-evdev](https://github.com/gvalkov/golang-evdev) (Go bindings for the Linux input subsystem)
* [evdev](https://github.com/jteeuwen/evdev) (Go implementation of the Linux evdev API)
