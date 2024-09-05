# EP-133 MIDI SysEx Thingy

check the `notes` directory for documentation i have found. i also left notes on wav formatting in `notes/notes.hmls`.

---

the `syx` directory has several working `.syx` files that do various things to the EP-133 without the sample tool.
you can trigger sounds (from pads or from internal memory not assigned to a pad).
you can also delete sounds from memory and add a sound.

the `syx/send_kick_to_011/` directory has a set of individual commands (in `.syx` format) that will initiate and transfer a wav file called `012 kick_01.wav` to sample bank `011`.
you can send each individual one in order, or you can just send the single `send_kick.syx` file. the `send_kick.syx` file contains every command to transfer a wav file into one single file.

to send a `.syx` file to the EP-133, you will need [SendMIDI](https://github.com/gbevin/SendMIDI).
there are actually several different applications to send `.syx` files, but this one uses the terminal and is simple to use:

run from the base directory of this repo:
```
sendmidi dev 'EP-133' syf syx/send_kick_to_011/send_kick.syx
```

after the transfer completes, you will have a new wav in sound slot `011` named `kick_01.wav`.

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

---

i am unsure how to pack a wav into the data format that the EP-133 expects during transfer. looking through the `.syx` files gives clues, but i still can't quite figure it all out yet. searching for individual bytes from a wav file in the `.syx` file works, but searching for more than a few bytes will fail, as there seems to be some extra bits inbetween every few bytes. transfer of a wav from the EP-133 and then back onto it, and once more transferring it back does not produce any artifacts, and the resulting wav file is identical every time.
