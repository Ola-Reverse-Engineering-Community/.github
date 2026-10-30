# Ola Reverse Engineering Community

A community hub for reverse engineering Ola Electric scooters — the head unit,
the CAN bus, the HMI, the update path — with an emphasis on **measured facts**,
**reversible steps**, and **not breaking anyone's vehicle**.

Everything here is done on a scooter the contributor owns, over the local
network. No flashing. No partition writes. No leaked keys.

---

## What we are

Ola's dashboard is an Android computer bolted to a vehicle. That makes it
unusually open to study: it runs a stock-ish Android 13, it bridges the CAN bus
into Android as `vcan0`, and — on the E2 platform we have looked at — it ships a
root daemon listening on TCP 50005 with **no authentication**.

We document what that means in practice, so an owner can repair, inspect, or
extend their own machine instead of waiting on a service centre.

We are **not** affiliated with Ola Electric. We do not distribute firmware,
keys, certificates, or anything that lets someone impersonate a vehicle or a
cloud account.

---

## Projects

| Repository | What it is |
|---|---|
| [**ola-s1-rea**](https://github.com/Ola-Reverse-Engineering-Community/ola-s1-rea) | The main lab notebook: root over IPv6, the 40-method gRPC schema, the CAN map, HMI notes, and a five-screen Android app |
| [**ola-dash-sdk**](https://github.com/abhi245y/ola-dash-sdk) | A replacement dash for the S1 Pro Gen 1 (Android 10): a Slint home app (~70 MB RAM vs ~640 MB stock), runtime themes, display-only plugins, and a host-drawn safety overlay themes cannot hide. Talks to the stock vehicle service through clean-room interface files. Tested on Gen 1; the latest themes and the trunk button are not yet tested on a scooter |
| [**ola-can-reference**](https://github.com/abhi245y/ola-can-reference) | Gen 1 CAN catalog pinned to the HVDP 5.0.0 app: IDs, signals, enums, source locations, a decoder for supplied frames, and evidence templates. Read-only: it never transmits |
| [**ola-dev-skill**](https://github.com/abhi245y/ola-dev-skill) | An agent skill (Claude Code, Codex, Gemini CLI, Cursor) for building themes, plugins and tools against the two repositories above, with their safety rules built in |

More projects land here as contributors publish them.

---

## Start here

If you have a scooter and a laptop, the shortest path to something useful is
about ten minutes:

1. **Find the address.** The dash answers on IPv6, not IPv4 — the firewall rule
   that blocks port 50005 is `iptables`, so it never sees IPv6 traffic.
2. **Prove it.** One gRPC call returns `uid 0`. That is root, no password.
3. **Then decide what you want**: a root shell, Android Settings on the dash,
   live CAN readings, or real `adb` for `scrcpy`.

The step-by-step walkthrough, with every command written out, lives in the
[main repo's README](https://github.com/Ola-Reverse-Engineering-Community/ola-s1-rea#readme).

---

## What we publish

- **Reproducible procedures.** If a step needs a flag, a scope id, or a pushed
  `.sh` file, we say so — because the daemon splits on whitespace and `exec`s,
  and half the traps here are exactly that.
- **Proven results, marked as proven.** Anything untested is labelled untested.
  Owner-observed vehicle behaviour beats a guess from a decompiled string.
- **Schemas and maps.** The recovered `.proto`, the CAN IDs and their formulas,
  the app list, the boot and sleep behaviour.
- **Tools you can read.** Small Python and Java, no binaries, no codegen
  required.

## What we never publish

- Private keys, tokens, certificates, OTA keys, or pairing material.
- Personal data: names, phone numbers, VINs, device IDs, UUIDs, home
  addresses, hotspot names or passphrases.
- A specific vehicle's identifiers, addresses, or readings.
- Firmware images, or anything that would let a third party take over someone
  else's scooter.

If you find a secret while researching: rotate it if you can, and leave it out
of your write-up. Say *"there is a key at path X"* — never the key.

---

## Ground rules

1. **Your own vehicle.** Not a friend's, not a rental, not one you found.
2. **No flashing, no partition writes.** Nothing documented here needs one.
3. **Know the way back** before you change anything persistent. `persist.*`
   properties survive a reboot; bind mounts do not.
4. **Do not send CAN frames you do not understand.** The bus moves the vehicle.
5. **Do not press the destructive buttons.** Firmware update, provisioning,
   certificate generation, `Deinit`. They change state or brick things.
6. **Do not restart `adbd` while `scrcpy` is connected.** You will lose the
   stream and sometimes the encoder.
7. **Write down the undo.** A finding without an undo is not finished.

---

## Contributing

- Open an issue first for anything that touches vehicle state, so we can talk
  about the blast radius before anyone runs it.
- Pull requests should keep the same style as the existing docs: short
  sentences, real commands, a table where a table helps, and no marketing.
- Say which unit and software version you tested on. "E2, 5.6.1" is a fact;
  "latest" is not.
- If you are correcting something in the docs, include the evidence. A
  screenshot, a command output, or an owner's confirmation all count.
- By contributing you agree to the [MIT licence](https://github.com/Ola-Reverse-Engineering-Community/ola-s1-rea/blob/main/LICENSE)
  and to the rule that no secrets or personal data go into a commit.

---

## Legal

Reverse engineering for interoperability, repair and security research is legal
in many places, and the rules differ by jurisdiction. Nothing here is legal
advice, and nothing here is a licence to access a vehicle you do not own.

Modifying your scooter can void your warranty, and can make it unsafe or
unrideable. You are responsible for what you do with this material. The
contributors are not.

Read the full [disclaimer](https://github.com/Ola-Reverse-Engineering-Community/ola-s1-rea/blob/main/DISCLAIMER.md).

---

*Ride safe. Measure twice. Keep the undo command in your clipboard.*
