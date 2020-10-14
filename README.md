# folder2mkv
Bash script for batch merging content to mkv files in folder by only one MKVToolNix option file. The core feature is to take additional files in other folders with same names as input files to add them in result output file. Script also don't change any information about attachments if you need put same attachment to each output file. Optionaly it can use title information from [Kodi's local .nfo metadata files](https://kodi.wiki/view/NFO_files/TV_shows) if they had the same name as input video files and locate in the same folder with input video files. 

**Option file must be created in MKVToolNix. Script don't able to work without it.** (Go to `Multiplexer > Create Option File`) 

Script use [mkvmerge](https://mkvtoolnix.download/doc/mkvmerge.html)

Posible arguments:
* `<inputPath>` - path to the directory with input video files.
* `<extention>` -  video file extension (`mkv`, `mp4`, `avi`, etc.) that will be used to find files in `<inputPath>` (is `mkv` by default if not defined).
* `<scriptFile>` - path to the MKVToolNix option file .
* `<outputPath>` - path to the output directory for result mkv files (is the same directory as `<inputPath>` by default if not defined).

Usage:                                                                  
* `folder2mkv <inputPath> [<extention>] [<scriptFile>] [<outputPath>]` \
or
* `folder2mkv [options]` \
or
* `folder2mkv` (without any aguments for the secret mode) 

Options:                                                                        
* `-c, --cover [<coverFile>]` - add custom cover picture to the result mkv files. One picture for all output files. Picture file must be `.jpg` or `.png` format. If `<coverPath>` is not defined picture will be taken from `<inputPath>` folder with `seasonXX-poster.**g` or `cover.**g` or `poster.**g` filename.                      
* `-e, --extension <extension>`                                                
* `-i, --inputPath <inputPath>`                                                
* `-o, --outputPath <outputPath>`                                               
* `-s, --scriptFile <scriptFile>`                                               
* `-t, --title` - add custom title to metadata of the result mkv file. Title line will be replaced with name of the input file. Custom title can be parsed from the kodi's .nfo infomation file that has the same name as input video file. If add `%title%` to any mkv track name (e.g. Video track) script also change it to same custom title  
* `-h, --help` for help.                        
                                                                                
NOTE: Usage cases #1 and #2 can be mixed for your convenience. Only need to keep the order of the arguments of case #1 style.                              
Example: `folder2mkv <inputPath> -ct <outputPath>`

NOTE: `-c` and `-t` options can be mixed in `-ct` or `-tc`, but without any ~`<coverFile>`~ string. 
                                                                            
### How it's works

Script will parse MKVToolNix option file into the string and will search for input files in `<inputPath>`folder with defined `<extention>`. If `--scriptFile` option is not defined, script will try to find `script.json` file in `<inputPath>` folder and will stop if can't find it. Script will make temporary folder in `<inputPath>` with name which stored in `tempFolderName` variable in script itself. By default the name is `mkvtmp`. You can change it whatever you want. Then if `--cover` option is defined script will copy cover picture in temporary folder for later use.  If `--title` option is defined, script will parse `.nfo` files for title or will use input file name for it. Then script will parse each MKVToolNix option string to change some information and to add some extra options for `mkvmerge`. After that it will execute result string. Result output file will move in `<outputPath>` or will replace input file in `<inputPath>` if `<outputPath>` id not defined.  

Example of MKVToolNix option file
```
[
  "--ui-language",
  "en_US",
  "--output",
  "/home/user/video/Wedding/Day 1/In Bride's house.mkv",
  "--language",
  "0:en",
  "--track-name",
  "0:%title%",
  "--default-track",
  "0:yes",
  "--display-dimensions",
  "0:1920x1080",
  "--language",
  "1:en",
  "--track-name",
  "1:Camera microphone",
  "--default-track",
  "1:yes",
  "(",
  "/home/user/Download/Wedding/Day 1/Uncle Comments/In Bride's house.mka",
  ")",
  "--language",
  "0:en",
  "--track-name",
  "0:Uncle Josh's Comments",
  "(",
  "/home/user/Download/Wedding/Day 1/Ricky's Music/In Bride's house.wav",
  ")",
  "--language",
  "0:en",
  "--track-name",
  "0:Ricky's Background Music",
  "(",
  "/home/user/Download/Wedding/Day 1/Subtitles for Grandma/In Bride's house.ass",
  ")",
  "--track-order",
  "0:0,0:1,1:0,2:0"
]
```
Example of output parsed string
`/usr/bin/mkvmerge --ui-language 'en_US' --output '/home/user/video/Wedding/Day 1/In Bride's house.mkv' --language '0:en' --track-name '0:%title%' --default-track '0:yes' '--display-dimensions' '0:1920x1080' --language '1:en'` and etc.

### Extra notes
It is my old script that I use sometimes. Version in this repository is refactored and can do nothing or break video files. So use it at your own risk. 

I don't use MKVToolNix settings file because is too difficult to parse it. 

I will accept any issues, but fix may take too long. 
