# GROMACS-in-your-laptop
Tutorial about how to install and run easy simulations in your laptop with GROMACS

# Fist steps:

The very first thing we need is to get GROMACS in our laptop, for that we can nicely use the package managers in our favourite OS (Windows is not one of those unfortunately).

## Linux users:
As simple as using apt-get:
1. Update your packages:
```sudo apt-get update -y```
2. Retrieve and install GROMACS
```sudo apt-get install -y gromacs```

## MacOS users:
Even though is not as straight forward as LINUX, we can still use brew to get a hold on linux packages:
1. Install brew
```ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"```
2. Update brew (if you already have it installed)
```brew update```
3. Install GROMACS
```brew install gromacs```

## Note for advanced users:
This version of gromacs is already compiled and therefore will not be compiled for your specific CPU architecture, what does that means? In practical terms not much, thus you'll be able to run GROMACS without complications and with all the packages. In theoretical terms GROMACS will not be able to communicate effectively with yor CPU since the envs during standard compilation are not defined. If you want to have a formally compiled version of GROMACS, and allow GROMACS to play more effectively with your CPU and optimize it, just go to: https://manual.gromacs.org/documentation/ and get the last version.

For compiling GROMACS you can just follow the instructions here: https://manual.gromacs.org/documentation/2022/install-guide/index.html
Just as a reminder, after compiling you have to source GROMACS every time, to avoing doing it all the time you can add:

```source /usr/local/gromacs/bin/GMXRC```

To your bash/zsh file (.bashrc or .zshrc) so the shell will initialize GROMACS everytime you open a new terminal.

## Fix for Windows Users:
Any Windows user that wants to install Linux packages has a savior called WSL that allows to install a Linux terminal of your favorite distribution. To know more and install it:
https://docs.microsoft.com/en-us/windows/wsl/install
The instructions are pretty clear

Once installed just use the terminal provided by the distro and follow the same steps as **Linux users**

## Pick your visualization environment 

I personally would recommend to go for [VMD](https://www.ks.uiuc.edu/Development/Download/download.cgi?PackageName=VMD) since in this tutorial I will be using it: 

# First simulation in GROMACS
To get started let's move to the following [example](example/Template.md)

# Running GROMACS in a Jupyter notebook
I leave a [template](example/jupyter_gromacs.ipynb) with the very basics of GROMACS and visualization tools in Python