# Setting up HIVE
For the remainder of the book, the directory where you installed HIVE will be called the *Application Root*. It will also be represented by the *./* in file names. It is important at this point that you verify that at least the system administrator has read/write/execute rights to the *Application Root* and its contents.

Open up *settings.ini* from the *Application Root*. Since the HIVE downloads are uploaded directly from the Continious Integration software, there should already be a few settings in this file. A simple settings file would look something like this:

    MODFOLDER=C:\*APPLICATION ROOT*\modules
    LANG=en-us
    FS=C:\*APPLICATION ROOT*\filestore
    UNITID=B1F1R1U1
    INTER=-1

*./settings.ini*

For a first run in a lab enviroment, all you would need to do is replace *APPLICATION ROOT* with the actual path to your istallation of HIVE. If you just want to get up and running, copy this to your *./settings.ini* file, then jump to section 1.5. If you need a more andvanced configuration, see below.

## Config file syntax
### Comments
If you are familiar with programming, you may understand what a comment is. A comment is a line inside of a file that the interpreter/compiler ignores. The purpose of a comment is to tell the reader, in english, what that line of code does. It helps organize code files and is a great habit to obtain in any branch of computer programming.

To declare a line a comment, start your line like this:

    //
    
After this line, you may put any text you wish. Do note that within config files, HIVE does not support adding comments on the same line as code. This will either cause an error or some **really** interesting results.

### Keys and values
In a HIVE config file, The text on the left side of the *=* sign is the name of the setting you are modifying/setting. This is known as the **key**. Keys are always in all caps. Anything to the right of the *=* sign is the **value** you are assigning to the key. Table A shows a list of all supported keys and suggested values.

*Figure A*

| Key | Value type |Values supported |
| -- | -- | -- |
| UNITID | String | A name to identify the unit in the HIVE. Must be unique from all nodes on the network. |
| INTER | Integer | Number of network interface to use. Put -1 to pick the first non-loopback interface. |
| LANG | String(locale) | Name of the locale used in your country. When in doubt, pick 'en-ALL'. |
| MODFOLDER | String(filepath) | Path to the folder containing extension modules. |
| FS | String(filepath) | Path to a folder where you would like the File Store folder created. Put on an SSD if possible. |