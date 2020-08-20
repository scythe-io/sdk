# Module Buster
Thank you for your interest in SCYTHE Module Buster 1100.

Module Buster is a wizard that guides you through the module creation process. You can also use Module Buster to test your module and package the module for use in the Marketplace.

This readme assists with the process of developing a module for SCYTHE. The Module Development Guides describe additional implementation details.


# Credits
* Ron Warren - Module Buster Frontend
* Larry White - Module Buster Backend
* Ateeq Sharfuddin - Original concept and server side

We are huge fans of a certain series.

# Prerequisites for Running Module Buster
The following are needed to run Module Buster on a given OS. If you experience issues with Module Buster functionality, verifying that these prerequisites have been met is an important first troubleshooting step. 
## Prerequisites for Windows
1. Python 3.8.2
2. The [jsonschema](https://pypi.org/project/jsonschema/ "jsonschema") Python package.
2. The [pycryptodome](https://pypi.org/project/pycryptodome/ "pycryptodome") Python package 
	1. If the Crypto package is already installed, uninstall it before installing pycryptodome.
1. Visual Studio 2019: used to compile and package the native module, and also to package the Python module.
1. To test with SCYTHE, ensure SCYTHE is running in Developer mode and also that the Marketplace key-pair has been setup.

## Prerequisites for macOS
1. Python 3.8.2
2. The [jsonschema](https://pypi.org/project/jsonschema/ "jsonschema") Python package.
2. The [pycryptodome](https://pypi.org/project/pycryptodome/ "pycryptodome") Python package 
	1. If the Crypto package is already installed, uninstall it before installing pycryptodome.
2. To test with SCYTHE, ensure SCYTHE is running in Developer mode and also that the Marketplace key-pair has been setup.

## Prerequisites for Debian Linux
1. Python 3.8.2
2. The [jsonschema](https://pypi.org/project/jsonschema/ "jsonschema") Python package.
2. The [pycryptodome](https://pypi.org/project/pycryptodome/ "pycryptodome") Python package 
	1. If the Crypto package is already installed, uninstall it before installing pycryptodome.
2. To run `make` in order to build modules you'll need the [gcc-multilib](https://packages.ubuntu.com/xenial/gcc-multilib "gcc-multilib") package
2. To test with SCYTHE, ensure SCYTHE is running in Developer mode and also that the Marketplace key-pair has been setup.


# Getting Started
## Modifying default settings
When you first start Module Buster you'll need to make changes to some of the default settings to accomodate your development environment and preferences. 

1. **Working Directory** - By default this is set to your Desktop, you don't need to change this, but can if you wish.
2. **Path to SCYTHE SDK** - By default this is set to use the SCYTHE SDK that comes bundled with Module Buster. You should only change this setting if you have a very good reason.
3. **Path to Python Executable** - This is set to the default Python3 installation path for your operating  system. If you have Python3 installed at a different location you'll need to update this setting, otherwise module validation and package finalization will not work.
4. **Path to SCYTHE Certificate** - This setting corresponds to the default value for the _Select path to public key file_ field on the _Finalize Marketplace Package_ screen. This defaults to `scythe.crt` in your operating system's home directory. You'll need to update this setting with the location of your SCYTHE developer certificate or public key file (.cer, .crt, .key,and .pub supported), otherwise you'll need to manually select your certificate/public key file each time you finalize a package.

5. **Path to private key file** - This setting corresponds to the default value for the _Select path to private key file_ field on the _Finalize Marketplace Package_ screen. This defaults to `id_rsa` in your operating system's home directory. You'll need to update this setting with the location of your private key file (.key supported). Otherwise you'll need to manually select your private key file each time you finalize a package. This private key is tied to the certificate issued to you by SCYTHE.

Click _Update_ to save these new settings. You will see a message confirming that your new settings have been saved.

![Settings Updated Successfully](images/settings_updated.png)


If for some reason you need to reset your settings to default, just click _Reset to Default_. You'll be prompted to confirm your choice, click _Confirm Reset_ to proceed. Your current settings will be permanently erased and the defaults restored.

 
## Creating a new module
1. Start Module Buster
1. Choose *New Module* or use the keyboard shortcut **Ctrl+N** (Windows and Debian Linux)/**Cmd+N** (macOS)
1. In Create a New Module, select the configuration for your module and input the necessary text, such as company name, module name, display name, description, icon, and path to save the module. *All fields are required unless otherwise indicated.*
1. Click *Create New Module*.

  ![New Module Figure](images/new_module_comp.png "New Module")


At this point, you should see a results screen like the one below, indicating that your new module was successfully created. Module Buster has created a skeleton at the highlighted path with all the files necessary to build the module. Clicking the path will open it in the system default file explorer.

Clicking *Target Additional Platform* will return you to the Create a New Module screen while preserving all previous selections and inputs so you can quickly make configuration changes and create versions of your module for other operating systems.

![Results Screen: Module Successfully Created](images/module_created_success.png)

If there's an issue with the information you've input or Module Buster is improperly configured, causing creation to fail, Module Buster will display a failure message on the results screen with the nature of error(s). For instance in the example below, module creation failed due to a missing module template file, indicating that the path to SCYTHE SDK directory in Settings is incorrect.

![Results Screen: Module Creation Failed](images/module_created_fail.png)



## Building your module
**NB - The build process is OS-dependent; in order to build native Windows modules you will need a Windows environment, and so on**

### Windows
1. Open a Visual Studio Command Prompt.
1. In this VS Command Prompt change the current working directory to the path provided after completing _Creating a new module_ section above.
1. Inside this folder, you will notice a `build.bat` and `build.xml`.
1. Run `build.bat`.
1. This will generate a `package.zip` for you under the bin directory.

### macOS and Debian Linux
1. Open a Terminal.
1. Change the current working directory to the path provided after completing _Creating a new module_ section above.
1. Inside this folder, you will notice a `Makefile`.
1. Run `make`.
1. This will generate a `package.zip` for you under the bin directory.


## Installing your module
1. You can install the `package.zip` generated in _Building your module_ in SCYTHE.
2. Log into SCYTHE and navigate to Administration, Modules.
3. In the Install a module section, click Choose File.

  ![New Module Figure](images/choose_package_zip.png "New Module")

4. Search for *.zip instead of *.arca files and select your `package.zip`.

![Install Module Figure](images/install_module.png "Install Module")

5. Click _Install_.

![Installed Module Figure](images/installed_module.png "Module installed")

## Running your module
1. Start a new campaign from SCYTHE
2. When the client becomes active, open the Shell for the client.
![Active campaign](images/active_campaign.png "Shell")
3. If this is a Python3 module, ensure the runtime is loaded:
```
loader --load-runtime python3
```
4. Load your module:
```
loader --load scythe.echo
```
5. Run help for your module:
```
scythe.echo --help
```
6. Send a command to your module:
```
scythe.echo --message "Hello"
```

## Validate your module
You can use Module Buster to validate that your module will run on SCYTHE prior to submitting it to Marketplace. 


1. Start Module Buster
2. Choose _Validate Module_ from Module Buster's home screen or use shortcut **Ctrl + M** (Windows and Debian Linux)/**Cmd + M** (macOS).
1. Select your module directory (this is the same directory displayed at the end of the _Create a new module_ section
	1. **NB - Module validation is OS-dependent; in order to validate a Windows module you will need to be running Module Buster in a Windows environment, and so on.**
1.  Select which architecture you want to validate.
2.  If your module has any additional dependencies, such as multi-language support scripts, select them.
3.  Enter a command to use to test the module.
	1.  **NB - This command must be wrapped in quoatation marks, for example:** `"--message 'Hello there!'"` 
1. If you're validating a Python3 module, you will see an additional field for the runtime path. Module Buster comes bundled with the necessary runtimes for testing of all supported module configurations. Click the file select button on this field, the resulting file explorer will default to Module Buster's bundled resources directory. Navigate to the "runtimes" folder, select the appropriate runtime file for your module's target runtime, operating system, and architecture.
![Module Validation Screen: Runtime Selection](images/select_runtime.png)

8. Check the _Verbose_ setting if you want to view the Base64 output and module information returned by your module's GetInfo function along with the validation results.
![Module Validation Screen](images/validate_module.png)
9. Click _Validate Module_


At this point you should see a results screen displaying your validation results and info.

![Results Screen: Module Validation Success](images/module_validation_comp.png)

If there are any problems during validation you'll see a screen with a number of errors. In the example below the python module's `package.zip` couldn't be loaded, so validation could not run.

![Results Screen: Module Validation Failure ](images/module_validation_fail_comp.png) 

Additionally you could see an error that looks something like this:

![Results Screen: Module Validation Failure JSONSCHEMA Missing](images/module_validation_fail_2.png) 

This means that you need to install the [jsonschema](https://pypi.org/project/jsonschema/ "jsonschema") Python package as mentioned in the **Prerequisites** section.

If you encounter validation errors you may want to save the validation results for quick reference when debugging your module; to do this just click _Save Results_. Additionally, if you need to make configuration changes or correct input errors, you can  re-run validation by clicking _Validate This Module Again_, which will take you back to the previous screen while preserving all your selections and inputs.

## Validate your package
You can use Module Buster to validate the package.zip created when you build your module.

1. Start Module Buster
2. Choose _Validate Package_ from Module Buster's home screen or use shortcut **Ctrl + B** (Windows and Debian Linux)/**Cmd + B** (macOS).
2. Select the package.zip you want to validate
2. Click _Validate_

![Results Screen: Package Validation Success](images/package_validation_success.png)
 
You should see a results screen indicating your package has passed validation, however if there are problems with your package.zip you will see a failure screen outlining the issues found:

![Results Screen: Package Validation Failure](images/package_validation_fail.png)
In this example validation failed because package.zip is missing the module icon image file.

## Finalizing package
Once you are ready to submit the module package to SCYTHE Marketplace, use your private key to sign the package, and your SCYTHE developer certificate or provided public key to encrypt the package. Transmit this package to SCYTHE for approval over Marketplace.

1. Start Module Buster
2. Choose _Finalize Marketplace Package_ from Module Buster's home screen or use shortcut **Ctrl + F** (Windows and Debian Linux)/**Cmd + F** (macOS).
3. Select the package.zip of the module you want to submit to the Marketplace.
4. If you want to use a different developer certificate and private key than those defined in your _Settings_, select new ones here.
5. Click _Finalize_
![Package Finalization Screen](images/finalize_package_2.png)

At this point you should see results screen indicating that your module package has been successfully finalized and is ready for submission to the SCYTHE Marketplace. Two paths will be listed: the path to the Retail Module Wrapper, "package.rmw" and a path to the package digest, "signature.txt". Clicking either file path will open its location in the system default file explorer.

![Results Screen: Package Finalization Success](images/package_finalization_success.png)

When attempting to finalize a package your may see an error that looks like this:

![Results Screen: Package Validation Failure](images/package_finalization_fail.png)

The exact text of this error will vary between operating systems, but notice the `No module named 'Crypto'`. This indicates that you need to install the [pycryptodome](https://pypi.org/project/pycryptodome/ "pycryptodome") module for Python3 as mentioned earlier in this guide.

## Viewing logs
You can view application logs by selecting _View Logs_ from the Module Buster home screen or using shortcut **Ctrl + L** (Windows and Debian Linux)/**Cmd + L** (macOS). The most recently logged event is listed last.