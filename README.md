# EP-133 MIDI SysEx Thingy

check the `notes` directory for documentation i have found. i also left notes on wav formatting in `notes/notes.hmls`.

---

the `syx` directory has several working `.syx` files that do various things to the EP-133 without the sample tool.
you can trigger sounds (from pads or from internal memory not assigned to a pad).
you can also delete sounds from memory and add a sound.

the `syx/send_kick_to_011/` directory has a set of individual commands (in `.syx` format) that will initiate and transfer a wav file called `012 kick_01.wav` to sample bank `011`.
you can send each individual one in order, or you can just send the single `send_kick.syx` file. the `send_kick.syx` file contains every command to transfer a wav file into one single file.

to send a `.syx` file to the EP-133, you will need [SendMIDI](https://github.com/gbevin/SendMIDI).
you can also use a GUI sysex application called [SysEx Drop](https://github.com/sourcebox/sysex-drop/releases/tag/v1.4.0). just drag one of the `.syx` files into it and make sure to set it to send data to the EP-133.

### WARNING : use at your own risk. i take no responsibility for any damage done to your device
### this will overwrite sounds 1, 2, 3, 4!

run from the base directory of this repo:
```
sendmidi dev 'EP-133' syf syx/send_1234.syx
```

after the transfer completes, there will be a kick, snare, hat, and sample in sound slots 001,002,003,004 respectively.

there are several other syx files in the `syx` directory that do various things. 
  * the `init.syx` file should be added to be beginning of any command that involves sending a sound wav to the device. the .syx files provided in this repo already have this init included, so you can run any .syx file in this repo without worry of error. but if you wanted to create your own wav transfer, the `init.syx` should be appended to the beginning of the transfer.
  * the `switch_to_project_6.syx` file shows an example of how to instantly switch to project 6.
  * the `delete_sample_001.syx` file will literally erase whatever sample is in slot 011 on your EP-133.


there are many more that i have added since and i can't document all of them. i hope this data is useful.

---

the `sounds` directory contains a directory called `original` and another called `from_ep_133`.

  * sounds
    * original
    * from_ep_133

the sounds in `original` are exactly the same as the sounds in `from_ep_133`. the difference between the wav files is that the ones in `from_ep_133` have headers. the header looks like this:

```
{
  "sound.playmode":"oneshot",
  "sound.rootnote":60,
  "sound.pitch":0,
  "sound.pan":0,
  "sound.amplitude":100,
  "envelope.attack":0,
  "envelope.release":255,
  "time.mode":"off"
}
```

in order to send a wav to the EP-133, it should be a 46875 sample rate wav, with the header above included.
