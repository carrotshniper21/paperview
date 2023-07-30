# PAPERVIEW

Paperview is a high performance animated desktop background setter for Linux and X11.

![](screenshot.png)

Video of the above screenshot: https://www.youtube.com/watch?v=6ZTiA885bWM

## Build

    make # NOTE: SDL2 is required

## Single Monitor Use

    ./paperview FOLDER SPEED

A lower SPEED number will result in a faster frame rate. Only BMP files are supported.

## Multi Monitor Use

Paperview supports any number of monitors with its dynamic parameter list:

    ./paperview FOLDER SPEED X Y W H FOLDER SPEED X Y W H # ... And so on

The values X, Y, W (width), H (height) are integers and represent a rectangle with pixel
dimensions specifying where the wallpaper animation will be placed.
For instance, with a 1366x768 monitor on the left and a 1920x1080 monitor on the right,
the following command will animate the left monitor with a cat animation,
and the right, a river animation:

    ./paperview \
        ~/scenes/cat   5    0   0 1366  768 \
        ~/scenes/river 5 1366   0 1928 1080

## Running Background Daemon

Append an (&) to a paperview command to have it run as a background process. Eg:

    ./paperview FOLDER &

To stop this backgroud process, use `killall`:

    killall paperview

## Creating Custom Scenes

Creating a custom BMP scene folder from a GIF requires imagemagick.
For example, to create a castle scene folder from a castle.gif:

    mkdir cyberpunk
    mv cyberpunk.gif cyberbunk-bmp
    cd cyberpunk
    convert -coalesce cyberpunk.gif cyberpunk-bmp.bmp
    rm castle.gif

## Random Animated Wallpapers at Startup

Assuming a scenes folder containing a number of scene folders is present in the home folder,
run the following snippet as a background process within .xinitrc before running `startx`,
or simply execute it after X11 is running:

    while true
    do
        scene=$(ls -d ~/scenes/*/ | shuf -n 1)
        timeout 600 paperview $scene 5 # See Multi-Monitor Use above for multiple monitor support
    done

## Performance

Running on a Thinkpad X230 from 2012 at 1920x1080 and 60fps with an integrated Intel GPU:

    intel_gpu_time ./paperview cyberpunk-bmp

    user: 1.904135s, sys: 0.357277s, elapsed: 100.458648s, CPU: 2.3%, GPU: 11.7%
