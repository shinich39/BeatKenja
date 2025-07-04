# BeatKenja

A collection of Beat Saber automapping tools. This Toolset aims to speed up creating an outline for a Beat Saber map.
It can create timings or whole maps.
It is not meant to be a replacement for a human mapper. It is meant to be a tool to speed up the process of mapping.
It is possible to generate a whole map with this tool, but it will not be a good map (for now). It will be a map that
you can work with.
The map generation is still in its early stages and will be improved.<br>
It can also generate Onsets for a song. This is useful if you want to create a map but not the timings of the song.
The Onset generation is not perfect (yet) and will generate a LOT of false positives. So you will have to check the
timings and correct the mistakes.
<br><br>
The difference to BeatSage is that BeatSage is a full automapper. It will generate a map without any human interaction.
BeatKenja is a tool to speed up the mapping process. It will generate a map that you can work with. It is not meant to
be a replacement for a human mapper.
Furthermore, BeatSage will make a lot of parity errors. BeatKenja will try make as little as possible parity errors. It
will also try not make any DDs or resets. But if it does, it will warn you.
It is not possible to tweak or customise a BeatSage map. But with BeatKenja it is fully possible. (See "Create Map").
<br><br>
This tool is still in its early stages and will be improved.

## Installation

### Prerequisites

1. [Java 21](https://www.oracle.com/java/technologies/javase/jdk21-archive-downloads.html)
2. [Python 3.11+](https://www.python.org/downloads/) (Only needed for onset generation)
3. Install pip (only needed for onsets)
    ```bash
    py -m ensurepip --upgrade
    ```
4. Install dependencies (only needed for onsets):

    ```bash
    pip install librosa
    pip install ffmpeg # if this fails, install it from: https://ffmpeg.org/download.html (and add it to PATH)
    pip install pydub
    ```
   Note that you may need to install ffmpeg manually and add it to PATH. Here is a tutorial for
   Windows [tutorial](https://phoenixnap.com/kb/ffmpeg-windows)

### Installation

1. Download newest .jar file from [Releases](https://github.com/Chrisvenator/BeatKenja/releases). You may put it into
   its own folder. It will generate a few files and folders.

2. Open BeatKenja.jar and let it create a few files and folders. Then close it again.

3. open config.txt and change the defaultPath to your WIP folder (or whatever folder you may like). **replace the
   backward slashes with forward ones!!!**
   Verbose shouldn't be changed except if you want to get spammed by the program

4. Now you are ready to go

## How to use

BeatKenja can be used in two ways: **GUI Mode** (graphical interface) or **CLI Mode** (command-line interface).

### GUI Mode (Default)

Simply run the jar file without any arguments to start the graphical interface. (Sometimes it's even possible to
double-click the jar)

```bash
java -jar BeatKenja.jar
```

### CLI Mode

For automation, scripting, or headless processing, you can use BeatKenja from the command line:

```bash
java -jar BeatKenja.jar --input <input_file> --output <output_file> --mode <mode> [--pattern <pattern_file>]
```

#### CLI Arguments

| Argument    | Short | Description                               | Required |
|-------------|-------|-------------------------------------------|----------|
| `--input`   | `-i`  | Input .dat file path                      | Yes      |
| `--output`  | `-o`  | Output .dat file path                     | Yes      |
| `--mode`    | `-m`  | Processing mode (`linear`, `complex`)     | Yes      |
| `--pattern` | `-p`  | Optional pattern file (.pat or .dat file) | No       |
| `--help`    | `-h`  | Show help information                     | No       |

#### CLI Examples

```bash
# Process a map with complex mode
java -jar BeatKenja.jar --input normal_a.dat --output normal_b.dat --mode complex

# Process with custom pattern file
java -jar BeatKenja.jar --input timings.dat --output expert.dat --mode complex --pattern custom.pat

# Process using another difficulty as a pattern file
java -jar BeatKenja.jar -i input.dat -o output.dat -m normal -p ExpertStandard.dat

# Process with short arguments
java -jar BeatKenja.jar -i input.dat -o output.dat -m simple

# Show help
java -jar BeatKenja.jar --help
```

#### Processing Modes

For full description see [Generation section below](#generation). But for now, the only supported modes are these:

- **`linear`**: Creates basic linear patterns with minimal complexity
- **`complex`**: Creates intricate patterns with higher complexity (may include DDs/resets)

#### CLI Features

- **Validation**: Automatically checks if input files exist and modes are valid
- **Pattern Support**: Load custom patterns from .pat files or existing .dat difficulties
- **Error Handling**: Provides clear error messages for invalid arguments
- **Logging**: All processing is logged for debugging purposes
- **Exit Codes**: Returns appropriate exit codes for scripting integration

#### Pattern Files

The `--pattern` option allows you to customize the generated patterns:

- **`.pat` files**: Custom pattern files created through the GUI's "load patterns" feature
- **`.dat` files**: Use patterns from existing Beat Saber difficulty files
- **Optional**: If no pattern file is specified, default patterns for the selected mode are used

#### CLI Limitations

- Currently supports only single difficulty processing
- Bookmark-based section processing is not available in CLI mode
- Pattern customization requires GUI mode
- Onset generation still requires GUI mode

---

### Onset Creation

<pre>Note that a map is the whole of a map. It contains a song, info.dat and at least one difficulty.
A diff (short for difficulty) is only a single difficulty file (For example: "ExpertPlusStandard.dat").
We will only work with difficulties from this point on. </pre>


**You need to have all prerequisites installed!**

1. Put all your desired Songs into the folder "./OnsetGeneration/mp3Files/". Note: All your files must be **mp3**
   files! (.wav files will probably work too).
2. Start BeatKenja and hit Convert "MP3s to timing maps". This will analyze the song and make a timings map. The output
   will be saved at OnsetGeneration/output/*SongName*.<br>
   The more songs that need to be converted, the longer it will take. You can always review the progress when you look
   into the OnsetGeneration/output folder.
3. Copy all the desired Maps folders from OnsetGeneration/output/ to your WIP folder.
   You can now open your maps in the editor of your choice.
4. Verify the accuracy of the note placements. No algorithm is perfect. So the chance is VERY high that the algorithm
   placed some notes wrong.

If you now want to make a "real" map out of these timings, then have a look at the next chapter:

----

## Map Generation

### Prerequisites

Prepare a timings and a target difficulty. A timings difficulty [(Example)](./assets/timings.gif) is a diff where every
note is a dot note and is located in the bottom left corner. There **MUST ONLY BE BLUE NOTES!!** Or else the program may
not work.<br><br>

If you are unsure to what a timing diff is, then convert an existing diff to a 1 color timing diff and then load it into
your editor of your choice.
***It is recommended that the timing diff is a no arrows diff***<br>
<pre>Map to timing notes --> To 1 color timing notes --> SAVE MAP</pre>
<br>The target difficulty should be the diff that you want to export the diff as. For example if you want to create an
Expert diff with this program, then you should create an Expert diff in the editor of your choice. You will later
overwrite
this diff. THERE WILL BE NO BACKUPS OF THE DIFF YOU OVERWRITE!!
<br>

### Generation

If the timings and target difficulties are prepared, then open BeatKenja.jar and choose the timing diff.<br>
When you click on Map creator you will then have a few options:<br>

+ **Create Linear Map [(Example)](./assets/linear.gif)**:<br>
  A really simple linear map. There should be no DDs or resets. It gets quite boring, quite fast. The swings will *
  *always** be alternating.
  It is possible to make a one-handed Linear Map: Map creator --> one han... (top left option)
+ **Create Complex Map [(Example)](./assets/complex.gif)**:<br>
  A map which can contain quite interesting patterns but the swings will **always** be alternating.<br>
  It might contain DDs or resets. But it will give a warning if it detects some.<br>
  It is possible to make a one-handed Complex Map: Map creator --> complex (top right option)
+ **Create Map [(Example)]()**:
  This is where you can have a LOT of freedom.<br>
  With bookmarks, it is possible to create different sections. The supported bookmarks at the moment:
  <pre>complex | linear | 1-2 | 2-1 | 2-2 | small, normal, big jumps | doubles | sequence (WIP)</pre>
  To create a section choose a **bookmark** from the ones above, and name the bookmark accordingly. The color is
  irrelevant for generating the map.
  Bookmarks can be freely set. There are no real restrictions.
  Just name the bookmark for example "small jump" or "doubles", and it will create a small jump or doubles section.
  A section will go on as long as there are notes in the map or until the next bookmark.
  <br>If a bookmark is directly on a note, then the note will count to the section that begins right there.
  <br>If a bookmark is not supported (or written incorrectly), then it will warn you and use the "complex" in the
  meantime.

  | **bookmark name & description**                                                                                                                                                                                                                                                                                | **GIF**                                        |
          |----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------|
  | **`linear`**<br>Linear quite boring pattern                                                                                                                                                                                                                                                                    | ![linear gif](./assets/linear.gif)             |
  | **`complex`**<br>A little bit more complex patterns. But nothing too complex.<br>**THERE MAY BE RESETS AND DDs**. So always check the map!                                                                                                                                                                     | ![complex gif](./assets/complex.gif)           |
  | **`1-2`**<br>Twice as many blue notes as red notes.<br>The red note will always spawn at the same beat as the blue one.<br>In between red notes there will always be a blue note.<br><br>Red notes generate like "linear" and blue notes generate like "complex".                                              | ![1 2 gif](./assets/1-2.gif)                   |
  | **`2-1`**<br>**<ins>Same as "1-2" but mirrored</ins>**<br>Twice as many red notes as blue notes.<br>The blue note will always spawn at the same beat as the red one.<br>In between blue notes there will always be a red note.<br><br>blue notes generate like "linear" and red notes generate like "complex". | ![2 1 gif](./assets/2-1.gif)                   |
  | **`2-2`**<br>There will be 2 blue notes followed by 2 red notes followed by 2 blue notes followed by...<br>Depending on the starting position of the first red and blue note, both notes may or may not be feel weird.                                                                                         | ![2 2 gif](./assets/2-2.gif)                   |
  | **`doubles`**<br>Red and Blue note will be placed at the same beat.<br>Depending on the previous notes they may be both the same or both different swings                                                                                                                                                      | ![big jumps gif](./assets/doubles.gif)         |
  | **`small jumps`**<br>Depending on the previous notes the blue or red note comes first                                                                                                                                                                                                                          | ![small jumps gif](./assets/small_jumps.gif)   |
  | **`normal jumps`**<br>Depending on the previous notes the blue or red note comes first                                                                                                                                                                                                                         | ![normal jumps gif](./assets/normal_jumps.gif) |
  | **`big jumps`**<br>Depending on the previous notes the blue or red note comes first<br>*Currently broken D:*                                                                                                                                                                                                   | ![big jumps gif](./assets/big-jumps.gif)       |
  | **`sequence`**<br>*Currently under construction*                                                                                                                                                                                                                                                               | *to be added*                                  |

<br>

Then at last, hit SAVE MAP and select the target difficulty that you created earlier and hit save.<br>
BE AWARE! THERE WILL BE **NO BACKUPS** OF THE OVERWRITTEN DIFFICULTY!! Check twice if you overwrite the **correct**
diff!

### Pattern customisation

It is possible to change the patterns of "Create Complex Map" and "Create Map".
There is a button named "load patterns". just simply load a .pat file and watch the patterns change.
Additionally, you can also load a difficulty file from a map.

TODO: Explain what load patterns does<br>

----

### Variance Slider & Visualizing Pattern Distribution

It is possible to change the variance of the pattern.

- When the variance is low, the program will always generate a similar pattern and often repeat certain notes. There is
  a low chance of parity breaks.
- When the variance is high, the program will generate a variety of different notes and patterns. There is a high chance
  of mapping errors and parity breaks!

You can always display the Pattern under "Visualize Pattern" and then "Normalized Heatmap".
The more intense the color blue is, the higher the chance that it will get picked.

_Note that if the Slider only works as intended, if it has seen all notes it is supposed to generate._

Example of low variance:
![Low Variance Example](assets/variance_low_variance.png)

Example of high variance:
![High Variance Example.png](assets/variance_high_variance.png)

Example of very high variance:
![Very High Variance Example.png](assets/variance_very_high_variance.png)

----

## Map Utilities

+ **Make diff into a no arrow diff**
+ **Convert all flashing lights**<br>
  This converts all flashing light events into regular on events.
+ **Delete Note Type**. Blue: 1 and Red: 0
+ **fix Placements**
  This moves all placements to (by default) 1/16 of a beat

----

## Known Issues:

+ **V3 Maps are finally supported** YAY
+ **There are holes in the map:** No Algorithm is perfect. But I will fix that sometime in the future
+ **Map won't load in Beat Saber:** There may be something wrong with a difficulty file. Just go into the editor of your
  choice and open and save every difficulty. That should fix it.
+ **The map I generated doesn't show up in the editor:** TBD
+ **The map didn't change after generating a new one:** There may be 2 possibilities:
    1. The Program didn't feel like generating something
    2. Chromapper caches difficulties. If this is the case, exit and then reopen the difficulty. That should fix it.
+ **mp3 to wav conversion is broken:**
+ **mp3 to wav overrides the wav file:**

----

## Currently under construction:

- [ ] Advanced Complex Map generation (wip)
- [ ] Change to another Pattern when the map gets faster (wip)
- [ ] Fix timings & swing patterns when the map gets faster (wip)

## TODOs:

- [ ] creating new pattern types:
    - [x] doubles
    - [x] (small, normal, big) jumps
    - [ ] more variation
    - [x] stacks
- [x] Variation Slider
- [x] Audio Analysis via Spectograms
- [x] Slightly better music onsets
- [ ] Better music onsets
- [ ] Good music onsets
- [x] Database Support
- [x] Parity checking
- [x] Ignore DDs
- [x] proper logging
- [x] Added a config
- [x] open all maps in directory
- [x] Dark Mode
- [x] Finish README.MD
- [x] FIX BIG JUMPS
- [x] .pat file extension format loading
- [x] add V3 support
- [ ] mouse over support
- [x] Aim of the tool?
- [x] Error message: "No audio files found in '/OnsetGeneration/wavFiles'"
- [ ] Max length Path for Windows users
- [x] CLI Support

## Future Ideas:

Features that may or may not be implemented in the future

- [ ] **Deep Learning Models:** Train a model on a large dataset of Beat Saber maps and their corresponding audio files.
  The model could learn to predict sequences of moves that not only follow the beat but also the mood and style of
  different parts of a song.
- [ ] **Dynamic Probability Adjustments:** Instead of static probabilities, adjust them dynamically based on certain
  triggers or sections in the music. For example, increase the complexity during a chorus or a bridge.
- [ ] **Pattern Variation:** Instead of focusing solely on the next note's probability, consider sequences of moves or
  introducing special patterns that can occur under certain conditions, like rapid sequences or alternating patterns
  that match the music's intensity.
- [ ] **Difficulty Scaling:** Scale the map's difficulty based on the song's progression or introduce difficulty spikes
  that correspond to climactic moments in the music.
- [ ] **Customizable Parameters:** Allow users to adjust the probabilities and patterns used by the algorithm to create
  maps that match their preferences or the style of a particular song.
- [ ] **Real-Time Feedback:** Provide real-time feedback on the generated map, such as visualizations of the note
  placements and patterns, to help users understand how the algorithm is interpreting the music and make adjustments as
  needed.
- [ ] **Customization Options:** Let users set preferences for map characteristics, like favoring certain patterns,
  complexity levels, or even specific movements, and use these preferences to tailor the map generation.
- [ ] **Advanced CLI Features:** Batch processing, configuration files, and more CLI-specific features.