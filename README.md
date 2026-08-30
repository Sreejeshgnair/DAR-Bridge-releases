# DAR Bridge

**Control the Dolby Atmos Renderer from your Avid surface.**

Monitoring level, Dim, Cut, speaker layouts, transport and loudness, all from the desk instead of reaching for the Renderer window every time. Works with S6, S4, Artist, S1, S3, Dock and Avid Control.

It also opens up HTTP and OSC on the same Mac, so Stream Deck and SoundFlow can drive the Renderer too.

Free. Signed and notarised by Apple. One build runs on Apple Silicon and Intel.

**[Download the latest version](../../releases/latest)**

This is a personal project. It is not made by, endorsed by or supported by Avid or Dolby.

---

## Download and Install

1. Grab **`DAR-Bridge-1.2.1.dmg`** from the Releases page.
2. Open the DMG and drag **DAR Bridge** into **Applications**.
3. Launch it. The app is signed and notarised by Apple, so it opens without any security warning.
4. When macOS asks for **Local Network** access, click **Allow**. This one is important. Without it the surface cannot see the app at all.

**Requirements:** macOS 13 or later. One build runs on Apple Silicon and Intel. You also need EuControl or WSControl installed, and a Dolby Atmos Renderer reachable on port 4010, either on this Mac or somewhere on your network.

If you missed the Local Network prompt, you can switch it on later in **System Settings, Privacy and Security, Local Network, DAR Bridge**.

## Setting Up Your Surface

S4 and S6 use **WSControl**. Artist, S1, S3, Dock and Avid Control use **EuControl**. The steps are the same in both.

1. Launch DAR Bridge and keep its window in front. There is also a **Bring to Front for WSControl** item in the menu bar if you need it.
2. Open **WSControl Settings** or **EuControl Settings**, and go to **Applications**.
3. Check that **Last Focused Application** shows **DAR Bridge**. If it shows up grey or says non managed, see the troubleshooting below.
4. Lock **Transport** to DAR Bridge.
5. Lock **Monitor and Control Room** to DAR Bridge.
6. Open **Soft Keys** and assign whatever you want from **Key Commands**. Soft keys will not appear on the S6 until you assign them yourself.

## What You Get on the Surface

**Monitor section**

| Control | What it does |
|---------|--------------|
| Main knob | Monitoring level, in dB or on the cinema fader scale, follows the Renderer |
| Dim | Dim on and off |
| Cut | Global mute |
| Main, Alt 1, Alt 2 | Physical, 7.1 and 5.1 |
| Talk | Switches between Master and Input |

**Settings**

Two options live in Settings, under EuCon.

| Option | What it does |
|--------|--------------|
| Enable / Disable Gain Control from Surface | Turn it off and the Main knob stops changing the Renderer level. It still follows along, so you can see where the room is sitting, it just will not move it. The Cinema soft keys keep working, so you can still recall a level on purpose. Handy when the room level belongs to the cinema processor and you do not want the knob nudged by accident. |
| Show level as Dolby Cinema scale | The Main knob reads in cinema fader numbers instead of dB. 7.0 is reference, 4.0 is 10 dB down, and 0.0 is off, following the standard Dolby fader taper. 7.0 is as high as it goes, because the Renderer will not go above unity. |

**Soft keys**

Transport (Play, Pause, Stop, Ext Sync, Rec Arm), mutes (Dim, Mute, Bed, Obj), all the layouts (Physical, 7.1, 5.1, 7.1.2, 5.1.2, 2.0, Stereo Direct), source (Input, Master), loudness (Atmos, Binaural, Reset, Pause, Analyze), view toggles, gain presets (0 dB and Cinema 7.0 down to 5.0) and Master Close.

## Stream Deck and SoundFlow

The app runs a small bridge on this Mac, so you can also control the Renderer over HTTP or OSC.

- HTTP on `http://127.0.0.1:8787`
- OSC in on UDP `9000`, feedback out on `9001`

A quick example:

```bash
curl -s -X POST http://127.0.0.1:8787/command \
  -H 'Content-Type: application/json' \
  -d '{"op":"dim","value":true}'
```

`GET /state` gives you everything the Renderer is doing right now, which is what you want for lighting up buttons on a deck. There is a full command list in the user manual inside the DMG.

## Troubleshooting

**The surface shows DAR Bridge as non managed, or the locks are greyed out.**

This is almost always Local Network permission. Switch it on in System Settings, Privacy and Security, Local Network, then quit DAR Bridge and open it again. Restart WSControl or EuControl after that. If you want to confirm, look in `~/Library/Logs/Avid/EuCon/` and check for `No Auth` or `Registering Service FAILED` in the DAR Bridge log.

**It keeps saying Searching.**

The app cannot find your Renderer. Check the Renderer is actually running and that port 4010 is reachable from this Mac. If the Renderer is on another machine, put its IP into Settings instead of leaving it on Auto.

**Soft keys do nothing.**

Assign them first in Key Commands, and make sure the status says Connected.

**The app is running warm.**

That is the sign of Local Network being denied. Grant it and the app settles down straight away.

**Record arm or Analyze will not work.**

Load a master first. Record arm also needs a master that is writable.

## Notes

Closing the window does not quit the app. Use the menu bar icon and choose Quit.

Logs live in `~/Library/Logs/DAR Bridge/`.

The DMG also carries a standalone `DAR-Bridge` command line tool. You only need that if you want HTTP and OSC without the surface side. Do not run both at once, they share the same port.

---

## Please Note

This is a personal project. I built it for my own room because I was tired of reaching for the Renderer window in the middle of a mix, and I am sharing it in case it is useful to you too.

It is **not** an Avid product and **not** a Dolby product. It is not made by, endorsed by, supported by or affiliated with either company. Please do not contact Avid or Dolby support about it. If something is not working, raise an issue here instead and I will help if I can.

It is offered free and as is, with no warranty of any kind. You are using it at your own risk. Do test it properly in your own room before you rely on it for a paying session.

**Tested on WSControl 2026.4.** Other versions will very likely work, but that is the one I have actually run it against. If you try it on a different version, do let me know how it goes so I can add it here.

Avid, EuCon, EuControl, WSControl, S4, S6, Artist and Avid Control are trademarks of Avid Technology. Dolby and Dolby Atmos are trademarks of Dolby Laboratories. Both are used here only to describe what this app works with.
