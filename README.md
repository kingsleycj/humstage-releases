## HumStage Desktop

A desktop app (`apps/desktop`) that puts a room's pages where a browser cannot:
full screen on a projector plugged into the machine, with no cursor and no
sleep, and out to a vision mixer as NDI®, with transparency, for vMix, OBS,
ProPresenter, TriCaster or an NDI-to-HDMI box. It is a standalone app; OBS is
only what we test against.

An operator connects by a room's six-digit code, or by a group link
(`humstage.app/s/your-group`), which it follows from one session to the next.
Then they add outputs — Projector, Projector without side panel, Lower third or
Join corner, each on a screen or as NDI — and press Start all. **Test NDI**
sends a test picture, finds it on the network the way a mixer would, and checks
it came back right. The setup is saved, and with "Start the outputs when HumStage
Desktop opens" ticked a booth machine needs no one at the keyboard.

The installers carry the NDI runtime (`resources/ndi`, beside NDI's notices for
the open-source code in it), as NDI's SDK documentation asks of an app that uses
it, under a licence with NDI's clauses (humstage.app/desktop-licence). CI takes
it from the current SDK for every build. On Linux, NDI also needs avahi; the
`.deb` depends on its client libraries and brings `avahi-daemon`. Run from
source, the app uses the machine's own runtime. Each version's release notes
are its entry in `apps/desktop/CHANGELOG.md`; a tag with no entry is refused.

```bash
pnpm --filter @humstage/desktop start      # run it from source
pnpm --filter @humstage/desktop dist       # installers for this platform, into apps/desktop/release
pnpm --filter @humstage/desktop test       # unit tests
pnpm --filter @humstage/desktop outputs    # outputs, recovery and Test NDI end to end (display + NDI)
pnpm --filter @humstage/desktop app        # the whole app through its control window (needs a server on :3100)
```

The `pixels`, `ndi`, `sender` and `screen` scripts are the phase 1 loopbacks;
`soak` is the two-hour run. CI (`.github/workflows/desktop.yml`) runs them, builds
the Linux AppImage and `.deb` and the Windows installer, smoke tests each packaged
app. It keeps the installers as workflow artifacts only on a release tag (a day,
for the publish job) or a run started by hand from the Actions tab (three days):
a private repository has 500MB of artifact storage, and one run's installers are
about 400MB.

**The builds are unsigned.** On Windows, the first run shows "Windows protected
your PC": press **More info**, then **Run anyway**. Linux has no such step.
Signing waits for money or an eligible company (see the desktop plan, §8).

NDI® is a registered trademark of Vizrt NDI AB — <https://ndi.video>.
