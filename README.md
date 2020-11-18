Based on:https://www.youtube.com/watch?v=yCSW-LtYlJc&t=178s&ab_channel=GeneralWhine

# Start
    sudo apt-get update
    sudo apt-get upgrade
# Kernal Building
    sudo apt install git bc bison flex libssl-dev make
    
    // This will clone the kernel
    git clone --depth=1 https://github.com/raspberrypi/linux
    
    // This will configure the kernel
    cd linux
    KERNEL=kernel7l
    make bcm2711_defconfig
    
    // This will configure the driver
    sed -ri "/CONFIG_SND_USB_POD/ { s/# (\w+).*/\1=m/; i CONFIG_SND_USB_LINE6=m }" .config
    
    // Build Kernel (takes a long time)
    make -j4 zImage modules dtbs
    sudo make modules_install
    sudo cp arch/arm/boot/dts/*.dtb /boot/
    sudo cp arch/arm/boot/dts/overlays/*.dtb* /boot/overlays/
    sudo cp arch/arm/boot/dts/overlays/README /boot/overlays/
    sudo cp arch/arm/boot/zImage /boot/$KERNEL.img

# Wrap Up
    sudo reboot
    
Plug in your pod, turn it on, and double check that everything works!
