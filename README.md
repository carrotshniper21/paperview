# PAPERVIEW

Paperview is a high performance animated desktop background setter for Linux and X11.

![](screenshot.png)

Video of the above screenshot: https://www.youtube.com/watch?v=6ZTiA885bWM

## Build

    make

## Single Monitor Use

    ./paperview FOLDER

Only BMP files are supported.

## Multi Monitor Use

Paperview supports any number of monitors with its dynamic parameter list:

    ./paperview FOLDER

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
