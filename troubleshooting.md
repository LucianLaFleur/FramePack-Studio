## troubleshooting: mentioned URL http://0.0.0.0:7860 inaccessable 
tl;dr: it's a known typo, you must use http://127.0.0.1:7860 (local loopback)

Explaination:
On running `python studio.py` the command line will say: "Running on local URL: http://0.0.0.0:7860 " but this is an inaccurate label.
It's actually running on 127.0.0.1:7860 (the local loopback address, since it's being hosted locally, not on the web).
Use http://127.0.0.1:7860 in a browser's address bar and it should take you to a display of Frame Pack Studio.

## xformers not installed / Installing xformers
If the CLI says something like `Error:xformers is not installed`, this could be a reason why FramePack Studio won't launch the Studio.py program<br /> 
This is a guide based off a user's experience on a PC.<br /> 
<br /> 
The following commands walks through a tester's experience installing FramePack Studio, and what they needed to do to successfully set up dependencies. <br /> 
Multiple error messages about dependencies will display after the various commands, but this guide will keep you on track to what actually got a working install up and running.<br /> 
<br /> 
First, you need to pip-install this wheel for some basic setup<br /> 
`pip install wheel ninja cmake setuptools`<br /> 
<br /> 
after that's downloaded, clone the xformers repo hosted on github with this command<br /> 
`git clone https://github.com/facebookresearch/xformers.git`<br /> 
<br /> 
In CLI, go into that xformers directory just made<br /> 
`cd xformers`<br /> 
<br /> 
Make a checkout of a stable version<br /> 
`git checkout v0.0.25.post1`<br /> 
<br /> 
Install xformers dependencies <br /> 
`pip install -r requirements.txt`<br /> 
<br /> 
We need to manually install a sub-module, called cutlass<br /> 
`git submodule update --init third_party/cutlass`<br /> 
<br /> 
Then we need to make a dummy folder to avoid crashing due to file pathing issues in xformers’ setup.py, since the script is still looking for the flash attention folder<br /> 
Make sure you're in your xformers directory, then make the dummy dir as follows:<br /> 
`mkdir third_party\flash-attention\flash_attn`<br /> 
<br /> 
Set an env variable to disable flash attention (causes complexity currently, needs further investivation to integrate in the future)<br /> 
`set XFORMERS_FORCE_DISABLE_FLASH_ATTENTION=1`<br /> 
<br /> 
Then, from within the xformers directory, we can install it locally<br /> 
`python setup.py install`<br /> 
If this doesn't work, see the section below, since you might be missing the necessary built tools.<br /> 
<br /> 
## Installing Microsoft C++ Build Tools
(This related to PC, documentations till needed for other platforms, if such an error is encountered when installing xformers)<br /> 
! [NOTE] : if you get an error like:<br /> 
"RuntimeError: Error compiling objects for extension"<br /> 
That means you need the "Microsoft C++ Build Tools" data installed.<br /> 
offical link: "https://visualstudio.microsoft.com/visual-cpp-build-tools/"<br /> 
<br /> 
important modules to add when installing Microsoft  C++ build tools:<br /> 
- C++ CMake tools for Windows<br />
  ![additionalHelp1](https://github.com/user-attachments/assets/40c82377-f2f3-4439-a523-02e487e4b2dd)
<br /> 
- Windows 10 SDK (10.0.19041.0) <br /> 
^^ any windows 10 should be okay, but this is documented as the tester's version<br />
![additionalHelp1](https://github.com/user-attachments/assets/e722caff-ca46-4a43-b1ff-b51184116e62)
<br /> 
- MSVC v143 - VS 2022 C__ x64/x86 build tool... <br /> 
^^ Again, another version may work, but this is the stable one the tester used<br /> 
![getit3](https://github.com/user-attachments/assets/acbe1981-21ea-4521-a2f1-7ace790a7931)
<br /> 
the above listed items should be all that is necessary. However, the specific list of all installs on the tester's environment were as follows:<br /> 
 ![fullListFrom_desktopDevelopment1](https://github.com/user-attachments/assets/ea540c82-79a0-4e7b-84a1-cb71253915df)
<br />
These followed other default recommendations, but should not be needed. The extra items are documented here for testing/comparison purposes.
<br /> 
Restart the computer after installation (needed to clean up files so the program will work).
Again, you will not be able to run FramePack Studio until you restart your computer so the installation changes take place.

Now, we need to actually install xformers

Go to start and search for: "Developer Command Prompt for VS 2022"
This will let you open a command prompt.

Use this command prompt window to go to your xformers directory
Change the below path to the one where you have xformers stored at on your machine
`cd Path\to\your\folder\FramePack-Studio\xformers`

Then make sure the flash attention is turned off with setting the environment variable to negate it.
`set XFORMERS_FORCE_DISABLE_FLASH_ATTENTION=1`

Then run the installer for xformers as follows
`python setup.py install`

[Note] Installation may bring about the following error:

Raise AssertionError("Torch not compiled with CUDA enabled")
AssertionError: Torch not compiled with CUDA enabled
--> It should list a number of incompatible dependencies, which we need to uninstall from here. We need to get rid of these before installing the correct version.
Execute each of the following and accept each of these with "Y" when it asks to proceed y/n
`pip uninstall torch`
`pip uninstall torchsde`
`pip uninstall torchaudio`

Install the correct working version (currently the one documented below)
`pip install torch torchvision --index-url https://download.pytorch.org/whl/cu126`

Further dependency issues listed by the resolver can be ignored, and FramePack Studio should be functional after this point. 

Possible python version issues:
Make sure you're running python 3.8 or later. A testing user found a possible incompatibility with Python 3.10.0, but it's confirmed to work on python 3.10.11. 
