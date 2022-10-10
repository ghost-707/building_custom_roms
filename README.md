# BASICS

If you already have these:

* A linux Environment (Recommended : Ubuntu Latest LTS (Long Term Support)) Remember you can build on any distro having proper repository like arch based or ubuntu based must have (Ubuntu repo)  .

* Internet Connection obviously (Better if it's a 5Mb/S plan at the least). Because ROM sources could be 25-30 GB approx. even when shallow cloned.

* CPU: 4 core 8 thread is the bare minimum or else it takes an eternity to build.

* BTW high config vps can be preferd.

* RAM: 16 GB Minimum ( SWAP optional and it is better to have 32 Gigs of RAM afterall) to compile Android 12 and onwards.

* Storage: SSD is better and it will build if you have HDD too.. 256GB is a decent amount of space

* Some Linux knowledge and Basic ideas about Git.

* Some Basic commands like clone, --force-sync, -j(), repo init, repo sync and some other.

* You need to have your device trees:

1:device

2:device-common

3:kernel

4:vendor

5:vendor-common

6:hardware

You can git clone device tree or make one from Template (https://github.com/HeRulez/android_device_tree_template) 

* A GitHub Account or a GitLab account! (Preferably with the trees forked/imported)

# STEPS

# Step 1 Setting up your Building Env.


Check for the update and upgrade

     (UBUNTU)
sudo apt-get update

sudo apt-get upgrade

     (ARCH)
sudo pacman -Syu

```bash
# get superuser access.
sudo su

     (UBUNTU)
# Adding & installing JDK
add-apt-repository ppa:openjdk-r/ppa

# update all packages.
apt-get update

# install Java packages.
apt-get install git-core gnupg curl repo fuse flex bison build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z1-dev libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig

# become a normal user.
exit

NOTE: ARCH BASED DEVS CAN SKIP THIS PART AND FOLLOW: (https://wiki.archlinux.org/title/android)

# creating a bin folder and setting up Script.

mkdir ~/bin

PATH=~/bin:$PATH

cd ~/bin

curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo

chmod a+x ~/bin/repo

git clone https://github.com/ghost-707/scripts

cd scripts

cd setup

./android_build_env.sh (UBUNTU)
./arch-manjaro.sh (ARCH)

# Step 2 Downloading a ROM Source


* This is important to do before cloning the device specific trees

But before that we need to configure our git as shown below
Open the command line.
Set your username:
git config --global user.name "FIRST_NAME LAST_NAME"

Set your email address:
git config --global user.email "MY_NAME@example.com"


Now find the GitHub org of the ROM you need to build and sync it like shown below

# we need to make a folder for the ROM first (Example LineageOS)
mkdir Lineage

# Connect your Github account.
git config --global user.name "FIRST_NAME LAST_NAME" git config --global user.email "YOUR EMAIL"

# Go to ROM folder.
cd Lineage

# Now we need to initialize LineageOS Source
# For that, go the LineageOS git and find the manifest (https://github.com/LineageOS/android this may vary from ROM to ROM)
# Choose the Branch (this is actually the version of android)
# Let's choose Android 12.1, so as an example the branch here would be "lineage-19.1"

NOTE: (-b) is branch

# So it goes like:
repo init -u git://github.com/LineageOS/android.git -b lineage-19.1

# As an alternative you can do a shallow clone to reduce size of the source..
# A Shallow Clone only clones the source with the last commit history specified

repo init --depth=1 -u git://github.com/LineageOS/android.git -b lineage-19.1


# And to Sync it
repo sync

This would actually depend on your internet speed.. the size of the LineageOS repo would be around 80-100 GB or more, probably

# Step 3 Device sources aka Trees


So, you probably downloaded the rom sources and set your git id. Well the next step is to find your device sources (Device Tree , Kernel Tree , Vendor Tree {And A Common Tree if available for both vendor and device})

* The prerequisites are:

- to know your device code name
- to know what platform it is (would make it easier to find the common trees)
- to know if the trees are readily avialable (for most oneplus/Xiaomi/etc.. devices its a true case scenario)
- to clone them to your github account so that u can do a bringup of the tree for the ROM

Here is an example device ( OnePlus 7T )

> Code Name: hotdogb, SoC: Snapdragon 855+, Platform: SM8150

Here, the common tree for hotdogb is sm8150-common (this exists for thr vendor too).
Also, the trees are avialable in the following:

device_oneplus_hotdogb
device_oneplus_sm8150-common
hardware_oneplus
kernel_oneplus_sm8150
vendor_oneplus_hotdogb
vendor_oneplus_sm8150-common

Note: 1: You can get vendor and Vendor common tree on The Muppets (https://github.com/TheMuppets)
      2: Search according to your device company aka manufacturer, device and device platform (common sense)
      3: Please note if you syned source with android 10,11,12,12.1,13 branch please import device, hardware, vendor and kernel according to android version or branch.

# Step 4 Placing right device trees folders to right souce folder

# Create folder in the following directory.

device_oneplus_hotdogb ----------------------------------------->(source/device/oneplus/hotdogb)
device_oneplus_sm8150-common ----------------------------------->(source/device/oneplus/sm8150-common)
hardware_oneplus------------------------------------------------>(source/hardware/oneplus)
kernel_oneplus_sm8150------------------------------------------->(source/kernel/oneplus/sm8150)
vendor_oneplus_hotdogb------------------------------------------>(source/vendor/oneplus/hotdogb)
vendor_oneplus_sm8150-common------------------------------------>(source/vendor/oneplus/sm8150-common)

      Note: Device, Vendor, Hardware and Kernel folder already available in Source directory you have to create folder or mkdir (oneplus and common) in the folders



# Step 5 Compile

After doing with this 4 steps

#you have to create builing environment:

. build/envsetup.sh

#lunch your device

lunch lineage_hotdogb-userdebug or lunch lineage_hotdogb-eng

#now start compiling

mka bacon

finger cross :D

IMPORTANT NOTE: SOME COMMON ERRORS ARE RELATED TO COMMONBROADCONFIG IN DEVICE-COMMON FOLDER AND ANDROID.BP FILES YOU NEED TO MODIFY IT ACCORDING TO THE ERRORS.....

step 6 upload and share

COMMON SENSE xD

# ********************* AS AN RESPONSIBLE INDIVIDUAL FIRST TEST THE BUILD YOURSELF IF IT BOOTS THEN SHARE, DONT! BUILD AND START SHARING WITH OTHERS IMMEDIATELY POSSIBLE CHANCES BRICKING OTHERS DEVICE.******************************

*STAY POSITIVE
*START BUILING


                                                              BIG LOVE FROM MY SIDE <3
                                                                       &
                                                      WELCOME TO ANDROID BUILDING COMMUNITY :'3          

# AS A NEWBIE FIRST START BUILDING LINEAGEOS AND THEN SWITCH TO OTHER CUSTOM ROMS THROUGH THIS METHOD YOU KNOW THE POSSBLE OUTCOME ERRORS AND WORKAROUND.

#HAPPY_BUILDING

CREDIT: 
AKHIL NARANG 
(https://github.com/akhilnarang)
IMASARU
(https://github.com/imasaru)
ATLAN PRIME
(https://github.com/AtlanPrime)





