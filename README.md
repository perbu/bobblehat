![bobbleHAT](https://cdn.rawgit.com/perbu/bobblehat/master/gopher/bobblehat.svg)

A Go library for Raspberry Pi HATs (Hardware Attached on Top), starting with the [Sense HAT](https://www.raspberrypi.org/products/sense-hat/).

[![GoDoc](https://godoc.org/github.com/perbu/bobblehat?status.svg)](https://godoc.org/github.com/perbu/bobblehat) [![Build Status](https://travis-ci.org/perbu/bobblehat.svg?branch=master)](https://travis-ci.org/perbu/bobblehat)

### Documentation

I've used this project in a couple of projects now. Since it was abandoned, I've decided to take it over and maintain it for a while. Mostly, I've
done this:
 - added module
 - got rid of init(), now you call a New function that can return an error

#### Screen

<img src="https://cdn.rawgit.com/perbu/bobblehat/master/gopher/screen.svg" width="200">

The Sense HAT has an 8x8 LED matrix that could be used to display the status of a headless server, to write a mini-game, or countless other possibilities.

You can create an 8x8 frame buffer, set pixels with (x,y) coordinates, and then draw the frame buffer to the screen.

```go
dev, err := screen.New()
if err != nil {
    log.Fatal(err)
}
fb := dev.NewFrameBuffer()
fb.SetPixel(0, 0, color.Red)
dev.Draw(fb)
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

