# KEN-CMD

**A small AI RC car I built myself — and the software I'm sharpening for the Awign Hackathon.**

I've been building KEN (the car's name) for a while now. It drives, it streams live
video to a browser, it stops when it can't see where it's going, and it answers to
my voice. All of it — the firmware, the server, the dashboard, the AI — is my own
work, built solo.

This repo is the public face of that project. The full source code stays private
during the hackathon; happy to walk judges through it in person.

---

## What it does today

- **Drives on command** — a 4-motor ESP32 car, controlled from a browser dashboard
  with keyboard or an on-screen thumbpad, with a speed limiter enforced at the
  server.
- **Sees, all the way around** — an ESP32-CAM on a motorized mount that pans a
  full **360°** and tilts up and down, so the car can look anywhere before it
  commits to a move. Raw JPEG frames push over WebSocket, so the video is real
  and live, not a slideshow. You can drag the camera to aim it.
- **Knows when to stop** — ultrasonic sensors watch the front and rear, and four
  IR edge sensors guard all four corners. The dashboard shows live range and
  edge readings. If commands stop arriving, the car stops itself.
- **Listens, and explains why it moves** — say "Ken, drive forward" and it does,
  then tells you what and why. Voice goes through a wake-word gate, then
  Whisper, then an LLM that decides what to do — and KEN answers with
  `{speech, action}`, so it narrates every move as it makes it ("turning left,
  something in the way"). Vehicle motion always follows sensor verdicts.
- **Follows faces and objects** — in auto mode it locks onto a face or a detected
  object and steers toward it, with obstacle and edge readings overriding the AI,
  always, no exceptions. You can switch targets by voice ("follow Sarah").

## The hardware

| Part | What it is |
|---|---|
| ESP32 DevKit | drive controller, 4 motors |
| ESP32-CAM (OV2640) | live video on a 360° pan/tilt mount |
| Sensor node | ultrasonic front + rear, 4 IR edge sensors |
| JBL GO speaker | voice in/out, paired over Bluetooth to the laptop |

## What I'm improving at the hackathon

The hardware is solid. The next few days are about the software:

1. **Better perception** — improve object detection and tracking so the car
   actually holds onto a target instead of losing it every few frames.
2. **A tighter safety layer** — obstacle readings should veto bad AI decisions
   faster and more predictably.
3. **Real conversation** — natural voice commands ("back up", "follow Sarah")
   that chain together instead of one command at a time.
4. **Reliability** — fewer dropped connections, cleaner recovery when a node
   reconnects.

### Why it moves

KEN never drives blind. Every move runs through the sensor layer first:

- front and rear **ultrasonic** range checks → stop or reverse if too close
- four **IR edge sensors** → no driving off the table
- a **command watchdog** → loss of signal = wheels stop within a second

So when KEN turns, dodges, or stops, it's reacting to what it actually senses —
and it tells you why in plain language as it does it.

## Quick facts

- **Solo project** — I designed and wrote all of it myself.
- **Stack:** TypeScript (Express + ws), Python, C++/Arduino, plain HTML/CSS/JS.
- **Tests:** 18/18 smoke tests passing on the server; all three firmware
  projects compile clean.
- **Safety first:** command watchdog on the car, latched e-stop, no drive
  commands at boot, sensor gate that the AI can't bypass.

## Where to find me

- GitHub: [sce24ic030-ken](https://github.com/sce24ic030-ken)
- Event: Awign Hackathon — Physical AI track, Bengaluru, Oct 10–11 2026

---

*Built solo, over many late nights. Ask me anything about it.*
