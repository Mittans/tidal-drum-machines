## Raison d'etre

When running the CodeMirror REPL Starter code from codeberg.org/uzu/strudel/src/branch/main/examples/codemirror-repl
I found it was broken because the drum machine sample list that lives on Strude.cc is not available to your local instance. 
You can't fetch them from Strudel.cc because you're running a localhost server, and their CORS config rejects your requests.
The actual tidal-drum-machines drum machines don't have a drum-machines.json for your REPL to understand the samples available and their folder structure.

## Checking if you need to use this repo

Try adding `s("bd").bank("RolandTR808")` and clicking play. Do you get any sound? If yes, you're fine, don't need this.

## Usage for your Strudel project

Just add:

`samples('https://raw.githubusercontent.com/Mittans/tidal-drum-machines/main/machines/tidal-drum-machines.json')`

To your Strudel REPL. Now you can `s("bd").bank("RolandTR808")` all day.

Original README.md follows.

# tidal-drum-machines
 A huge collection of Drum Machines for SuperDirt and Tidal

### List of drum machines

See the full list of drum machines [here](/machines).

---

### Installation

```supercollider
// install this repository
Quarks.install("https://github.com/geikha/tidal-drum-machines.git");

// add this to your superdirt startup
~drumMachinesDir = Quarks.all.detect({ |x| x.name == "tidal-drum-machines" }).localPath;
~nameFunc  = { |x| x.basename.replace("-","")};
~dirt.loadSoundFiles(~drumMachinesDir +/+ "machines" +/+ "*" +/+ "*", namingFunction: ~nameFunc);
// Windows Users can use this line instead: (~drumMachinesDir +/+ "machines" +/+ "*").pathMatch.do({ |x| ~dirt.loadSoundFiles(x +/+ "*", namingFunction: ~nameFunc) });

// test in sclang
(type:\dirt, s: \rolandtr909cr, n: 0).play;
```
Thanks [Julian](https://github.com/telephon) for the installation script!

---

## How to use

Run the custom SuperCollider bootup found in [tdm-sc-boot.scd](/tdm-sc-boot.scd), or add the necessary parts to your own bootup. Then run the haskell/tidal code found in [tdm-hs-setup.tidal](/tdm-hs-setup.tidal), or just copy and paste it from here:

```hs
let drumMachine name ps = stack 
                    (map (\ x -> 
                        (# s (name ++| (extractS "s" (x)))) $ x
                        ) ps)
    drumFrom name drum = s (name ++| drum)
    drumM = drumMachine
    drumF = drumFrom
```

### Examples

Here are some examples of how to use the drum machines:

#### drumMachines

```hs
d1 $ drumMachine "bossdr220" [
    s "[~perc]*2" # note 7
    ,s "bd:4(3,8)"
    ,s "~[cp,sd]"
    ,s "hh*8"
]
```

The drum machine can be pattern'd:
```hs
d1 $ drumMachine "<bossdr220 rolandtr808>" [
    s "[~perc]*2" # note 7
    ,s "bd:4(3,8)"
    ,s "~[cp,sd]"
    ,s "hh*8"
]
```

#### drumFrom

You can also just call one percussive element:

```hs
d1 $ drumFrom "linn9000" "bd*2"
```

This method could be useful for live performance:
```hs
do
 let dm = "linn9000"
 d1 $ drumFrom dm "bd*2"
```

---

### Drum names abbreviations:
| Drum                                | Abbreviation |
|-------------------------------------|--------------|
| Bass drum, Kick drum                | bd           |
| Snare drum                          | sd           |
| Rimshot                             | rim          |
| Clap                                | cp           |
| Closed hi-hat                       | hh           |
| Open hi-hat                         | oh           |
| Crash                               | cr           |
| Ride                                | rd           |
| Shakers (and maracas, cabasas, etc) | sh           |
| High tom                            | ht           |
| Medium tom                          | mt           |
| Low tom                             | lt           |
| Cowbell                             | cb           |
| Tambourine                          | tb           |
| Other percussions                   | perc         |
| Miscellaneous samples               | misc         |
| Effects                             | fx           |
